---
title: Falsehoods Programmers Believe About ATProto
link: https://reiver.link/atproto/falsehoods-programmers-believe/
source: lobste-rs
published: 2026-11-22T16:37:12Z
updated: 2026-11-22T16:37:12Z
first_seen: 2026-11-25T10:22:41.845233465Z
authors:
- reiver.link via jacky
labels:
- api
summary: Comments
content: extracted
html: 2026-11-22-falsehoods-programmers-believe-about-atproto.html
---

In the year 2010, Patrick McKenzie wrote and published the article [“Falsehoods Programmers Believe About Names”](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/) — a list of reasonable-sounding assumptions about people's names that not only *were not true*, but also (if assumed) would *create bugs* and even *break software*. That article started a whole genre of *Falsehoods Programmers Believe* articles. This article (*"Falsehoods Programmers Believe About ATProto"*) is one of my contributions to that genre.

*Every* numbered statement below is *false*. Each one is followed by what is actually true — and where to read more.

These are not made up for effect. Each one is backed by the specification, or by a live request to real **ATProto** servers (made in November 2026).

## Handles

1.  **A user's handle never changes.**

    Handles can change. The account's **DID** is what is permanent — the accounts specification calls it (**DID**s) “unique and immutable”.

    My handle changed! My handle was first `reiver.bsky.social`, then became `reiver.myatproto.social`, and then later became `reiver.blacksky.team`. You can see that if you dig through the data at [plc.directory/did:plc:scewmn2pl3oz36mxme2b6czz/log/audit](https://plc.directory/did:plc:scewmn2pl3oz36mxme2b6czz/log/audit). (`did:plc:scewmn2pl3oz36mxme2b6czz` is my DID-PLC **DID**.)

2.  **So, a handle is a good database key for a user.**

    Use the **DID**. Store the handle only for display (cosmetic) purposes — and re-check what each **DID**'s handle is from time to time.

    But, you need to normalize the **DID** before using it. **DIDs** are URIs, and URI schemes are case-insensitive ([RFC 3986](https://www.rfc-editor.org/rfc/rfc3986#section-3.1)) — so `DID:plc:scewmn2pl3oz36mxme2b6czz` and `did:plc:scewmn2pl3oz36mxme2b6czz` are the same **DID**. ATProto requires the normalized (lowercase) form: normalize **DIDs** when they come from user input, store and compare only the normalized form, and reject non-normalized **DIDs** that arrive in protocol data.

3.  **When a user changes their handle, the first part stays the same — such as `reiver.bsky.social` → `reiver.myatproto.social` → `reiver.blacksky.team`.**

    A new handle does not have to have anything in common with the old one. `joeblow.bsky.social` → `me.example.com` → `bbqlover.net` → `ilikeicecream.com` would be fine. Changing a handle just means pointing the account at a different domain — `com.atproto.identity.updateHandle` takes any valid handle, and only “Verifies handle validity”.

    For example, Bluesky's own account (`did:plc:z72i7hdynmk6r22z27h6tvur`) started out as `bluesky-team.bsky.social`, and then became `bsky.app`. You can see that in its audit log at [plc.directory/did:plc:z72i7hdynmk6r22z27h6tvur/log/audit](https://plc.directory/did:plc:z72i7hdynmk6r22z27h6tvur/log/audit). (Another reason to key users by **DID**, and not by any part of their handle.)

4.  **You can trust a handle you saw five minutes ago.**

    A handle can change at any moment. The user can switch to a new handle. The domain's owner can point it at a different **DID** — or the domain can expire, and be registered by someone else. Or, its DNS record or `/.well-known/atproto-did` file can just disappear, and the handle stops resolving. The handle specification says “If the handle for a known DID is confirmed to no longer resolve, it should be marked as invalid.”

    So, a handle → **DID** mapping is only as good as when you last checked it. The specification recommends caching handle resolution “up to some lifetime”, and re-resolving “periodically”. Watch the firehose for `#identity` events, which announce handle changes. And, never use a cached handle to decide *who* someone is (for example, for permissions) — use the **DID**.

5.  **Every user has a handle.**

    An account might have no handle in its **DID document** (no `at://` entry in `alsoKnownAs`), or a handle that no longer resolves back to it. The **DID** specification says “an account whose DID document does not contain a valid and confirmed handle can still participate in the atproto ecosystem. Software should be careful to either not display any handle for such account, or obviously indicate that any handle associated with it is invalid.”

    In API responses, such an account's handle may show up as the special value `handle.invalid` — which “can not be used in most situations (search queries, API requests, etc).” And, the `handle` field on the firehose's `#identity` event is optional. So, your code needs a “no handle” case.

6.  **Every handle ends in `.bsky.social`.**

    Any Internet domain can be a handle. And, other **PDSes** hand out their own — the `blacksky.app` **PDS**, for example, offers `.blacksky.app`, `.myatproto.social`, `.cryptoanarchy.network`, and others.

7.  **Even if a user's handle is not a `.bsky.social` handle now, their *first* handle was.**

    A user's first handle is whatever they got when their account was created — on whichever **PDS** they signed up with. `com.atproto.server.createAccount` just takes a “Requested handle for the account”, and each **PDS** offers its own domains. So, someone who signed up on `blacksky.app` might start out as `alice.myatproto.social` — and never have had a `.bsky.social` handle at all.

    You can see this in a **DID**'s audit log on `plc.directory` (`https://plc.directory/<did>/log/audit`) — the very first operation (the one that created the **DID**) shows the first handle, and the first **PDS**. For example, the audit log for `did:plc:ub2k2k5zahuegnkkvxjk5y2w` ([plc.directory/did:plc:ub2k2k5zahuegnkkvxjk5y2w/log/audit](https://plc.directory/did:plc:ub2k2k5zahuegnkkvxjk5y2w/log/audit)) has just 1 operation — the one that created it, on November 13th, 2026. Its first handle was `shadowoftruth.myatproto.social`, and its first **PDS** was `blacksky.app`. It has never had a `.bsky.social` handle.

8.  **A handle is a third-level domain — like `alice.bsky.social`, `alice.blacksky.app`, or `alice.myatproto.social`.**

    A handle can have any number of segments, as long as there are at least 2. `example.com` is a valid handle — and so is `apple.banana.cherry.example.com`. The handle specification only says “there must be at least two segments” (so a bare top-level domain like `com` is not allowed), each segment can be 1 to 63 characters, and the whole handle can be at most 253 characters. Its own examples include `a.co`, `8.cn`, and `XX.LCS.MIT.EDU`; and the official test vectors include `john.test.bsky.app`, and a valid handle with 10 segments.

9.  **Handles are resolved with DNS.**

    With DNS (a `TXT` record at `_atproto.<handle>`) *or* HTTPS (`/.well-known/atproto-did`). You need to support both.

10. **If a DID document lists a handle, the account owns that handle.**

    Anyone can put any handle in their own **DID document**. The handle has to resolve *back* to the same **DID** — both directions must agree. Otherwise, show `handle.invalid`.

11. **`com.atproto.identity.resolveHandle` checks both directions for you.**

    Its own lexicon says it “Does not necessarily bi-directionally verify against the the DID document.” (See: [How-To Resolve a Handle to a DID](https://reiver.link/atproto/how-to/resolve-handle-to-did/).)

12. **Handles are case-sensitive — so keep the capitalization the user typed.**

    Handles are not case-sensitive. The handle specification says “Only normalized (lowercase) handles should be stored in records or used in outbound API calls. Applications should not preserve user-provided case information and attempt to display handles in anything other than lower-case.” Its example: “the handle input string `BlueskyWeb.xyz` should be normalized, stored, and displayed as `blueskyweb.xyz`.”

13. **The `@` is part of the handle.**

    It is just decoration, for display. The handle specification says “Handles may be prefixed with the "at" symbol (like `@user.example.com`) in user interfaces, but this is not a valid syntax for a handle in records, APIs, and other back-end contexts.” So, strip it from user input before resolving.

14. **On `example.com`'s own site, it is fine to show `@alice.example.com` as just `@alice`.**

    The handle specification says “Handles should generally not be truncated to local context. For example, the handle `@alice.example.com` should not be displayed as `@alice`, even in the local context of an `example.com` service.” (It also warns that very long handles “can result in similar issues when truncated at the start or end” — for example, a handle that starts with `paypal.com` being shown as `paypal.com…`.)

## DIDs

15. **Every DID is a `did:plc`.**

    `did:web` is supported too. Your code needs to handle both.

16. **A `did:web` can be any `did:web`.**

    In ATProto, “did:web is restricted in atproto to only full domains, not paths or port specification” (from the record key specification).

17. **Whatever `plc.directory` returns for a DID is that DID's document.**

    Check that the document's `id` matches the **DID** you asked for. (See: [How-To Resolve a DID to a PDS](https://reiver.link/atproto/how-to/resolve-did-to-pds/).)

18. **A `did:plc` is case-insensitive — so `DID:PLC:…` or `did:plc:Z72I7HDYNMK6R22Z27H6TVUR` is fine.**

    The `did:` and the method name (`plc`) must be lowercase — “The URI starts with lowercase did:”, and “The method segment is one or more lowercase letters”. And, a `did:plc` identifier is lowercase too: “the entire identifier is lower-case (and should be normalized to lower-case)” (from the `did:plc` specification). The ATProto specification says “protocol implementations should reject DIDs with invalid casing.” (`plc.directory` answered `did:plc:Z72I7HDYNMK6R22Z27H6TVUR`, `did:PLC:…`, and `DID:PLC:…` with `400` “Resolution of non-PLC DIDs not supported”.)

    (This is about **DIDs** in *protocol data* — records, tokens, API responses. As a URI, the `did:` scheme is case-insensitive, so `DID:` means the same thing — but ATProto requires the normalized, lowercase form there. **DIDs** typed in by a user may be normalized; see item 2. And, even under the general **DID** syntax, the method name must be lowercase, so `did:PLC:…` is invalid on its own.)

19. **DIDs are case-sensitive — so `did:plc:abc…` and `did:plc:ABC…` are 2 different accounts.**

    For `did:plc`, the uppercase one is not a different **DID** — it is not a valid `did:plc` at all, since its identifier is lowercase-only base32. The ATProto specification says “the currently-supported methods are not case sensitive, and could be safely lowercased”, and that “It is permissible to attempt case normalization when receiving user-controlled input, such as when parsing public URL path components, or text input fields.” So: reject bad casing in protocol data; lowercasing what a user typed is fine.

20. **If a DID parser accepts it, it is a valid DID.**

    A generic **DID** parser only checks the generic syntax — which allows uppercase in the identifier (the test vectors list `did:method:VAL` as valid). The [indigo](https://github.com/bluesky-social/indigo) library's `syntax.ParseDID` accepts `did:plc:Z72I7HDYNMK6R22Z27H6TVUR` — but `plc.directory` rejects it. Generic syntax is not the same as a valid `did:plc`.

21. **A `did:plc`'s DID document never changes.**

    The **DID** never changes — but its **DID document** can. Each change is a signed *operation*, and the handle (`alsoKnownAs`), the **PDS** (the `#atproto_pds` service), the signing key (`#atproto`), and the rotation keys can all be changed.

    For example, my **DID**'s audit log ([plc.directory/did:plc:scewmn2pl3oz36mxme2b6czz/log/audit](https://plc.directory/did:plc:scewmn2pl3oz36mxme2b6czz/log/audit)) has 4 operations:

    | Date       | Handle                    | PDS                                   | What Changed             |
    | ---------- | ------------------------- | ------------------------------------- | ------------------------ |
    | 2023-04-18 | `reiver.bsky.social`      | `bsky.social`                         | (created)                |
    | 2023-11-13 | `reiver.bsky.social`      | `lionsmane.us-east.host.bsky.network` | PDS, signing key         |
    | 2026-10-23 | `reiver.myatproto.social` | `blacksky.app`                        | handle, PDS, signing key |
    | 2026-11-06 | `reiver.blacksky.team`    | `blacksky.app`                        | handle                   |

    And, the `did:plc` specification has 2 more surprises. A “tombstone” operation “clears all of the data fields and permanently deactivates the DID”. And, there is a 72-hour window during which a higher-authority rotation key can “"rewrite" history”. So, do not cache a **DID document** forever — re-resolve it from time to time, and watch the firehose for `#identity` events.

## Personal Data Servers

22. **“Personal” Data Server means one server per person.**

    “Many PDSes will host multiple users” (the glossary). Most host many.

23. **Bluesky users' data is on `bsky.social`.**

    Bluesky's accounts are spread across many **PDS** hosts — for example, `@bsky.app` is on `puffball.us-east.host.bsky.network`. `bsky.social` is where they *log in*.

24. **The server a user logs in at is the server that has their data.**

    It might be an *entryway* — a separate OAuth server for many **PDSes**. `puffball.us-east.host.bsky.network`'s `/.well-known/oauth-protected-resource` lists `https://bsky.social` as its authorization server. (`blacksky.app`, on the other hand, lists itself.)

25. **A user's PDS never changes.**

    Accounts can migrate to another **PDS** — that is a core design goal. Always look up the current **PDS** from the **DID document** (and do not cache it forever).

26. **A user's PDS can show them everyone's posts.**

    A **PDS** only has its own accounts' data. For timelines, threads, and counts, it forwards the request to an **AppView**. (See: [ATProto PDS](https://reiver.link/atproto/service/pds/).)

27. **A user's PDS builds (or renders) their timeline.**

    The timeline (`app.bsky.feed.getTimeline`) is built by an **[AppView](https://reiver.link/atproto/service/appview/)**. The **PDS** just forwards the app's request to it. And, no server *renders* it — the **AppView** returns JSON, and the app (on the user's phone or computer) renders that.

    (This one used to be closer to true. Bluesky's feed generator starter kit still says “In the future, the PDS will hydrate the feed with the help of an App View, but for now, the PDS handles hydration itself.” Today, the **AppView** does it. See: [ATProto Feed Generator](https://reiver.link/atproto/service/feed-generator/).)

28. **Bluesky's PDS software is the only PDS software.**

    Any server that implements the right end-points is a **PDS**. (For example, the Blacksky team's [rsky](https://github.com/blacksky-algorithms/rsky) project, written in Rust, includes `rsky-pds`.)

29. **Self-hosting a PDS means you have to index the entire network.**

    A **PDS** only stores its *own* accounts' data. Indexing the network is the job of **[relays](https://reiver.link/atproto/service/relay/)** and **[AppViews](https://reiver.link/atproto/service/appview/)** — a self-hosted **PDS** uses someone else's. (In Bluesky's **PDS** software, `PDS_CRAWLERS` defaults to `https://bsky.network`, and `PDS_BSKY_APP_VIEW_URL` defaults to `https://api.bsky.app`.)

    That is why the hardware is small. The [official **PDS** instructions](https://github.com/bluesky-social/pds) recommend 1 GB of memory, 1 CPU core, and a 20 GB SSD — for 1 to 20 users. (See: [ATProto PDS](https://reiver.link/atproto/service/pds/).)

## Records and Repositories

30. **Every record on the network is a Bluesky post.**

    A repository can hold records of *any* type — any **[lexicon](https://reiver.link/atproto/lexicon/)**, from any app — all mixed together in the same user's repository. Calendar events, bookmarks, blog posts, game state, workout logs, or a custom app's own schema: each is just another collection. (And, by default, a **PDS** accepts records whose lexicon it does not know — “optimistic validation”.)

    For example, my repository (`reiver.blacksky.team`) has these collections, according to `com.atproto.repo.describeRepo`:

    ```
    actor.rpg.generator
    actor.rpg.sprite
    actor.rpg.stats
    app.bsky.actor.link
    app.bsky.actor.profile
    app.bsky.feed.generator
    app.bsky.feed.like
    app.bsky.feed.post
    app.bsky.feed.repost
    app.bsky.graph.follow
    app.bsky.graph.list
    app.bsky.graph.listitem
    chat.bsky.actor.declaration
    equipment.rpg.item

    ```

    Only one of those 14 collections is Bluesky posts (`app.bsky.feed.post`). Even among Bluesky's own record types, most are not posts — likes, reposts, follows, lists, a profile. And, `actor.rpg.*` and `equipment.rpg.item` are from a different app altogether. (See: [ATProto Record](https://reiver.link/atproto/record/).)

31. **Everything a user does on ATProto is public.**

    Everything in a user's *repository* is public — posts, likes, follows, and even blocks (see the next item). But, not everything a user does is stored as a record in their repository. For example, in Bluesky:

    - **preferences** — `app.bsky.actor.putPreferences` says it will “Set the private preferences attached to the account.”
    - **mutes** — `app.bsky.graph.muteActor` says “Mutes are private in Bluesky.”
    - **direct messages** — `chat.bsky.convo.sendMessage` sends them to a separate chat service (`did:web:api.bsky.chat`), not into anyone's repository.
    - **account details** — email address, password, and app passwords stay on the user's **[PDS](https://reiver.link/atproto/service/pds/)**.

    Note that this data is private from the *public* — not encrypted. The **PDS** (or chat service) that holds it can read it. And, these are features of particular apps and services (`app.bsky.*`, `chat.bsky.*`), not a privacy guarantee of the protocol itself.

    (The **[spaces](https://reiver.link/atproto/space/)** proposal would add something new: *records* that only some people may read — see the Spaces section, below.)

32. **Blocks are private, like mutes.**

    In Bluesky, a mute is private — but a block is a public record. A block is an `app.bsky.graph.block` record in the *blocker's* repository, and its lexicon says so plainly: “Record declaring a 'block' relationship against another account. NOTE: blocks are public in Bluesky; see blog posts for details.”

    So, anyone can list an account's blocks — for example, with `com.atproto.repo.listRecords`, or from the firehose — and see who blocked whom. If you build features on blocks, treat them as public information.

33. **A record has an author field.**

    The author is the **repository** the record is in. (See: [ATProto Record](https://reiver.link/atproto/record/).)

34. **A record key is unique within a repository.**

    “The tuple of (did, rkey) is not unique; the tuple (did, collection, rkey) is unique.”

35. **Record keys are always TIDs.**

    TIDs (like `3n2opitxdyc2j`) are the most common, but each record type's **[lexicon](https://reiver.link/atproto/lexicon/)** says which kind of record key it uses. The record key specification defines 4 kinds:

    - `tid` — a timestamp identifier (posts, likes, follows, etc),
    - `literal:<value>` — one fixed key, so there is only one record in the collection; usually `literal:self` (for example, `app.bsky.actor.profile/self`),
    - `nsid` — the key must be an **NSID** (published lexicons use the **NSID** as the record key), and
    - `any` — any valid record key, such as a domain name, or a number.

    Some other things about record keys:

    - They have their own syntax: 1 to 512 characters, using only letters, digits, and `.` `-` `_` `:` `~` — and not `.` or `..`.
    - They are case-sensitive (though “it is a recommended practice to use all-lower-case record keys”).
    - The same TID can be used in 2 different collections, to show the records are related (a “"sidecar" extension record”).
    - Software should treat record keys as plain strings — the specification discourages, for example, decoding TIDs into numbers for use as a database key.
    - More kinds of record key may be added in the future.

    (See: [ATProto Record](https://reiver.link/atproto/record/).)

36. **A `tid` record key tells you when the record was created.**

    “Record keys are "user-controlled data" and may be arbitrarily selected by hostile accounts.” Do not trust them as timestamps.

37. **A record's `createdAt` is when it was created.**

    It is whatever the client put there. The `app.bsky.feed.post` lexicon describes it as a “Client-declared timestamp”. A client can set it to any time — past or future.

    When a record was actually seen is tracked separately, by the services that see it:

    - the firehose's `#commit` event has a `time` — “Timestamp of when this message was originally broadcast”,
    - the commit's `rev` is a TID, which a **PDS** “should use the current wall time” to make (from the sync specification), and
    - an **[AppView](https://reiver.link/atproto/service/appview/)**'s post view has an `indexedAt` — when *that* **AppView** indexed it.

    For example, for one of `@bsky.app`'s posts, `public.api.bsky.app` reported `createdAt` as `2026-11-19T18:33:36.813Z`, and `indexedAt` as `2026-11-19T18:33:38.543Z` — close, here, but nothing requires them to be. (Even these are each one service's view — a different **AppView** may have indexed it at a different time.)

38. **ATProto JSON is just regular JSON.**

    It is JSON with extra rules, from the **ATProto** data model — the same data model that is also written as DRISL-CBOR (the successor to DAG-CBOR) for storing, signing, and hashing. The differences from regular JSON:

    - **no floating-point numbers** — only integers, and those are best kept to 53 bits (between `-9007199254740991` and `9007199254740991`), for JavaScript,
    - **links** are written as `{"$link": "<CID>"}`,
    - **bytes** are written as `{"$bytes": "<base64>"}` (plain base64, not URL-safe; padding optional),
    - **blobs** are written as `{"$type": "blob", "ref": {"$link": …}, "mimeType": …, "size": …}`,
    - **field names starting with `$` are reserved** for the protocol (`$type`, `$link`, `$bytes`) — and unknown `$` fields should be ignored,
    - **empty field names are not allowed**,
    - **strings must be valid UTF-8**,
    - **missing, `null`, and “false-y” are 3 different things**, and `null` is only allowed where the lexicon says `nullable`, and
    - **it is not byte-exact** — key order and spacing do not matter, so it cannot be hashed directly; the DRISL-CBOR form is what gets hashed and signed.

    It is not DAG-JSON either — the data model specification says **ATProto** uses `$link` “as the key name instead of `/`”. And, there are practical limits too — for example, Bluesky's data validation guide suggests at most 32 levels of nesting, and 1 MiB per record (as CBOR). (See: [ATProto Record](https://reiver.link/atproto/record/).)

39. **Records can contain floating-point numbers.**

    Floats are not allowed in the data model — they do not round-trip reliably, and the bytes must be exact for hashing.

40. **You can compute a record's CID by hashing its JSON.**

    The CID is a hash of its DRISL-CBOR encoding. JSON is not byte-exact.

41. **A missing field and a `null` field mean the same thing.**

    They are different — and both are different from `false`, `0`, or an empty array.

42. **Every record in a repository is valid against its lexicon.**

    By default, a **PDS** only checks records whose lexicon it knows (“optimistic validation”, or “fail-open”). Validate what you read.

43. **A record with fields its lexicon does not mention is invalid.**

    Unexpected fields should be ignored — “at worst as warnings”. (Validating a post with an extra `mood` field against `app.bsky.feed.post` passes. See: [ATProto Lexicon](https://reiver.link/atproto/lexicon/).)

44. **A deleted record leaves a tombstone.**

    “Record deletion is supported without leaving a trace or "tombstone" of previous contents.”

45. **Deleting a record removes it from the Internet.**

    Deleting a record removes it from the user's repository. But, by then, copies may already be elsewhere — the creation went out on the **PDS**'s firehose, and **[relays](https://reiver.link/atproto/service/relay/)**, **[AppViews](https://reiver.link/atproto/service/appview/)**, feed generators, search indexes, backups, and anyone else watching may have kept it.

    The deletion goes out on the firehose too, and the specifications *expect* other services to follow it. The accounts specification says “When accounts remove records from their public repositories, hosting services should remove public access to that content.” And, the sync specification says mirrors should “respect repository updates (eg, record deletion) … in a timely manner (within seconds or minutes)”. But, “should” is not a guarantee — nothing forces every service to comply, and a copy someone else kept (or a screenshot) is out of the user's control.

    (And, since the data was signed, such a copy can still be shown to be genuine. Treat anything written to a public repository as published.)

## NSIDs and Lexicons

46. **An NSID is an address you can fetch.**

    It is a *name* — a reversed Internet domain plus a name. (See: [ATProto NSID](https://reiver.link/atproto/nsid/).)

47. **It is safe to lowercase an NSID.**

    The Internet domain part is not case-sensitive, but the name is — `getRecord` is not `getrecord`. “Implementations should not force-lowercase NSIDs.”

48. **Implementations reject an NSID whose Internet domain part is longer than 253 characters.**

    The specification says 253 — but the official test vectors list a 283-character one as valid, and Bluesky's TypeScript and Go code only check the 317-character total.

49. **If `_lexicon.feed.bsky.app` does not exist, try `_lexicon.bsky.app`.**

    Lexicon resolution is not hierarchical. If the exact record does not exist, resolution simply fails. (See: [How-To Resolve a Lexicon](https://reiver.link/atproto/how-to/resolve-lexicon/).)

50. **The `app.bsky.*` lexicons are in the `bluesky-social/atproto` repository on GitHub.**

    Not anymore — its main branch has no `lexicons/app/bsky/` directory. Resolve them from the network instead (they are published by `did:plc:4v4y5r3lwsbtmsxhile2ljac`).

51. **A union in a lexicon lists every type you will ever see there.**

    Unions are *open* by default — new types can be added. (The live `app.bsky.feed.post` lexicon's `embed` union includes `app.bsky.embed.gallery`, which older code will not know about.)

52. **A lexicon never changes.**

    It can change — in compatible ways (new optional fields, more union types, etc).

## XRPC

53. **You need a special library to talk ATProto.**

    **XRPC** is plain HTTP and JSON. `curl` works. (See: [ATProto XRPC](https://reiver.link/atproto/xrpc/).)

54. **A server returns the error names listed in the end-point's lexicon.**

    Not always. `resolveHandle`'s lexicon lists `HandleNotFound`, but `public.api.bsky.app` answered an unknown handle with `InvalidRequest`.

55. **Errors are always JSON.**

    The specification itself warns that “many services will return generic load-balancer or reverse-proxy HTML error pages.”

56. **An end-point a server does not have returns `404`.**

    A Bluesky **PDS** answered a made-up end-point with `401 AuthMissing`.

57. **A cursor means something you can use.**

    Treat it as opaque. (One server's `listRecords` cursor happens to be the last record key — do not depend on that.)

## The Network

58. **There is one firehose.**

    Every **PDS** has its own firehose, and there are several **relays** — for example, Bluesky's `bsky.network` and the Blacksky team's `atproto.africa`. (See: [ATProto Relay](https://reiver.link/atproto/service/relay/).)

59. **A user's PDS knows who follows them.**

    A follow is an `app.bsky.graph.follow` record — stored in the *follower's* repository, with the followed account's **DID** as its `subject`. So, a user's **PDS** knows exactly who the user *follows*, but not who follows *them* — those records are spread across everyone else's repositories, on every **PDS**. (See: [How-To Count an ATProto Account's Followers](https://reiver.link/atproto/how-to/follower-count/).)

60. **Likes and replies live on the author's post.**

    They live in the repositories of the people who liked or replied. A like is an `app.bsky.feed.like` record in the *liker's* repository, whose `subject` is a strong reference (`uri` and `cid`) to the post. A reply is an `app.bsky.feed.post` record in the *replier's* repository, with a `reply` field pointing at the thread's `root` and the `parent` post. (Reposts work the same way.) The author's post record itself has no list of likes or replies, and no counts.

    So, like and reply counts — and threads — are put together by services that index the network, like an **[AppView](https://reiver.link/atproto/service/appview/)** or a backlink index. For example, for one of `@bsky.app`'s posts (`app.bsky.feed.post/3n2opitxdyc2j`), on November 22nd, 2026, `public.api.bsky.app` said 1951 likes and 774 replies — and so did the Constellation backlink index. (For a recent post, they agree. For older ones, they may not — see the follower counts, below.)

61. **A follower count is a fact.**

    It is one service's opinion. For the same account (`reiver.blacksky.team`), at the same time (November 22nd, 2026), `public.api.bsky.app` (Bluesky's **AppView**) said 292 followers, `api.blacksky.community` (the Blacksky team's **AppView**) said 196, and the Constellation backlink index said 134. (See: [ATProto AppView](https://reiver.link/atproto/service/appview/) and [How-To Count an ATProto Account's Followers](https://reiver.link/atproto/how-to/follower-count/).)

62. **With enough work, a follower count can be computed exactly.**

    You would have to check every repository on every **PDS** — and you can never be sure you found them all, since a **relay** only knows the hosts it crawls. By the time you finished, the answer would have changed. And, what counts as a follower is a choice — for example, the `app.bsky.graph.follow` lexicon says “Duplicate follows will be ignored by the AppView.” (See: [How-To Count an ATProto Account's Followers](https://reiver.link/atproto/how-to/follower-count/).)

63. **A feed generator sends you posts.**

    It sends a *skeleton* — a list of post URIs. The **AppView** fills in the posts. (See: [ATProto Feed Generator](https://reiver.link/atproto/service/feed-generator/).)

64. **A label deletes content.**

    A label is a signed annotation. What to do with it (hide, blur, warn, nothing) is up to each app and user. (See: [ATProto Labeler](https://reiver.link/atproto/service/labeler/).)

65. **Account status (active, deactivated, etc) can be verified cryptographically.**

    “Unlike other aspects of atproto, account status is not self-certifying”. It is passed along hop by hop — so different services may disagree. When in doubt, ask the account's own **PDS** (`com.atproto.sync.getRepoStatus`).

66. **ATProto is Bluesky.**

    Bluesky is one app built on it. Others use their own lexicons — for example, `site.standard.document`, `community.lexicon.calendar.event`, and `actor.rpg.sprite`.

67. **ATProto is a microblogging protocol.**

    Microblogging (short posts, like Bluesky's) is just one thing built on it. The overview describes **ATProto** as “a decentralized protocol for large-scale social web applications”. Nothing in the core protocol is about posts — records, repositories, **XRPC**, and sync work the same for any record type. What a record *is* comes from its **[lexicon](https://reiver.link/atproto/lexicon/)**, and anyone with a domain can define new ones.

    For example: long-form documents (`site.standard.document`), calendar events (`community.lexicon.calendar.event`), game sprites (`actor.rpg.sprite`), custom feeds (`app.bsky.feed.generator`), and, in the **[spaces](https://reiver.link/atproto/space/)** proposal, bookmarks, private forums, and group chats. Even the protocol's own **lexicons** are published as records (`com.atproto.lexicon.schema`).

## Compared to ActivityPub, the Fediverse, and Mastodon

(Falsehoods that come from comparing **ATProto** with [ActivityPub](https://reiver.link/activitypub/), the [Fediverse](https://reiver.link/fediverse/), and Mastodon.)

68. **Unlike ActivityPub (and the Fediverse, and Mastodon), ATProto does not use JSON-LD.**

    **DID documents** are [JSON-LD](https://reiver.link/json-ld/). `plc.directory` serves them with the content type `application/did+ld+json`, and each one starts with an `@context`:

    ```
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/multikey/v1",
      "https://w3id.org/security/suites/secp256k1-2019/v1"
    ]

    ```

    (That is from `plc.directory/did:plc:scewmn2pl3oz36mxme2b6czz`. A `did:web` document has one too — for example, `api.bsky.app/.well-known/did.json`.)

    What is true is that **ATProto**'s *own* data — records, **lexicons**, **XRPC** — is not JSON-LD. And, **ATProto** software usually reads **DID documents** as plain JSON — the **ATProto** **DID** specification says which fields to read, and does not call for JSON-LD processing.

69. **Like on Mastodon, an ATProto handle includes the user's server — `@user@server`.**

    On Mastodon, a user's full address is a user name *and* a server — Mastodon's documentation says it is “in the form `@username@domain`”, and that “`@user@example.com` is not the same as `@user@example.org`.”

    An **ATProto** handle is just one domain name — like `reiver.blacksky.team` — and it says nothing about the user's server. The server (**PDS**) is found separately, from the **DID document**. (A handle shown as `@reiver.blacksky.team` has just one `@`, as a decoration.)

70. **ATProto has nothing like a Fediverse “instance”.**

    It does — the **[Personal Data Server](https://reiver.link/atproto/service/pds/)** (**PDS**). Like an instance, a **PDS** hosts many users' accounts (“Many PDSes will host multiple users”, says the glossary), is run by someone, decides who can sign up (for example, with invite codes), has its own terms of service and privacy policy (listed in `com.atproto.server.describeServer`), and can take accounts down (the `takendown` account status).

    The difference is that it is more hidden. It is not part of the user's identity or handle — `reiver.blacksky.team` does not say which **PDS** hosts it. It is not what decides what a user sees — that is mostly the **[AppView](https://reiver.link/atproto/service/appview/)**. And, a user can move to another **PDS** while keeping the same **DID**, handle, posts, and followers.

71. **Like Mastodon, ATProto looks up handles with WebFinger.**

    Mastodon uses WebFinger (`/.well-known/webfinger`) — its documentation says “WebFinger is required for fully interoperating with Mastodon.”

    **ATProto** does not. A handle is resolved with a DNS `TXT` record at `_atproto.<handle>`, or with `https://<handle>/.well-known/atproto-did`. (See: [How-To Resolve a Handle to a DID](https://reiver.link/atproto/how-to/resolve-handle-to-did/).)

72. **Like on Mastodon, when a user moves to a new server, their posts stay behind, and their followers have to be moved over.**

    On Mastodon, moving an account moves followers — but Mastodon's documentation says “Your posts will not be moved, due to technical limitations”, and that “Mastodon currently does not support importing posts or media”. Moving also has to “force everyone to unfollow your current account and follow your new account”.

    On **ATProto**, moving to a new **PDS** includes “migrating data from the old PDS to the new PDS” (from the accounts specification) — the whole repository, posts and all. And, the account keeps the same **DID**. Since follow records point at the **DID**, followers do not have to be moved at all. (See: [ATProto PDS](https://reiver.link/atproto/service/pds/).)

73. **Like ActivityPub servers, ATProto servers send posts to each other's inboxes.**

    In ActivityPub, a server *pushes* each activity to its recipients — the ActivityPub specification describes it being “inserted into the actor's inbox (delivery)”, by HTTP `POST`.

    In **ATProto**, there are no inboxes. A **PDS** publishes every change on its own firehose, and services that want the data — **[relays](https://reiver.link/atproto/service/relay/)**, **[AppViews](https://reiver.link/atproto/service/appview/)**, etc — subscribe and pull it. The **PDS** does not need to know who its readers are.

74. **Like in ActivityPub, to trust a copy of a post, you have to check it against the server it came from.**

    The ActivityPub specification says servers should do “something at least as robust as checking that the object appears as received at its origin”, and that “No particular mechanism for verification is authoritatively specified”. (Mastodon uses HTTP Signatures, to check who *sent* an activity.)

    **ATProto** data checks itself. Repository commits are signed with the account's key, and records are identified by their hash (CID) — so a copy can be checked no matter who it came from. The data model specification says this “makes it possible to redistribute content and trust copies even if coming from an untrusted party.” (See: [ATProto Record](https://reiver.link/atproto/record/).)

75. **ATProto and the Fediverse cannot talk to each other.**

    They are different protocols — but there are bridges. For example, [Bridgy Fed](https://fed.brid.gy/docs) describes itself as “a decentralized social network bridge. It connects the fediverse, the web, and the Atmosphere (Bluesky).” With it, people on one network can follow, reply to, like, and repost people on the other.

## Spaces (Permissioned Data)

(These are about the **[spaces](https://reiver.link/atproto/space/)** proposal, which is not yet part of the specification.)

76. **Space data is encrypted.**

    “Atproto spaces provide access control, not confidentiality. They are not end-to-end encrypted.”

77. **A space is stored on one server.**

    Each member's records stay on that member's own **PDS**. “The space does not colocate records on a single network host.”

78. **A copy of space data proves who wrote it.**

    Space data is authenticated with MACs, not signatures — on purpose, so that it “should not carry proof of authorship that survives a leak outside its access boundary.”

## Why These Matter

Most of these come from the same idea: **ATProto** separates things that are usually tied together.

- Identity (**DID**) is separate from name (handle) — and from server (**PDS**).
- Data (**records**) is separate from views of the data (**AppViews**, **feed generators**).
- Schemas (**lexicons**) are separate from any one server, and can evolve.
- And, every service decides for itself what to trust and what to show.

So, the safe defaults are: key everything by **DID**; resolve, do not assume; validate what you read; ignore what you do not understand; and expect other servers to disagree with yours.

## Sources

- Patrick McKenzie, [“Falsehoods Programmers Believe About Names”](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/) (June 17th, 2010) — the original
- [AT Protocol: Accounts](https://atproto.com/specs/account) — immutable DIDs, account status
- [AT Protocol: Handle](https://atproto.com/specs/handle) — handle syntax
- [AT Protocol: DID](https://atproto.com/specs/did) — DID syntax and casing
- [`did:plc` method specification](https://github.com/did-method-plc/did-method-plc) (v0.3.0, December 2025) — identifier syntax
- [AT Protocol: Record Key](https://atproto.com/specs/record-key)
- [AT Protocol: Repository](https://atproto.com/specs/repository)
- [AT Protocol: Sync](https://atproto.com/specs/sync) — mirrors and deletion
- [AT Protocol: Data Model](https://atproto.com/specs/data-model)
- [AT Protocol: Data Validation](https://atproto.com/guides/data-validation) (guide) — UTF-8, empty keys, 53-bit integers, size and nesting limits
- [AT Protocol: Lexicon](https://atproto.com/specs/lexicon)
- [AT Protocol: Namespaced Identifiers (NSIDs)](https://atproto.com/specs/nsid) and [atproto-interop-tests](https://github.com/bluesky-social/atproto-interop-tests) (NSID, DID, and handle syntax vectors)
- [AT Protocol: HTTP API (XRPC)](https://atproto.com/specs/xrpc)
- [AT Protocol: OAuth](https://atproto.com/specs/oauth) — entryways
- [AT Protocol: Glossary](https://atproto.com/guides/glossary)
- [blacksky-algorithms/rsky](https://github.com/blacksky-algorithms/rsky) — includes `rsky-pds`
- [bluesky-social/pds](https://github.com/bluesky-social/pds) — README (server recommendations, environment variables)
- [Bluesky Proposal 0016: Atproto Spaces](https://github.com/bluesky-social/proposals/blob/main/0016-permissioned-data/README.md)
- Mastodon documentation — [WebFinger](https://docs.joinmastodon.org/spec/webfinger/), [Moving or leaving accounts](https://docs.joinmastodon.org/user/moving/), and [Security](https://docs.joinmastodon.org/spec/security/) (HTTP Signatures)
- [W3C ActivityPub](https://www.w3.org/TR/activitypub/) — delivery, and verification
- [Bridgy Fed documentation](https://fed.brid.gy/docs)
- Live requests made while writing the other articles on this site (November 2026) — see each linked article's Sources
