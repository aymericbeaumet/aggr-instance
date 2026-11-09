---
title: A Critique of GPUI
link: https://blog.zortax.de/posts/a-critique-of-gpui
source: hnrss-org
published: 2026-11-09T03:51:10Z
updated: 2026-11-09T03:51:10Z
first_seen: 2026-11-09T10:14:22.046232619Z
authors:
- ByteAtATime
content: extracted
html: 2026-11-09-a-critique-of-gpui.html
remote_preview:
  url: https://blog.zortax.de/images/a-critique-of-gpui/scaling-linear.png
  alt: On a linear axis, the cost per row is the slope
---

Recently, there has been a trend of more desktop GUI apps being built using native rust GUI frameworks. And there are now even well known electron apps getting official rewrites: Discord announced their app is being rewritten using GPUI.[1](https://blog.zortax.de/posts/a-critique-of-gpui#1) Meanwhile, a lot of the LLM-clones being published seem to choose egui. In this post, I want to explain why I am not sure those frameworks are a good foundation to build on.

To put it plainly: I believe both GPUI’s and egui’s state models are dead ends for complex desktop applications. Both are impressive projects, and they have their place. Zed genuinely feels fast and nice to use, and I use egui myself for debug interfaces in bevy games. But I don’t think those frameworks should be the default choice for native desktop apps.

I want to make two arguments:

- Every UI architecture ends up needing to know what changed and who depends on it, or performance characteristics will not scale. A framework whose state model cannot say so makes the application say it by hand, at the cost of whatever made that framework’s API semantics attractive in the first place.
- Once you admit you want that precision, components that run once, with signals and reactive holes, are the simplest API for it.

*Disclosure: I ported GPUI’s current Linux renderer to wgpu [2](https://blog.zortax.de/posts/a-critique-of-gpui#2), I built the benchmark harness used below, and I work on [zgui](https://github.com/zortax/zgui), a signal-based Rust framework that appears in the charts.*

## The only question that matters

Raph Levien quotes Ron Minsky: “hidden inside of every UI framework is some kind of incrementalization framework.”[3](https://blog.zortax.de/posts/a-critique-of-gpui#3) A UI is a function of state, and state changes a little at a time. But if that function becomes expensive, you might want to start looking for ways to derive a faster way to update pixels on the screen from an incremental state change. So every framework has to answer one question: when one value changes, what runs?

| Model                                  | What a write runs          | Who finds the change |
| -------------------------------------- | -------------------------- | -------------------- |
| Immediate (egui)                       | All UI code, next frame    | Nobody               |
| Elm (Iced)                             | All of `view`, then a diff | The diff             |
| Views (GPUI)                           | Every view without a cache | You, with `notify`   |
| Signals, re-run (Freya, Compose)       | Components that read it    | The framework        |
| Signals, run once (Floem, Solid, zgui) | Closures that read it      | The framework        |

I understand why people like the first three. No state to synchronize, one place where state lives, a `render` function that is plain Rust, no macros or separate compiler. I’m gonna make the argument that these approaches don’t scale, and fixing the underlying problem defeats what makes these models simple and easy to reason about.

## GPUI: `notify` is zone.js

GPUI calls itself “a hybrid immediate and retained mode” framework.[4](https://blog.zortax.de/posts/a-critique-of-gpui#4) It retains state in entities. The UI is defined in `render` functions, changing state must trigger a rerun.

```rust
impl Render for Sidebar {
    fn render(
        &mut self,
        _: &mut Window,
        cx: &mut Context<Self>,
    ) -> impl IntoElement {
        let inbox = self.inbox.read(cx);
        div()
            .child(format!("{} unread", inbox.unread()))
            .children(inbox.threads.iter().map(|t| t.title.clone()))
    }
}
```

It is ordinary Rust. You can use `if`, `for` and local variables, and there aren’t any weird wrappers. State changes require an explicit notification:

```rust
self.threads[i].unread = false;
cx.notify();
```

`notify` carries the entity id.[5](https://blog.zortax.de/posts/a-critique-of-gpui#5) Since Rust doesn’t have a write barrier on `&mut`, GPUI cannot see which field changed (or whether anything changed at all). The Angular team wrote this about zone.js in 2023: it “is only capable of notifying us when something *might* have happened”, and “This approach has not scaled well.”[6](https://blog.zortax.de/posts/a-critique-of-gpui#6) `notify` is more precise than a zone, since it names an entity, but ultimately it makes the same kind of statement. Below that id, GPUI knows nothing, so it re-renders every view that is not behind a valid `.cached()` boundary. And in practice, GPUI often rerender every view, because `.cached()` is rarely used by application authors, and imposes certain restrictions on layout semantics.

GPUI has a reputation of being “fast”. And while constants might be relatively low, asymptotically, it is leaving a lot on the table. So I ran a few benchmarks, that specifically test how much a sparse state change in a large UI actually costs.

At 3,000 live rows with a cached view per row, changing one label takes 8.4 ms. A hover, a keystroke and writing a value equal to the old one all cost about the same, because the cost follows the tree and not the change. It grows by about 3 ms per 1,000 live rows. Write the rows as plain elements, as in the code above, and the same label takes 92 ms. A hand-written equality guard takes the equal write from 7.9 ms to 0.9 µs (demonstrating how much is left for each call site to find).

![On a linear axis, the cost per row is the slope](https://blog.zortax.de/images/a-critique-of-gpui/scaling-linear.png)![On a linear axis, the cost per row is the slope](https://blog.zortax.de/images/a-critique-of-gpui/scaling-linear-dark.png)

This doesn’t mean that every GPUI app is slow, but it shows the limit of the state model. Especially with more expensive layout, you *do hit* this wall in practice. Zed might feel fast, but the truth is that Zed is a relatively simple app, UI wise. The app’s chrome contains a few buttons, in a simple layout, a sidebar and a text editor, and the text editor is implemented as a custom element, completely bypassing GPUI’s layout and view semantics. By their own account, a window “has tens of nodes and a few thousand elements”.[7](https://blog.zortax.de/posts/a-critique-of-gpui#7)

## Making GPUI precise defeats GPUI

The obvious fix is to retain more. Zed has experimental branches for this, and Longbridge’s gpui-fast is a fork that keeps every view’s subtree between frames.[8](https://blog.zortax.de/posts/a-critique-of-gpui#8) [9](https://blog.zortax.de/posts/a-critique-of-gpui#9) It works, within a limit: the smallest thing either one can reuse is a view. So to update one label without rebuilding the sidebar, the label has to be a view:

```rust
pub struct UnreadBadge { unread: usize, _watch: Subscription }

impl UnreadBadge {
    pub fn new(inbox: Entity<Inbox>, cx: &mut Context<Self>) -> Self {
        let unread = inbox.read(cx).unread();
        let _watch = cx.observe(&inbox, |this, inbox, cx| {
            let unread = inbox.read(cx).unread();
            if unread != this.unread {
                this.unread = unread;
                cx.notify();
            }
        });
        Self { unread, _watch }
    }
}

impl Render for UnreadBadge {
    fn render(
        &mut self,
        _: &mut Window,
        _: &mut Context<Self>,
    ) -> impl IntoElement {
        format!("{} unread", self.unread)
    }
}
```

Its owner then has to create it and keep the handle:

```rust
let badge = cx.new(|cx| UnreadBadge::new(inbox.clone(), cx));
```

That is an entity, a `Render` impl, a subscription, a stored copy of the value, a hand-written guard and an owner, all to express that one label depends on one count. In a signal framework it is this:

```rust
let unread = Memo::new(move |_| {
    threads.with(|ts| ts.iter().filter(|t| t.unread.get()).count())
});
Label::derived(move || format!("{} unread", unread.get()))
```

The rest of GPUI’s API works against this too. `Render` takes no props, so parents push values into child entities during their own render. `RenderOnce` components, which is what buttons and rows are, draw inside their parent’s view and are no boundary at all. List rows are closures, so a row view means managing a keyed map of entities by hand.

And it only helps where state happens to line up with views. I reran gpui-fast’s own 60-panel benchmark, drew each frame once, and then moved the state:

![Retention pays only where state already matches the views](https://blog.zortax.de/images/a-critique-of-gpui/retention.png)![Retention pays only where state already matches the views](https://blog.zortax.de/images/a-critique-of-gpui/retention-dark.png)

With state owned by each panel, one change saves 83%. With a shared model, retention makes the same frame 11% slower. The window and the edit are identical. In real Zed, the Zed team measured retention “1–3% slower than off on ordinary workloads”, found that “Most of the real-Zed win” came from other work, and warned that “microbenchmark wins didn’t predict real-Zed results”.[8](https://blog.zortax.de/posts/a-critique-of-gpui#8) gpui-fast is real progress and costs little to adopt: it takes the 3,000-row label from 8.4 to 3.4 ms. But that frame renders three views and lays out one element, and still spends 3.4 ms, because the frame is still a walk over the whole tree.

The web has already walked this road. Redux’s style guide says “Prefer having more UI components subscribed to the Redux store and reading data at a more granular level”.[10](https://blog.zortax.de/posts/a-critique-of-gpui#10) MobX says to “create lots of small observer components”.[11](https://blog.zortax.de/posts/a-critique-of-gpui#11) Both tell you to make a boundary per projection, manually, because the framework can’t infer it.

This is the part I find hardest to get past. People choose GPUI because `render` is a plain function of state. A UI where each projection is a separately owned entity with a subscription and a guard is no longer that. It is a signal graph built by hand from much heavier parts, where a missed edge is silent. Zed’s own design note says “Missing notifications are application bugs”.[12](https://blog.zortax.de/posts/a-critique-of-gpui#12) Keep the pleasant API and you get view-sized updates. Take the precision and you lose the pleasant API. A signals crate on top would not change the trade, because its smallest sink is still a view. Reaching one node with no `render` running needs writes the framework can see and nodes that outlive a frame, and that means a new state model and a new element contract.

## Elm: deceptively simple at small scales

At the size it is usually shown, the Elm architecture is a deceptively simple UI model: one state value, one `update`, one `view`, and every change has a name. While large applications do use it successully,[13](https://blog.zortax.de/posts/a-critique-of-gpui#13) [14](https://blog.zortax.de/posts/a-critique-of-gpui#14) I do want to make the point that this clarity belongs to the small case, and in a complex application signals and stores are arguably easier to reason about.

Take the same `unread` count. It has to be right after every message, so either `view` recomputes it on every update or `update` maintains it:

```rust
match message {
    InboxMessage::MarkRead(i) => {
        if self.threads[i].unread {
            self.threads[i].unread = false;
            self.unread -= 1;
        }
    }
    InboxMessage::Delete(i) => {
        if self.threads.remove(i).unread {
            self.unread -= 1;
        }
    }
    InboxMessage::Arrived(thread) => {
        self.unread += thread.unread as usize;
        self.threads.push(thread);
    }
}
```

The definition of `unread` is now nowhere in particular. It is spread across every arm that touches a thread, and each new message has to remember it. The compiler checks that the `match` is exhaustive, but it does not check the arithmetic. The memo above has one definition, and a new writer cannot forget it.

The same thing happens with location. Where does a hover state or a half-typed draft live? Single source of truth says in the model, with a message type routed down and an outcome routed back up. Elm’s guide: “Actively trying to make components is a recipe for disaster in Elm.”[15](https://blog.zortax.de/posts/a-critique-of-gpui#15). Iced recommends returning an `Action` to the parent.[16](https://blog.zortax.de/posts/a-critique-of-gpui#16) Iced’s maintainer says components in this architecture always end “with pain”, and also makes the best case for the discipline: avoiding them “forces you to think hard about your data model”.[17](https://blog.zortax.de/posts/a-critique-of-gpui#17) Okay, I guess that’s a fair argument. But cosmic-files has 136 top-level message variants and 73 more for a tab, plus a `Tab::update` that returns commands for the parent to interpret.[18](https://blog.zortax.de/posts/a-critique-of-gpui#18) Everything in that is explicit, and none of it is simple.

The architecture also has the same issue GPUI has. The `update` implementation tells you what a message writes, it cannot tell you what read the field. Anything inside the `view` might. Elm’s architecture doesn’t answer “what changed?”, it answers “what happened?”. Session export might be useful in some scenarios, but it is not useful for scalable performance characteristics.[19](https://blog.zortax.de/posts/a-critique-of-gpui#19)

This is what I read in the web’s history. Redux is the best-known relative of this design, and its docs read like a field report. Treat a portion of your store “as if it were a database”.[20](https://blog.zortax.de/posts/a-critique-of-gpui#20) Selectors “will be re-run after every dispatched action”.[21](https://blog.zortax.de/posts/a-critique-of-gpui#21) The core package is “obsolete”, replaced by a toolkit made to remove boilerplate.[22](https://blog.zortax.de/posts/a-critique-of-gpui#22) Outside React, the mainstream has moved to tracked reads. Angular, Vue, Svelte and Solid build on signals or something equivalent, Preact offers them, and Svelte’s team put it this way: “we’ve come to the realisation that Knockout was right all along.”[23](https://blog.zortax.de/posts/a-critique-of-gpui#23) Inside React, Jotai and MobX bring tracking in as libraries, and Legend-State even offers a “render once” style.[24](https://blog.zortax.de/posts/a-critique-of-gpui#24) Most applications are still on React, and React’s team disagrees, which I come back to below. So the convergence is on the state model, outside one large exception.

Performance follows the same line. Since Iced rebuilds all of `view` after every batch of messages, one label at 3,000 rows builds 39,023 widgets and takes 7.3 ms. `Lazy` on every row only gets that to 5.7 ms, because `view` still has to visit each row to hash its key. The official `lazy` example keys on a `version: u8` that four `update` arms bump by hand.[25](https://blog.zortax.de/posts/a-critique-of-gpui#25) That is a dirty flag with a nicer name. In fairness, cosmic-files and Halloy use no `lazy` at all, and on a virtualized list Iced is one of the fastest frameworks I measured, at 0.19 ms. A bounded screen rebuilds cheaply. But asymptotic complexity is clear. And similar to GPUI, the API semantics simply can’t express the necessary information.

There is actually precedence for this: Vizia shows what happens when you add precision to this model. It moved from lenses to signals in March 2026 and kept its `Model` and event dispatch.[26](https://blog.zortax.de/posts/a-critique-of-gpui#26) This is its counter, shortened:[27](https://blog.zortax.de/posts/a-critique-of-gpui#27)

```rust
pub enum AppEvent {
    Increment,
}

impl Model for AppData {
    fn event(&mut self, _: &mut EventContext, event: &mut Event) {
        event.map(|app_event, _| match app_event {
            AppEvent::Increment => self.count.update(|count| *count += 1),
        });
    }
}
```

```rust
let count = Signal::new(0);
AppData { count }.build(cx);
Button::new(cx, |cx| Label::new(cx, "Increment"))
    .on_press(|cx| cx.emit(AppEvent::Increment));
Label::new(cx, count).width(Pixels(50.0)).live(Live::Polite);
```

The label reads the signal directly, and the same closure that builds the button created it. The enum, the dispatch and the handler exist to carry one call to `update`. Vizia was never strict Elm, since its views are built once, and signals were a sensible move: it got per-field writes. But look at what remains of Elm’s reasons. State is a graph of mutable cells, `Model` is a bag of handles, and `event` is a `&mut self` side effect. You now have two systems for saying that something changed. So my objection to adding signals to the Elm architecture really is the same as with GPUI: they work, and they remove what you came for.

egui’s pitch is the best in the field: “You don’t have to worry about app state and GUI state being out-of-sync”.[28](https://blog.zortax.de/posts/a-critique-of-gpui#28) For a tool, this is hard to beat:

```rust
for row in rows {
    ui.horizontal(|ui| {
        ui.label(&row.name);
        ui.label(row.price.to_string());
        if ui.button("+").clicked() {
            row.price += 1;
        }
    });
}
```

The price is that a frame costs the size of the UI, independent of what changed. At 3,000 rows every small edit takes about 32 ms, or about 19 ms with accessibility off. egui’s author is candid about the trade: immediate mode is “easier to use, but less powerful”.[28](https://blog.zortax.de/posts/a-critique-of-gpui#28)

As the application grows, the original attraction goes. The layout paradox is “a fundamental shortcoming”.[28](https://blog.zortax.de/posts/a-critique-of-gpui#28) Long lists need `show_rows`, which takes one fixed row height. Rerun, the largest egui application I know of, clamps queries by “inspecting egui state from the previous frame”.[29](https://blog.zortax.de/posts/a-critique-of-gpui#29) Anything expensive goes behind a cache keyed on your data. A cache that has to agree with your data is the synchronization problem immediate mode set out to remove, now in application code, one frame late.

## One engine, different wiring

The harness can show this directly, because some frameworks can be wired coarsely and finely on the same engine:

I benchmarked some of the engines in two ways, once in the most fine grained way they can be used, and once in a coarser way. The results show how much there is to gain with fine-grained reactivity.

![Tracking pays, unless the view function still has to run](https://blog.zortax.de/images/a-critique-of-gpui/granularity.png)![Tracking pays, unless the view function still has to run](https://blog.zortax.de/images/a-critique-of-gpui/granularity-dark.png)

Two things stand out. Iced’s factor is small because the view function still runs. And Floem’s coarse case is worth a look: every cell reads one global revision signal, which is `notify` in signal clothing. It is the slowest result on the chart. So signals do not rescue coarse state. The React team found the same thing from the other side: on a benchmark with “one giant input”, a signals-style runtime disappointed, and Joe Savona concluded the data model “is just as important, maybe more so” than the algorithm.[30](https://blog.zortax.de/posts/a-critique-of-gpui#30) The state has to be shaped like the reads. That is why I call this a state model problem. A good API should make it easy to align the data model with its UI projection.

## Why run once

Suppose you accept all of that. You want the framework to track reads, and you want state shaped to match. What is the simplest API?

```rust
pub fn sidebar(threads: RwSignal<Vec<Thread>>) -> impl IntoView {
    // Runs once, when the sidebar mounts.
    let unread = Memo::new(move |_| {
        threads.with(|ts| ts.iter().filter(|t| t.unread.get()).count())
    });

    Stack::vertical((
        Label::derived(move || format!("{} unread", unread.get())),
        dyn_stack(move || threads.get(), |t| t.id, thread_row),
    ))
}
```

That is the whole thing. The body runs once. The closures you hand to the framework run again when something they read changes. A local variable is a local variable. There are no hook rules, no dependency arrays, no stale closures and no `PartialEq` on props. Nothing reconstructs a row’s identity, because the row is still there.

Compare signals with re-running components, which is Freya’s model. It works well: one keystroke at 3,000 rows costs 1.2 ms. But look at a counter:[31](https://blog.zortax.de/posts/a-critique-of-gpui#31)

```rust
#[derive(PartialEq)]
struct Counter;

impl Component for Counter {
    fn render(&self) -> impl IntoElement {
        let mut count = use_state(|| 0);
        // ...
    }
}
```

`use_state` exists because that line runs again. You now hold both models: read tracking decides which scope re-runs, and then re-render semantics apply inside it, with hooks, prop equality and a diff. Finer updates need smaller components, which is GPUI’s problem at a much lower price. What the re-running body buys is ordinary control flow, and that is the serious objection.

Dan Abramov makes a compelling argument.[32](https://blog.zortax.de/posts/a-critique-of-gpui#32) Run-once puts your rendering logic in closures, so top-level control flow stops working, and you group code around values and not around logic. “The beauty of React is that making things ‘computed’ is an optimization, not a requirement.” He is right about it. This compiles, but the first line never updates:

```rust
Label::new(count.get()),               // read once; never updates
Label::derived(move || count.get()),   // a hole; updates
```

A reactive `if` has to become a `dyn_container`, and a `for` a `dyn_stack`. Solid 2.0 had to add dev warnings for top-level reads.[33](https://blog.zortax.de/posts/a-critique-of-gpui#33)

But the same comment contains the reason I land the other way. He says Solid’s regrouping is “Similar to how you have to restructure your code around the values when you optimize it with useMemo.” Exactly. If your application never needs that, React’s model may be simpler. If it does, you pay for the regrouping either way, and the only choice is when: from the start, or later under a profiler. Ryan Carniato’s reply is the one I agree with: “In Solid you always write as if the data is derived so you don’t refactor to optimize.”[32](https://blog.zortax.de/posts/a-critique-of-gpui#32) Once you know you want this precision, run-once is the API that stops pretending otherwise. The failure modes also favour it. A forgotten hole goes stale on the first update, in the place you wrote it. A forgotten `notify` depends on what else happened to redraw.

To be perfectly honest, the ecosystem’s wider convergence is on read tracking in general, and not on run-once specifically. Angular considered per-binding updates and chose per-view, in writing, and it lands within about 1 ms of Solid on a partial update.[6](https://blog.zortax.de/posts/a-critique-of-gpui#6) [34](https://blog.zortax.de/posts/a-critique-of-gpui#34) Apple and Google, who control their languages, kept re-running bodies with `@Observable` and Compose.[35](https://blog.zortax.de/posts/a-critique-of-gpui#35) [36](https://blog.zortax.de/posts/a-critique-of-gpui#36) A hole is itself a re-run scope, so what differs is how big the default scope is. Two things I want to note on this though:

- First, those systems pay for their larger scopes with a compiler. Compose has a compiler plugin, Angular compiles templates, and React’s compiler is the serious rival to everything here: it keeps React’s API and removes many needless re-renders, with loads up to 12% better.[37](https://blog.zortax.de/posts/a-critique-of-gpui#37) For Rust, I suspect that most people that prefer immediate mode or elm (or whatever GPUI is) would not like the idea of an additional compiler. A proc macro only sees tokens, not types, and `render` is an arbitrary function, so there is no pass that can split it into scopes. What Rust can express cheaply is a closure.
- Second, their own guidance keeps describing the same problem: “Defer reads as long as possible” for Compose, and “Unnecessary view body updates can really add up” for SwiftUI.[38](https://blog.zortax.de/posts/a-critique-of-gpui#38) [39](https://blog.zortax.de/posts/a-critique-of-gpui#39) A framework that runs the body once removes that category at component level.

View-level rerun and fine-grained signals can have the same performance ceiling. But signals encourage fine-grained state projections and memoization from the beginning, for view-level rerun its an explicit choice that often leads to more boilerplate.

## Signals do not automatically make your app fast

Fine-grained reactivity does not make an app fast on its own.

![Signals alone do not flatten the curve](https://blog.zortax.de/images/a-critique-of-gpui/models.png)![Signals alone do not flatten the curve](https://blog.zortax.de/images/a-critique-of-gpui/models-dark.png)

Floem’s reactivity does exactly what it promises. Changing a label runs 5 effects and spends 0.01 ms in reactive work, then about 14 ms in layout, because Taffy does not know that one leaf moved. (Floem also has a known bug where that edit never repaints, so treat this as a floor.) Vizia runs 4 bindings, then walks all 120,091 entities, for 46 ms. Slint writes an equal value in 4 µs, and takes 172 ms for a label, in a pass that walks about 30,000 items. Freya produces 5 mutations, then visits 57,050 layout nodes to recompute 14. GPUI beats most of them on this edit.

This is the distinction the argument depends on. All of those linear costs are in the engine respectively. Nothing in the application’s code requires them, so they can be fixed behind the same API without changing an application. zgui shows how far that goes: 0.69 ms for the label at 3,000 rows and about 0.26 ms per 1,000 rows, around 12 times flatter than GPUI. It is still not flat, and it is my framework and was tuned on this harness, so take these numbers with a grain of salt. In Iced and egui the linear cost is in the contract: the API says your code runs. No engine work can remove that, and app code has to change. GPUI probably sits somewhere in between, which is why gpui-fast can improve the situation to some extent.

At this point, I wan’t to share a more detailed benchmark matrix. Read the re-running columns down the first five rows and they barely move, because they do not depend on the edit. The signal columns move a lot, because the underlying implementations differ.

![Re-running makes most edits cost the same. Signals do not](https://blog.zortax.de/images/a-critique-of-gpui/comparison.png)![Re-running makes most edits cost the same. Signals do not](https://blog.zortax.de/images/a-critique-of-gpui/comparison-dark.png)

The bottom of that grid is also where the cost signals have becomes visible.

- **Creation.** Filtering at 3,000 live rows takes 56 ms in Iced, 67 in egui and 123 in GPUI, against 371 in zgui, 835 in Floem and 982 in Freya. On a virtualized list it is 1.0 ms in Iced and 7.2 in zgui. “Setting up the dependency graph is not free.”[6](https://blog.zortax.de/posts/a-critique-of-gpui#6)
- **Memory.** Resident memory usage for large scenes is generally higher the more a framework retains.
- **Effects.** Facebook moved away from two-way binding because “cascading updates” became hard to predict.[40](https://blog.zortax.de/posts/a-critique-of-gpui#40) That history is true, and Angular’s docs still say “Effects should be the last API you reach for.”[41](https://blog.zortax.de/posts/a-critique-of-gpui#41) Modern graphs are glitch-free. Careless effects are still difficult to reason about and quickly result in feedback loops or similar stuff.
- **Rust.** Signals rely on arena handles and `move` closures. Levien thinks such patterns “adapt poorly to Rust”.[3](https://blog.zortax.de/posts/a-critique-of-gpui#3) I think they adapt well enough, and GPUI’s entities are arena handles too, but, admittedly, it is not a free lunch.

## Conclusion

GPUI makes a full frame realitively cheap, and that makes it probably *good enough* for many use-cases. It is certainly enough for an editor whose expensive surface is handled by hand. For a large application it leaves a choice: keep the API people like and pay per view, or get precision by writing a signal graph out of entities. Elm and immediate mode have the same problem. Each is clearest in the small, and each loses the thing it was chosen for as precision becomes necessary, and, from my personal experience, that happens faster than people expect.

Tracked reads are where most of the field has ended up, whatever it started from. If that is where we are going, I think we should start there, and make the component a function that runs once.

## Method

- Numbers are medians of apply and settle from my unpublished harness, on one machine: i9-13900K, RTX 3080 Ti, Vulkan, Linux. Windows, compositor and input are not included. Frameworks’ maintainers have not reviewed the adapters.
- GPUI is at `ddc9eda925` and gpui-fast at `86a8228229`. zgui is at `082337db`. These were through September and November 2026, as the best of three passes.
- Cells marked † in the grid failed their correctness oracle: Floem’s label and keystroke edits, as noted above, and Slint’s font size. Every other charted cell passed.
- The 60-panel results are gpui-fast’s `retained_bench` with my additions, in test mode, so no GPU work is included. Retention off there is still gpui-fast.
- Figures from Zed’s plan are its authors’, from experimental branches.
- Rust examples were type-checked against the vendored frameworks. Vizia’s and Freya’s come from their own sources, shortened.

## References
