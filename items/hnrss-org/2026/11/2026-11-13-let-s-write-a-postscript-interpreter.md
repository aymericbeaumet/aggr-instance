---
title: Let's Write a PostScript Interpreter
link: https://beyondloom.com/blog/postscript.html
source: hnrss-org
published: 2026-11-13T05:12:56Z
updated: 2026-11-13T05:12:56Z
first_seen: 2026-11-15T00:13:30.261415141Z
authors:
- RebelPotato
content: extracted
html: 2026-11-13-let-s-write-a-postscript-interpreter.html
---

I think the most trustworthy programming languages are those which are *human-scale*: languages which a single user could reasonably write from scratch on some readily-available computational substrate. Knowing how to make your own tools provides a satisfying [feeling of power](https://en.wikipedia.org/wiki/The_Feeling_of_Power).

Today I’d like to take you on a brisk journey through the process of implementing an interesting subset of the [PostScript](https://en.wikipedia.org/wiki/PostScript) programming language. We will ignore or simplify many features of the language, and we will elide most error-handling for simplicity, but every PostScript example we discuss will be a well-formed PostScript program which has the same behavior in our toy subset and a standard PostScript interpreter like [Ghostscript](https://en.wikipedia.org/wiki/Ghostscript).

## The Elements of PostScript

PostScript is primarily designed for describing printed documents: it has a rich vocabulary of features for creating and manipulating raster and vector graphics. For the purposes of this article I will narrowly focus on its mechanics as a general-purpose programming language.

PostScript programs are composed of a series of whitespace-delimited tokens, plus a few special characters (`% / ( ) < > [ ] { }`) which immediately terminate the preceding token and begin a new one. The `%` character begins a line-comment. The `/` character can prefix a *literal name*, which we’ll discuss in more detail momentarily. The `( )` and `< >` characters enclose PostScript *strings*, which we will omit from our interpreter for simplicity. The `[`, `]`, `{` and `}` characters are used to enclose PostScript *arrays* and *procedures*, respectively, but are ordinary self-delimiting tokens. Given a PostScript program fragment:

```
% This is a line comment
/double {2 mul} def

3 double
```

The tokens are:

```
['/double','{','2','mul','}','def','3','double']
```

The PostScript execution model centers around several *stacks*: chiefly, an *operand stack* which is used to pass arguments between operators and a *dictionary stack* which is used to associate names with values.

The above program fragment is executed as follows:

1. The name `/double` begins with a `/` prefix, so it is pushed onto the operand stack.
2. The token `{` collects the tokens up to the matching `}` into a *procedure* without executing them, and the resulting procedure `{2 mul}` is pushed onto the operand stack.
3. The name `def` does not begin with a `/` prefix, so it is looked up in the dictionary stack, starting with the topmost dictionary and searching deeper dictionaries until a match is found. This name is associated with an *operator*\- a primitive provided by the language. The `def` operator pops a procedure from the operand stack (`{2 mul}`) as well as a name (`/double`) and associates the name with the procedure in the topmost dictionary on the dictionary stack.
4. The number `3` is pushed onto the operand stack.
5. The name `double` does not begin with a `/` prefix, so it is looked up in the dictionary stack. This retrieves the procedure we previously associated with the name. When a procedure is retrieved from the dictionary stack in this manner, its contents (`2 mul`) are executed in sequence.
6. The number `2` is pushed onto the operand stack.
7. The name `mul` does not begin with a `/` prefix, so it is looked up in the dictionary stack. The `mul` operator pops two values from the operand stack- `2` and `3`\- multiplies them, and then pushes the result- `6`\- onto the operand stack.

It’s also worth taking a moment to touch on PostScript *arrays*. The `[` operator pushes a special value called `mark` onto the operand stack, and the `]` operator pops all of the values of the operand stack down to the topmost `mark` and builds an array from them:

```
[]             % an empty array
[1 2]          % an array of two values
[[1 2][3 4]]   % an array of two values, each of which is an array
[2 3 add]      % equivalent to [5]
```

We’ll discuss programming in PostScript in more detail later, but this is enough to get building.

## Code and Data

Our PostScript interpreter will be written in a very concise style of JavaScript that is influenced by the philosophy of [Forth](https://en.wikipedia.org/wiki/Forth_\(programming_language\)), an influential ancestor of PostScript:

- We will *factor* our program apart into many short routines, generally single lines.
- We will prefer routines which are “pure” functions without side effects, but we will use global state when it simplifies our code.
- Each routine should express a single idea, and contain no more than one control structure.

Here are a few routines which illustrate this style:

```
last = x=>x[x.length-1]
iota = x=>Array.from(Array(x)).map((_,i)=>i)
mod  = (x,y)=>x-y*Math.floor(x/y)
```

If you’re finding yourself struggling to “skim” code in this style, try to slow down and read each definition carefully, character-by-character. You may also find it useful to experiment with each definition in your browser’s JavaScript console as you go along.

The first step in writing any interpreter is determining how we’ll represent data, in this case describing PostScript datatypes in terms of JavaScript datatypes:

| PostScript | Example     | JavaScript           | Example                  |
| ---------- | ----------- | -------------------- | ------------------------ |
| number     | `37`        | Number               | `37`                     |
| boolean    | `true`      | Boolean              | `true`                   |
| name       | `a`         | String               | `"a"`                    |
| literal    | `/a`        | String               | `"/a"`                   |
| dictionary | `<</a 11>>` | Map                  | `new Map([['/a',11]])`   |
| array      | `[11 22]`   | Array                | `[11,22]`                |
| procedure  | `{1 add}`   | Object-wrapped Array | `{t:'proc',v:[1,'add']}` |
| operator   | `add`       | Function             | `(x,y)=>x+y`             |
| mark       | `mark`      | Singleton Object     | `MARK`                   |

Using primitive JavaScript values to represent most of these types will be particularly helpful for writing the definitions of all the operators.

Here are a set of definitions for constructing, accessing, and identifying various PostScript values:

```
MARK    = {}
proc    = x=>({t:'proc',v:x})
procval = x=>x.v
isproc  = x=>x&&x.t=='proc'
isarray = x=>Array.isArray(x)
isfunc  = x=>'function'==typeof x
isname  = x=>'string'==typeof x&&`/`!=x[0]
litval  = x=>x.slice('/'==x[0])
```

Next, we need to translate PostScript syntax into the above data representations. We can use a JavaScript RegExp to split an input program into a sequence of tokens- line-comments (which can be discarded), special delimiters (`[]{}`), names, and numbers (which can be coerced into JavaScript numbers). While it is possible to implement `{` and `}` as PostScript operators, it is simpler for our purposes to cluster the tokens of procedures together as we parse the program:

```
parse = x=>{
 const t=x.trim().match(/(%[^\n]*\n)|(\[|\]|\{|\})|(\/?[^\[\]\{\}\/% \n]+)|(-?\d+)/gm).filter(x=>x[0]!='%')
 const next=()=>t.shift()
 const peek=()=>t[0]
 const isnum=x=>+x==+x
 const rec=()=>peek()=='{'?(next(),seq()): isnum(peek())?+next(): next()
 const seq=()=>{const r=[];while(peek()&&peek()!='}')r.push(rec());return next(),proc(r)}
 return seq()
}
```

Given a PostScript program like

```
[10 20 30] {2 mul} forall
```

The `parse(x)` routine will break it down into

```
{t:'proc', v:['[', 10, 20, 30, ']', {t:'proc', v:[2, 'mul']}, 'forall']}
```

## The Interpreter

The PostScript operand stack and dictionary stack can be represented as simple JavaScript global arrays- `o` and `d`, respectively:

```
o=[]
d=[]
```

Some simple routines for manipulating these stacks:

```
push     = x=>o.push(x)
pop      = x=>o.pop()
popn     = x=>{const r=[];while(x-->0)r.unshift(pop());return r}
popmark  = ()=>{const r=[];while(o.length&&MARK!=last(o))r.unshift(pop());pop();return r}
load     = x=>{const v=litval(x);return d.findLast(s=>s.has(v)).get(v)}
```

Executing pre-tokenized PostScript programs is very simple: we execute each token in sequence. Names are looked up in the dictionary stack and the corresponding operator or procedure is called; anything else is pushed onto the operand stack. Since operators are JavaScript functions, when we execute them we’ll pop a number of values from the operand stack corresponding to their argument list count and `apply()` them to the function. If the function returns something other than `undefined`, we’ll push the result onto the stack:

```
execfunc = x=>{const v=x.apply(null,popn(x.length));if(v!=undefined)push(v)}
execproc = x=>{isproc(x)?procval(x).forEach(exec): exec(x)}
exec     = x=>{isfunc(x)?execfunc(x): isname(x)?execproc(load(x)): push(x)}
```

The large bulk of the machinery of our interpreter lives in the primitive operator definitions, but each is individually fairly simple, with the possible exception of the `for` operator, which needs a few helper routines for generating stepped ranges. Many of these operators are pure JavaScript functions which do not directly access or modify either stack and can be tested and understood in isolation.

```
countup  = (x,y,z)=>{const r=[];while(x<=z)r.push(x),x+=y;return r}
countdn  = (x,y,z)=>{const r=[];while(x>=z)r.push(x),x+=y;return r}
range    = (x,y,z)=>x<z&&y>0?countup(x,y,z): y<0?countdn(x,y,z): []

prims={
 true:   true,
 false:  false,
 exec:   execproc,
 for:    (x,y,z,w)=>range(x,y,z).forEach(v=>(push(v),execproc(w))),
 forall: (x,y)=>x.forEach(v=>(push(v),execproc(y))),
 repeat: (x,y)=>iota(x).forEach(v=>execproc(y)),
 ifelse: (x,y,z)=>execproc(x?y:z),
 load:   load,
 def:    (x,y)=>{last(d).set(litval(x),y)},
 begin:  x=>{d.push(x)},
 end:    ()=>{d.pop()},
 dup:    x=>(push(x),x),
 exch:   (x,y)=>(push(y),x),
 pop:    x=>{},
 index:  x=>o[o.length-(1+x)],
 copy:   x=>o.slice(-x).forEach(push),
 roll:   (x,y)=>popn(x).forEach((_,i,a)=>push(a[mod(i-y,x)])),
 floor:  x=>Math.floor(x),
 sqrt:   x=>Math.sqrt(x),
 not:    x=>!x,
 add:    (x,y)=>x+y,
 sub:    (x,y)=>x-y,
 mul:    (x,y)=>x*y,
 div:    (x,y)=>x/y,
 mod:    mod,
 eq:     (x,y)=>x==y,
 ne:     (x,y)=>x!=y,
 gt:     (x,y)=>x>y,
 ge:     (x,y)=>x>=y,
 lt:     (x,y)=>x<y,
 le:     (x,y)=>x<=y,
 '[':    ()=>MARK,
 ']':    popmark,
 length: x=>x.length,
 get:    (x,y)=>x[y],
 dict:   x=>new Map(),
}
```

To access the primitives from within `exec()` and `execproc()` we need to install them into the dictionary stack. Here’s a procedure to initialize both stacks:

```
init = ()=>{o=[],d=[new Map(Object.entries(prims))]}
```

We can now execute programs and observe their effects on the stacks:

```
init()
execproc(parse('2 3 add'))
console.log(pop()) // 5
```

## Making it Interactive

For interactive use, we can wire everything up into a [Read-Eval-Print-Loop](https://en.wikipedia.org/wiki/Read–eval–print_loop) (REPL). We’ll need at least some rudimentary output capabilities, so for starters we can install an extra primitive `==` which pops a value from the stack and displays it:

```
format = x=>isarray(x)?`[${x.map(format).join(' ')}]`: isproc(x)?`{${format(procval(x))}}`: `${x}`
prims['==']=x=>{console.log(format(x))}
init()
```

If we’re using [Node.js](https://nodejs.org) we have to write our REPL in an event-driven style:

```
// usage: node ps.js
rl=require('readline').createInterface({input:process.stdin,output:process.stdout,prompt:'  '})
rl.prompt()
rl.on('line',x=>(execproc(parse(x)),rl.prompt()))
rl.on('close',_=>(console.log(''),process.exit(0)))
```

If we’re using [QuickJS](https://bellard.org/quickjs/) we can write a more straightforward infinite loop instead:

```
// usage: qjs --std ps.js
while(1){std.out.printf('  '),execproc(parse(std.in.getline()))}
```

Either way, we now have a fancy PostScript-style desk calculator:

```
  2 3 add ==
5
  [[11 22 33] {10 mul} forall] ==
[110 220 330]
```

[The entire interpreter in one piece](https://beyondloom.com/blog/postscript-figures/ps.js), for reference.

## Exploring PostScript

Our interpreter offers a very small subset of the primitive operators supplied with standard PostScript. There are some fun finger-exercises to be had in fleshing out more of that set in terms of what we already have:

```
/if  {{} ifelse} def
/neg {-1 mul} def
```

Try your hand at `abs`\- compute the absolute value of a number:

/abs {dup 0 lt {neg} if} def

How about `min`\- find the minimum of two values on the stack?

/min {2 copy gt {exch} if pop} def

It can also be worthwhile to try some classic functional programming exercises, like recursively computing the Fibonacci sequence:

```
/fib {dup 2 ge {1 sub dup 1 sub fib exch fib add} {} ifelse} def

[0 1 10 {fib} for] ==
% [0 1 1 2 3 5 8 13 21 34 55]
```

There are, of course, more idiomatic iterative approaches:

```
[0 1 9 {2 copy add} repeat] ==
% [0 1 1 2 3 5 8 13 21 34 55]
```

I quite enjoy the way that enclosing PostScript expressions in `[]` acts like a more composable alternative to [list comprehensions](https://en.wikipedia.org/wiki/List_comprehension).

Here’s a utility routine for making a copy of an array with the elements in reversed order:

```
/reverse {[exch dup length 1 sub -1 0 {1 index exch get exch} for pop]} def

[11 22 33] reverse ==
% [33 22 11]
```

How many approaches can you think of for *concatenating* a pair of arrays, given the primitive operators we have at hand?

```
[11 22 33] [44 55] concat ==
% [11 22 33 44 55]
```

One approach:

/concat {\[3 1 roll exch {exch} forall {} forall\]} def

* * *

Much of my experience with [concatenative programming languages](https://en.wikipedia.org/wiki/Concatenative_programming_language) like PostScript is with Forth, which uses slightly different names and factorings for most operators. Fortunately, PostScript is fairly malleable:

```
/swap  {exch} def
/drop  {pop} def
/over  {1 index} def
/2dup  {2 copy} def
/2drop {pop pop} def
/rot   {3 -1 roll} def
/-rot  {3 1 roll} def
/@     {load exec} def
/!     {exch def} def
```

PostScript’s `roll` and `index` operators make it easier to reach deep into the stack than the usual Forth stack-twiddlers, but this also demands the programmer retain a larger perspective of the contents of the stack in their limited working memory as they read. Even `rot`/`-rot` can be controversial “code smells” to some Forthwrights.

In Forth, we can freely manipulate the “return stack” as well as the operand stack, usually with words like `r>` and `>r` for transferring values from the top of one stack to the other. Forth control structures will often store things like loop indices on the return stack between iterations in order to prevent them from interfering with the flow of arguments within the loop body. It can also be handy for temporarily stashing intermediate results to keep the operand stack shallow. PostScript has an “execution stack” for nested procedure bookkeeping, but it is not under direct control of the programmer; as a result we can’t use these sorts of idioms.

The dictionary stack offers nestable scopes for named local variables:

```
2 dict begin
 /b def
 /a def
 a a mul
 b b mul
 add sqrt
end
```

It’s even possible to build tidy abstractions around this pattern:

```
/local {1 dict begin 3 1 roll exch def exec end} def

25 /a {a a add} local
```

But since this approach produces *dynamic* rather than *lexical* scopes, it is not transparent to procedures we `exec`. Without an auxiliary stack to “hide” intermediate state, it can be surprisingly challenging to write higher-order functions. Try defining the equivalent of a Lispy `(cond)` conditional in PostScript and you’ll see what I mean:

```
[{p1} {e1} {p2} {e2} ... {e}] cond
```

PostScript has a tantalyzing `bind` operator (not implemented in our interpreter) which is meant to help sidestep the footguns of dynamic scope by eagerly replacing references to operators within a procedure with their values at `bind`-time. However, since `bind` concerns itself only with resolving *operators*, it isn’t useful for simulating general-purpose lexical scope; we’d need to build a more general mechanism for performing named substitutions within procedures.

## What Comes Next?

My preceding semantic quibbles aside, I think PostScript is quite elegant, and well worth learning. Now that we have an interpreter for the language which fits on a printed page or two, we have an inviting foundation for experimentation. I’m sure that over the course of our brief explorations you’ve already thought of many new primitive operators you could add- `sin`, `cos`, `pi`, perhaps more tools for manipulating data like `type`, `cvx`, and `cvlit`, or a richer set of input and output routines?

You might find it worth the added bulk to add more robust error-handing. For example, you could make `load` tolerate undefined names and return a placeholder:

```
load = x=>{const v=litval(x);return (d.findLast(s=>s.has(v))||new Map()).get(v)||null}
```

You could also implement a more complete range of datatypes and syntax, like `(strings)`, non-integer numbers, and the `<</a 11 /b 22>>` syntax for dictionary “literals”.

Take your budding new human-scale programming environment in whatever direction you like. Enjoy the feeling of power.

## Further Reading

- [PostScript Language Reference, third Edition](https://www.adobe.com/jp/print/postscript/pdfs/PLRM.pdf)
- [Concatenative Language Wiki](https://concatenative.org/wiki/view/Front%20Page)
- [How To Write a Lisp Interpreter in Python](https://www.norvig.com/lispy.html)

[back](https://beyondloom.com/blog/index.html)
