---
title: Safari introduces Adaptive Dark Mode in 27.3
link: https://webkit.org/blog/18541/introducing-adaptive-dark-mode/
source: hnrss-org
published: 2026-11-03T21:39:05Z
updated: 2026-11-03T21:39:05Z
first_seen: 2026-11-04T00:52:13.198927099Z
authors:
- paddingtone
content: extracted
html: 2026-11-03-safari-introduces-adaptive-dark-mode-in-27-3.html
remote_preview:
  url: https://webkit.org/wp-content/uploads/Demo-of-Adaptive-Dark-Mode_light.png
  alt: A demo website in light mode on the left and dark mode on the right. It's a complex site with lots of UI full of bits of color that's transformed by Adaptive Dark Mode.
---

We’re excited to announce Adaptive Dark Mode in Safari 27.3 beta. When enabled by a user, Safari adapts light websites and presents them as dark.

In the spring of 2019, Safari 12.1 became the first browser to support dark mode on the web. A lot of people love using dark mode, but not every website has a dark version. Almost [40% of web page loads](https://chromestatus.com/metrics/feature/timeline/popularity/3580) are from users in dark mode, and yet [only 10%](https://chromestatus.com/metrics/feature/timeline/popularity/3707) of page loads support it. If you love dark mode, you’ve probably hit a site that felt particularly [bright](https://www.tiktok.com/@syntaxfm/video/7307721014891367711). Adaptive Dark Mode is designed to close that gap, providing a consistent dark experience across the web.

![A demo website in light mode on the left and dark mode on the right. It's a complex site with lots of UI full of bits of color that's transformed by Adaptive Dark Mode.](https://webkit.org/wp-content/uploads/Demo-of-Adaptive-Dark-Mode_light.png)

You can try it today in Safari 27.3 beta. To enable, first make sure your system appearance is set to dark. Then browse to a website with a bright background. Safari will offer a preview of Adaptive Dark Mode. Tap ‘Try’.

For websites that signal support of dark mode with `<meta name="color-scheme" content="light dark">` in HTML or `color-scheme: light dark` in CSS, Adaptive Dark Mode does not change a thing. For the rest of the web, it quickly analyzes the page, looking at colors and estimating whether it’s predominantly light or dark. If the page generally has a dark background with light text, nothing is changed. When a page has a light appearance, Adaptive Dark Mode adapts the page to be dark.

## How does Adaptive Dark Mode work?

At the core of Adaptive Dark Mode is the transformation of color. Sometimes colors are simply decorative, while other times they have semantic meaning. Adaptive Dark Mode determines when to invert colors so elements remain visible or when to leave colors unchanged.

While creating Adaptive Dark Mode, we developed a color matrix to invert the luminosity of colors while preserving hue. It’s used to make light backgrounds dark, and dark text light. When transforming text color, Adaptive Dark Mode aims for sufficient contrast as defined by the [WCAG 4.5:1 contrast ratio](https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum.html). Light CSS gradients are also transformed to dark.

Adaptive Dark Mode makes careful judgments so transformed content looks good and stays usable: buttons have contrast against backgrounds, text is visible over images, and hover menus look clear.

It takes the same care with images. Most are left unchanged, preserving their integrity. But many icons and logos can lose contrast when the background color behind them is inverted. So Adaptive Dark Mode inspects SVG images and transforms colors when needed. For bitmap images, it analyzes the pixel histograms and inverts simple black-and-white logos.

![Side by side screenshots of the Interop 2026 dashboard in Safari. On the left, the original light mode website. On the right, the site transformed by Adaptive Dark Mode. The graph still matches light mode (untransformed) but everything else on the page is dark.](https://webkit.org/wp-content/uploads/Interop-dashboard-with-Adaptive-Dark-Mode_light-scaled.png)

Effects like masks and `mix-blend-mode` can create undesirable results on an element whose colors have been transformed, so in some cases Adaptive Dark Mode turns such effects off. Also, `<canvas>` content is left unchanged, since canvases contain a wide range of content that’s hard to predict.

It’s precisely because Adaptive Dark Mode is happening deep within the WebKit browser engine that we are able to find creative ways to solve the most complex challenges, apply the transformations with great speed and power efficiency, all while rigorously protecting user privacy.

## Making your site work even better

In general, a web page will work well with Adaptive Dark Mode if it follows good modern web development practices.

If your site still uses older techniques, small updates can help. For logos and icons, swap bitmap images for SVG. Use solid or gradient colors in CSS instead of tiled background images. Replace image sprites with modern CSS for rounded corners, shadows, and button states. Create content images with transparent backgrounds instead of baking the page background color into the image. Add padding to containers to prevent crowding.

Perhaps you want a specific dark mode experience for a certain portion of the page. Apply [`color-scheme: light dark`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/color-scheme) to the relevant element and add your own dark styles.

## Building your own dark mode

For complete control over how your site looks in dark mode, add full support. It’s a terrific time to do so. It’s never been easier.

![The demo website from above, only now, with a third browser window showing what that website might look like with a custom dark mode.](https://webkit.org/wp-content/uploads/Demo-of-Adaptive-Dark-Mode-plus-custom-dark-mode_light-scaled.png)\
Here’s an example of a website in light mode (left) and with Adaptive Dark Mode applied (center). If the site creates a custom dark mode, it could look like the third version (right).

All the web technology needed to fully support dark mode is widely available today, including:

- [`<meta name="color-scheme" content="light dark">`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/meta/name/color-scheme)
- [`:root { color-scheme: light dark }`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/color-scheme)
- [`@media (prefers-color-scheme: dark) { }`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/prefers-color-scheme)
- [`<picture><source media="(prefers-color-scheme: dark)">`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/picture)
- [`light-dark()`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/color_value/light-dark)

Newer CSS functions like [`color-mix()`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/color_value/color-mix) and [`contrast-color()`](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/color_value/contrast-color), plus popular techniques for organizing design systems with CSS variables provide additional options and flexibility.

Be sure to declare dark mode support using the `color-scheme` meta tag in your HTML head and/or by setting the `color-scheme` property on `:root` or `body` in your CSS. Don’t wait to inject this information later via JavaScript. This best practice helps ensure your site is rendered correctly as fast as possible.

## Using the “light only” color scheme

If you would like your web app or website to not be transformed by Adaptive Dark Mode, you can communicate this using `color-scheme`. The HTML `<meta name="color-scheme" content="light only">` tag lets browsers know that 1) this page supports `light` mode and 2) should `only` be displayed in light mode. The `color-scheme: light only` rule does the same thing, but with the flexibility of CSS, allowing you to just target specific elements on the page — or of course, you can target the whole page with `:root { }`. (Note: the ‘only’ keyword is not supported in Safari 27.3 beta 1. Check back as Adaptive Dark Mode evolves through the beta process.)

## Protecting user privacy

Apple firmly believes user privacy is a fundamental human right. If websites could detect when a user has enabled Adaptive Dark Mode, that would increase the amount of fingerprintable information and weaken privacy protections against trackers.

As a result, Safari prevents websites from detecting when Adaptive Dark Mode has been applied. JavaScript that queries CSS colors will get the original authored colors, not the transformed ones. Any images drawn into canvas render in their original form.

## Feedback

If you manage a website or web app, and would like to discuss how Adaptive Dark Mode is adapting your project, feel free to reach out at [web-evangelist@apple.com](mailto:web-evangelist@apple.com).

If you try out Adaptive Dark Mode as a user, and find a site that does not work well, we’d love for you to file a report with [Feedback Assistant](https://feedbackassistant.apple.com/). (Learn more about [filing Feedback](https://developer.apple.com/bug-reporting/)).
