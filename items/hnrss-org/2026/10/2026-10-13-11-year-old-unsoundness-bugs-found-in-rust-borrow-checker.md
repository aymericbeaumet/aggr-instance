---
title: 11 year old unsoundness bugs found in Rust borrow checker
link: https://github.com/rust-lang/rust/issues/164250
source: hnrss-org
published: 2026-10-13T10:27:00Z
updated: 2026-10-13T10:27:00Z
first_seen: 2026-10-13T18:08:43.541012300Z
authors:
- formerly_proven
content: extracted
html: 2026-10-13-11-year-old-unsoundness-bugs-found-in-rust-borrow-checker.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50069499
  num_comments: 323
  points: 558
---

Note

Given that this issue has been shared online and is currently getting quite a few visitors, here's some additional context.

Rust's type system is intended to prevent memory unsafety. Upholding this promise is something we take very seriously, which is why we've been working on things like [the next-generation trait solver](https://goals.rust-lang.org/2026/next-solver.html), [the region constraint rework](https://goals.rust-lang.org/2026/next-gen-region-constraints.html), and more generally [Project Zero](https://goals.rust-lang.org/2026/roadmap-project-zero.html). It is a large priority of the Types Team to fix all type system unsoundnesses and to work towards there being a solid foundation to minimize the risk of unknown ones.

This bug is a `P-high` issue and would have been `P-critical` regardless of whether people encountered it in the wild. The reason that it is "only `P-high`" is that it's 11 years old. A bug like this which has not been found during the last 11 years cannot actually be that easy to hit.

Looking at the bug itself, it is also very likely that this unsoundness has not caused memory unsafety by accident. While it looks easy to trigger, to exploit this you need an intentionally malicious `Drop` implementation.

\~ [@lcnr](https://github.com/lcnr)

* * *

In the following code:

```rust
use std::sync::Mutex;
static KEPT: Mutex<Vec<Box<dyn std::any::Any + Send>>> = Mutex::new(Vec::new());
struct Q<T: 'static + Send>(Option<T>);
impl<T: 'static + Send> Drop for Q<T> {
    fn drop(&mut self) { if let Some(t) = self.0.take() { KEPT.lock().unwrap().push(Box::new(t)); } }
}
fn main() {
    {
        let l = String::from("hello world, this is long enough");
        let mut q = Q(None);
        q.0 = Some(&l);
    }
    let _z = String::from("XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX");
    let k = KEPT.lock().unwrap();
    println!("{}", k[0].downcast_ref::<&'static String>().unwrap());
}
```

I expected to see this happen: program is not accepted

Instead, this happened: program compiles and prints `XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX`. With asan, the program will crash with a `AddressSanitizer: stack-use-after-scope`

### Meta

Issue on current nightly, and stables at least down to 1.85.0

Godbolt with asan: [https://rust.godbolt.org/z/83MvfbKde](https://rust.godbolt.org/z/83MvfbKde)\
 Godbolt without asan: [https://rust.godbolt.org/z/GsGdqqMWe](https://rust.godbolt.org/z/GsGdqqMWe)
