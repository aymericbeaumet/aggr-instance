---
title: 'Formatting an entire 25 million line codebase overnight: the rubyfmt story'
link: https://stripe.dev/blog/formatting-an-entire-25-million-line-codebase-overnight-the-rubyfmt-story
source: stripe-com
published: 2026-04-28T00:00:00Z
updated: 2026-04-28T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Fable Tales
summary: This is the story of how our Developer Productivity team extended and rolled out rubyfmt, our Rust-based zero-config, ultra-fast autoformatter across the world's largest Ruby codebase.
content: extracted
html: 2026-04-28-formatting-an-entire-25-million-line-codebase-overnight-the.html
remote_preview:
  url: https://stripe.dev/og/blog/formatting-an-entire-25-million-line-codebase-overnight-the-rubyfmt-story.png
  alt: 'Stripe Developer Blog: Formatting an entire 25 million line codebase overnight: the rubyfmt story'
extra:
  content:interactive: true
---

Stripe runs the [world's largest Ruby codebase](https://newsletter.pragmaticengineer.com/p/stripe). Before `rubyfmt`, no true autoformatter for Ruby existed anywhere in the industry. Previous tools had crashed just trying to process our files. Then, six years after I started [`rubyfmt`](https://github.com/fables-tales/rubyfmt) as a personal open-source project, two Stripe engineers got on a Saturday morning Zoom call with 25 million lines of Ruby and a plan that had never been attempted at this scale.

![](https://stripe.dev/images/formatting-an-entire-25-million-line-codebase-overnight-the-rubyfmt-story/image1.png)

> Sample created using [rubyfmt playground](https://rubyfmt.run/)

## The origin of rubyfmt

At RubyConf 2018, over a rye Manhattan at the Millennium Biltmore, I found myself in a heated debate with Justin Searls and Aaron Patterson about how Ruby desperately needed an autoformatter. At the time, language servers in Ruby were nascent, if they existed at all. The cost of booting Ruby with gems and loading `bundler` made the idea of "format on save" editing too slow.

We left that bar without a solution. But we knew what one would need to look like: zero configuration, fast, and accessible to engineers coming from other languages. That conversation was the spark. I went home and started building `rubyfmt` as a personal open source project.

### No configuration, no arguments

If you've ever had a team debate about trailing commas, keyword layout, or conditional formatting, you already understand the core problem. If you've ever argued about what the correct configuration for a linter is, you understand it even better.

`rubocop` is the default linter for almost all Ruby teams. It's a popular and powerful tool, but it isn't an autoformatter. Its hundreds of individually configurable rules mean that any two engineers can reasonably disagree about the right setup, which just moves the bikeshedding from pull requests to configuration files.

One thing I've learned during my career is that if engineers *can* disagree about something, they will. Style debates are no different, and they have a way of consuming far more time than anyone wants to admit.

What the Ruby ecosystem was missing was something like `gofmt`, a zero-configuration autoformatter that simply makes the decisions for you, runs fast, and ensures you never argue about formatting again.

### Built for the fastest workflows imaginable

Starting a regular Ruby process takes around 158ms (Ruby 2.7 on my M4 Pro MacBook Pro). To the most productive engineers, that's an eternity for a save hook. Starting Ruby inside a `bundler` environment is worse, closer to 345ms.

We set a strict budget of 100ms for `rubyfmt` to format all but the very largest files. To hit it, we couldn't invoke Ruby in the normal way. We had to go deeper, compiling a hand-authored C program directly against `libruby` to squeeze out every millisecond we could.

### Ruby for engineers who don't speak Ruby (yet)

At Stripe, we recruit great engineers regardless of their language background. Many engineers come to Stripe never having written a line of Ruby, and we then ask them to work on the world's largest Ruby codebase. The Go engineers I worked with told me they were struggling with Ruby. Its flexibility left too much room for uncertainty without an autoformatter. They missed `gofmt`, which would simply “snap the code into place” for them.

A strict autoformatter removes that uncertainty. It lets engineers new to a language stop thinking about how code should look and start focusing on what it should do.

## Building rubyfmt

Ruby isn’t a syntactically simple language. Consider this perfectly valid Ruby program:

```
a = <<EOD
part 1 of heredoc #{ "not a heredoc" + <<EOM }
eom part
EOM
part 2 of heredoc
EOD
```

Given that complexity, the hypothesis was simple: tackle the hardest syntax first and the rest will follow. `rubyfmt` started its life in 2018 as a pretty simple Ruby program built on top of [`ripper`](https://ruby-doc.org/core-2.7.0/Ripper.html), a Ruby-level API built on top of Ruby’s parser.

In `rubyfmt`'s early days, it was essentially a large switch statement over all possible [`ripper`](https://github.com/fables-tales/rubyfmt/blob/cf08f1479b6063c34f3792f1854fc4c62c81c4dd/run.rb#L715-L748) [expression types](https://github.com/fables-tales/rubyfmt/blob/cf08f1479b6063c34f3792f1854fc4c62c81c4dd/run.rb#L715-L748):

```
def format_expression(ps, expression)
  type, rest = expression[0],expression[1...expression.length]
  {
    :return => lambda { |ps, rest| format_return(ps, rest) },
    :def => lambda { |ps, rest| format_def(ps, rest) },
    :if => lambda { |ps, rest| format_if(ps, rest) },
    # ...and so on for all expression types
  }.fetch(type).call(ps, rest)
end
```

Speed was a priority, so users were advised to start `rubyfmt` with the `--disable=gems` flag. This meant `rubyfmt` couldn't depend on any gems or be part of a `bundler` environment. Loading either would have blown the 100ms speed budget before a single line was formatted.

The distribution strategy was to [literally combine all source files into one large Ruby file](https://github.com/fables-tales/rubyfmt/blob/v0.2.0/Makefile#L6-L7) and tell people to stick it in their path. Not a technique I’d recommend in most contexts, but given the constraints, it was the right tradeoff.

### Rewriting in Rust (it went deeper than expected)

As the complexity of `rubyfmt` grew, it became slower and slower to use Ruby to autoformat Ruby. `ripper` was originally chosen because it's the only Ruby parsing library built on top of `parse.y`, the exact same parser Ruby uses when executing code. But `rubyfmt` was blowing the 100ms latency budget autoformatting files of moderate complexity, so I rewrote `rubyfmt` in Rust.

This turned out to be complicated. At the time there were no parsers for Ruby that could be compiled without the Ruby VM present. Ruby is itself a very complicated C program, so the result was a build process that *first* compiled all of Ruby, *then* linked it to a Rust program. Ultimately, this required evaluating Ruby code from within a Rust binary:

```
pub unsafe fn load_rubyfmt() -> Result<(), ()> {
    let rubyfmt_program = include_str!("../rubyfmt_lib.rb");
    eval_str(rubyfmt_program)?;
    Ok(())
}
```

That’s when something "fun" emerged about the Ruby data structures `ripper` produces: all the output types are valid JSON (with the exception of symbols, which Ruby can turn into JSON strings when you dump JSON from primitives).

```
irb(main):001> Ripper.sexp("def foo; 1 + 2; end")
=> [:program, [[:def, [:@ident, "foo", [1, 4]], [:params, nil, nil, nil, nil, nil, nil, nil], [:bodystmt, [[:binary, [:@int, "1", [1, 9]], :+, [:@int, "2", [1, 13]]]], nil, nil, nil]]]]
```

In the first Rust draft of `rubyfmt`, I didn’t want to get fancy with manipulating Ruby objects from Rust. So I took the entire `ripper` tree, encoded it to JSON (in Ruby), and deserialized it using `serde`. Super janky, but it allowed rapid processing of `ripper` parse trees in Rust.

Eventually, going from Ruby objects to JSON to Rust objects became onerously slow. One extremely neat thing about `serde` is that it entirely separates the notion of schema from any specific serialization format.

For example, here’s the schema definition for `ripper`'s `@ident` node type:

```
def_tag!(ident_tag, "@ident");
#[derive(Deserialize, Debug, Clone)]
pub struct Ident(pub ident_tag, pub String, pub LineCol);
```

Here's what that node looks like in `ripper`'s output:

```
irb(main):002> Ripper.sexp("foo")
=> [:program, [[:vcall, [:@ident, "foo", [1, 0]]]]]
```

Converting that to JSON:

```
irb(main):003> JSON.dump(Ripper.sexp("foo"))
=> "[\"program\",[[\"vcall\",[\"@ident\",\"foo\",[1,0]]]]]"
```

From there, I gave Rust a pointer to the Ruby string. Rust called back into Ruby to retrieve the raw bytes and handed them to `serde`, which decoded the JSON into Rust objects according to the schema. At the time, sharing objects across Ruby, C, and Rust was largely uncharted territory.

This JSON serialization worked. But it was slow.

In Ruby, the C type for a Ruby object is called `VALUE`. It’s an opaque wrapper around the structures Ruby uses to represent objects under the hood. I asked myself: the `ripper` parse tree is full of primitives—Can I just teach `serde` to walk through the Ruby objects in memory?

The task was to unpack `VALUE`, determine what it represents, and tell `serde` if I’ve encountered some value type it understands:

```
    fn deserialize_any<V: de::Visitor<'de>>(self, visitor: V) -> Result<V::Value> {
        pub use ruby::ruby_value_type::*;

        match unsafe { ruby::rubyfmt_rb_type(self.0) } {
            RUBY_T_SYMBOL => visitor.visit_borrowed_str(sym_to_str(self.0)?),
            RUBY_T_STRING => visitor.visit_borrowed_str(rstring_to_str(self.0)?),
            RUBY_T_ARRAY => visitor.visit_seq(SeqAccess::new(self.0)),
            RUBY_T_NIL => visitor.visit_none(),
            RUBY_T_TRUE => visitor.visit_bool(true),
            RUBY_T_FALSE => visitor.visit_bool(false),
            RUBY_T_FIXNUM => visitor.visit_i64(unsafe { ruby::rubyfmt_rb_num2ll(self.0) }),
            other => Err(de::Error::custom(format_args!(
                "Unexpected type {:?}",
                other
            ))),
        }
    }
```

Ruby's C API lets us introspect a `VALUE`. `rubyfmt_rb_type` is equivalent to Ruby's `rb_type` macro, and returns a constant telling us what Ruby type is underneath. Most types map to Rust fairly easily, but `RUBY_T_ARRAY` needs its own special support code:

```
struct SeqAccess {
    arr: VALUE,
    idx: usize,
    len: usize,
}

impl SeqAccess {
    fn new(arr: VALUE) -> Self {
        let len = unsafe { ruby::rubyfmt_rb_ary_len(arr) } as usize;
        Self { arr, len, idx: 0 }
    }
}

impl<'de> de::SeqAccess<'de> for SeqAccess {
    type Error = Error;

    fn next_element_seed<T: de::DeserializeSeed<'de>>(
        &mut self,
        seed: T,
    ) -> Result<Option<T::Value>> {
        if self.idx < self.len {
            let elem = unsafe { ruby::rb_ary_entry(self.arr, self.idx as _) };
            self.idx += 1;
            seed.deserialize(Deserializer(elem)).map(Some)
        } else {
            Ok(None)
        }
    }

    fn size_hint(&self) -> Option<usize> {
        Some(self.len - self.idx)
    }
}
```

With these changes, the schema of `ripper_tree_types` (the Rust module defining the shape of `ripper`’s output) didn’t need to change at all. All that was left to do was implement the Ruby `VALUE` deserializer and everything worked.

Linking a full Ruby VM into a Rust binary to walk its parse tree in memory isn’t a normal thing to do. But it worked, and that was enough for now.

## Bringing rubyfmt to Stripe

### The problems we couldn't ignore

`rubyfmt` was already two years in the making when I joined Stripe in 2020, though it wasn't finished yet. Stripe had already tried adopting [`prettier-ruby`](https://github.com/prettier/plugin-ruby), but it was too slow (`prettier-ruby` is implemented in JavaScript) and too unstable. It had crashed outright on some of our larger files.

The pain of not having an autoformatter was visible in how our engineers worked. Stripe's Developer Productivity org runs regular "shoulder surfing" sessions, where we watch engineers work to spot friction. In several sessions, we noticed engineers wrestling with `rubocop` and spending significant time just trying to get their code into the right place. They wanted to spend their time coding, not developing an intuition for Ruby formatting conventions.

Something had to change. With 25 million lines of Ruby in Stripe's monorepo, we needed something purpose-built for Ruby at this scale. The manager and tech lead of Stripe's Ruby team came to me with a question: could `rubyfmt` work, and what would it take to fund it? `rubyfmt` wasn't done yet, but it was the best option we had. Stripe decided to bet on it. In 2022, two engineers from our Ruby infrastructure team started working on it full time, and `rubyfmt` remained open source throughout. As the author of `rubyfmt`, I was thrilled. Stripe was going to fund my project and keep it open source. This was exactly what I had hoped for.

### 62,213 files, one Saturday morning

Rolling out a novel autoformatter to 25 million lines of code has two big risks: merge conflicts and correctness. A bug affecting just 0.01% of lines would still touch tens of thousands of files. To manage both, we built in a per-file opt-in so `rubyfmt` would only format files that explicitly asked for it. Following the Developer Productivity org’s typical pattern, we started with systems we owned and could observe closely, then expanded coverage gradually as our confidence grew.

We also built a tool to diff `ripper` trees across formatted files, accounting for things like `rubyfmt` converting single quotes to double quotes. Combined with our [extensive test suite](https://stripe.dev/blog/fast-secure-builds-choose-two), we built confidence slowly and deliberately.

Then came that Saturday morning in 2024. We flipped the per-file opt-in model entirely: instead of files opting in, only the tiny minority `rubyfmt` couldn't yet handle were held back. We chose a Saturday to format the entire codebase to avoid merge conflicts. And while our test suite gave us high confidence we'd gotten everything right, it's always a bit daunting to have a diff so large that GitHub can't render it. It just shows `files changed: infinity`. We merged it anyway. From there, we burned down the remaining exceptions.

The codebase has grown since. And today, 100% of Stripe's 42 million lines of Ruby are formatted with `rubyfmt`.

### How it’s going

The thing that strikes us most about `rubyfmt` is how little anyone talks about it. Like the best infrastructure, it's mostly invisible. It just works.

The engineers who feel it most are the ones who came to Stripe from other languages. One engineer, who joined Stripe having never written Ruby before, put it plainly:

> “I sometimes receive code format nits on code reviews. Coming from Python where e.g. black was very common I found this quite wasteful/frustrating. It was probably among the ~10 things that felt most obviously off about Stripe's developer productivity at the time. I haven't thought about this problem in years and I'm very glad that's the case.”

Engineers across Stripe echo this. Faster PRs, less friction, and one less thing to think about:

> “It is very fast and it is format-on-save. Personally, I am very happy about it. Best formatting experience I've had in a while.”

> “It’s really nice not having to waste time giving or receiving PR feedback about simple formatting stuff.”

> “Very fast, no discussions on what format to use, when you have something you don’t even have to think about it means the tooling is at its best.”

### We're not done yet

For years, `rubyfmt` required linking to a full Ruby VM just to parse files. In 2025 that changed: `Prism`, a new parser built to replace `ripper`, became the official Ruby parser. Crucially, it can instantiate a parse tree without linking a Ruby VM at all.

Migrating `rubyfmt` to `Prism` was gradual. We made the parser toolchain selectable, ran our full test suite against both `ripper` and `Prism` with all `Prism` tests marked as expected failures, and slowly burned down the `Prism` failing tests until `rubyfmt` produced identical output from both parsers. Because `Prism` loads native objects directly into memory, we no longer needed `serde` or the object deserialization code.

Our binary shrank by megabytes and `rubyfmt` got dramatically faster.

If you want to help figure out what comes next, or work on other developer productivity problems at scale, [we’re hiring](https://stripe.com/jobs).

–

*We'd like to give special thanks to Reese, a former Stripe engineer. He worked on `rubyfmt` at Stripe and has continued to commit after leaving the company. We truly couldn't have achieved what's described here without him.*
