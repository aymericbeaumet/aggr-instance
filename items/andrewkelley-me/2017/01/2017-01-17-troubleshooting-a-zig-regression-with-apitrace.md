---
title: Troubleshooting a Zig Regression with apitrace
link: https://andrewkelley.me/post/troubleshooting-zig-regression-apitrace.html
source: andrewkelley-me
published: 2017-01-17T23:25:58Z
updated: 2017-01-17T23:25:58Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Troubleshooting a Zig Regression with apitrace The past three months I have spent rewriting Zig internals. Previously, the compiler looked like this: Source → Tokenize → Abstract Syntax Tree → Semantic Analysis → LLVM Codegen → Binary Now the compiler looks like this: Source → Tokenize → Abstract Syntax Tree → Intermediate Representation Code → Evaluation and Analysis → Intermediate Representation Code → LLVM Codegen → Binary This was a significant amount of work: 92 files changed, 22307 insertions(+), 15357 deletions(-) It took a while to get all 361 tests passing before I could merge back into master. Part of my testing process is making sure this Tetris game continues to work. Here''s a screenshot of it working, from master branch: Unfortunately, after my changes to Zig, the game looked like this: So, I ran both versions of the game with apitrace. This resulted in a handy diff: Here, it looks like both programs are issuing the same OpenGL commands except for different values to glUniform4fv. Aha! Let''s go see what''s going on there. After investigating this, it turned out that the glUniform4fv was simply for the piece color and since the game uses a random number for each piece, the two instances of the game started with different pieces. So, I made a small change to the Random Number Generator code... Before: fn getRandomSeed() -> %u32 { var seed : u32 = undefined; const seed_bytes = (&u8)(&seed)[0...4]; %return std.os.getRandomBytes(seed_bytes); return seed; } After: fn getRandomSeed() -> %u32 { return 4; } After this change, the glUniform4fv commands were sending the same data. Therefore, the difference must be in the "data blob" parameters sent in initialization. This led me to scrutinize this code: const rect_2d_vertexes = [][3]c.GLfloat { []c.GLfloat{0.0, 0.0, 0.0}, []c.GLfloat{0.0, 1.0, 0.0}, []c.GLfloat{1.0, 0.0, 0.0}, []c.GLfloat{1.0, 1.0, 0.0}, }; c.glGenBuffers(1, &sg.rect_2d_vertex_buffer); c.glBindBuffer(c.GL_ARRAY_BUFFER, sg.rect_2d_vertex_buffer); c.glBufferData(c.GL_ARRAY_BUFFER, 4 * 3 * @sizeOf(c.GLfloat), (&c_void)(&rect_2d_vertexes[0][0]), c.GL_STATIC_DRAW); I discovered the problem was &rect_2d_vertexes[0][0]. The compiler noticed that rect_2d_vertexes was a compile-time constant and therefore generated the 2D array data structure as static data. It therefore evaluated &rect_2d_vertexes[0][0] as a compile-time known expression as well. The problem was that each element in the rect_2d_vertexes referenced another array. The compile-time constant generation code emitted an independent array for the inner arrays, whereas we are expecting the pointer to point to a 2D array that contains all the data contiguously. So I updated the data structure of constant arrays to refer to their parents, added a test case to cover the change, and now the tetris game works again. Huzzah!'
content: extracted
html: 2017-01-17-troubleshooting-a-zig-regression-with-apitrace.html
preview:
  file: 2017-01-17-troubleshooting-a-zig-regression-with-apitrace.preview-343c59df5442.webp
  width: 186
  height: 256
  color: '#45433c'
images:
- source: http://superjoe.s3.amazonaws.com/blog-files/troubleshooting-zig-regression-apitrace/tetris-working.png
  original:
    file: 2017-01-17-troubleshooting-a-zig-regression-with-apitrace.image-4dfeb27a48cf.png
    width: 504
    height: 692
  variants:
  - file: 2017-01-17-troubleshooting-a-zig-regression-with-apitrace.image-7aef6159d0e5.webp
    width: 504
    height: 692
  color: '#474747'
- source: http://superjoe.s3.amazonaws.com/blog-files/troubleshooting-zig-regression-apitrace/tetris-not-working.png
  original:
    file: 2017-01-17-troubleshooting-a-zig-regression-with-apitrace.image-4c3a1815fc7f.png
    width: 504
    height: 692
  variants:
  - file: 2017-01-17-troubleshooting-a-zig-regression-with-apitrace.image-0eace9d2bad5.webp
    width: 504
    height: 692
  color: '#000000'
- source: http://superjoe.s3.amazonaws.com/blog-files/troubleshooting-zig-regression-apitrace/apitrace-diff.png
  original:
    file: 2017-01-17-troubleshooting-a-zig-regression-with-apitrace.image-f2c80fe1ce47.png
    width: 1195
    height: 1190
  variants:
  - file: 2017-01-17-troubleshooting-a-zig-regression-with-apitrace.image-6b9ad77f535f.webp
    width: 320
    height: 319
  - file: 2017-01-17-troubleshooting-a-zig-regression-with-apitrace.image-dbd4a9fc4400.webp
    width: 640
    height: 637
  - file: 2017-01-17-troubleshooting-a-zig-regression-with-apitrace.image-69cce72eb069.webp
    width: 1195
    height: 1190
  color: '#020202'
---

The past three months I have spent rewriting [Zig](http://ziglang.org/) internals.

Previously, the compiler looked like this:

Source → Tokenize → Abstract Syntax Tree → Semantic Analysis → LLVM Codegen → Binary

Now the compiler looks like this:

Source → Tokenize → Abstract Syntax Tree → Intermediate Representation Code → Evaluation and Analysis → Intermediate Representation Code → LLVM Codegen → Binary

This was a significant amount of work:

```
92 files changed, 22307 insertions(+), 15357 deletions(-)
```

It took a while to get all 361 tests passing before I could merge back into master.

Part of my testing process is making sure this [Tetris game](https://github.com/andrewrk/tetris) continues to work. Here's a screenshot of it working, from master branch:

![](http://superjoe.s3.amazonaws.com/blog-files/troubleshooting-zig-regression-apitrace/tetris-working.png)

Unfortunately, after my changes to Zig, the game looked like this:

![](http://superjoe.s3.amazonaws.com/blog-files/troubleshooting-zig-regression-apitrace/tetris-not-working.png)

So, I ran both versions of the game with [apitrace](http://apitrace.github.io/). This resulted in a handy diff:

![](http://superjoe.s3.amazonaws.com/blog-files/troubleshooting-zig-regression-apitrace/apitrace-diff.png)

Here, it looks like both programs are issuing the same OpenGL commands except for different values to `glUniform4fv`. Aha! Let's go see what's going on there.

After investigating this, it turned out that the `glUniform4fv` was simply for the piece color and since the game uses a random number for each piece, the two instances of the game started with different pieces.

So, I made a small change to the Random Number Generator code...

Before:

```
fn getRandomSeed() -> %u32 {
    var seed : u32 = undefined;
    const seed_bytes = (&u8)(&seed)[0...4];
    %return std.os.getRandomBytes(seed_bytes);
    return seed;
 }
```

After:

```
fn getRandomSeed() -> %u32 {
    return 4;
 }
```

After [this change](http://xkcd.com/221/), the `glUniform4fv` commands were sending the same data. Therefore, the difference **must** be in the "data blob" parameters sent in initialization.

This led me to scrutinize this code:

```
const rect_2d_vertexes = [][3]c.GLfloat {
    []c.GLfloat{0.0, 0.0, 0.0},
    []c.GLfloat{0.0, 1.0, 0.0},
    []c.GLfloat{1.0, 0.0, 0.0},
    []c.GLfloat{1.0, 1.0, 0.0},
};
c.glGenBuffers(1, &sg.rect_2d_vertex_buffer);
c.glBindBuffer(c.GL_ARRAY_BUFFER, sg.rect_2d_vertex_buffer);
c.glBufferData(c.GL_ARRAY_BUFFER, 4 * 3 * @sizeOf(c.GLfloat), (&c_void)(&rect_2d_vertexes[0][0]), c.GL_STATIC_DRAW);
```

I discovered the problem was `&rect_2d_vertexes[0][0]`. The compiler noticed that `rect_2d_vertexes` was a compile-time constant and therefore generated the 2D array data structure as static data. It therefore evaluated `&rect_2d_vertexes[0][0]` as a compile-time known expression as well.

The problem was that each element in the `rect_2d_vertexes` referenced another array. The compile-time constant generation code emitted an independent array for the inner arrays, whereas we are expecting the pointer to point to a 2D array that contains all the data contiguously.

So I updated the data structure of constant arrays to refer to their parents, added a test case to cover the change, and now the tetris game works again. Huzzah!

Thanks for reading my blog post.
