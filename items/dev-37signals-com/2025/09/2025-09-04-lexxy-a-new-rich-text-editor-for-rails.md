---
title: 'Lexxy: A new rich text editor for Rails'
link: https://dev.37signals.com/announcing-lexxy-a-new-rich-text-editor-for-rails/
source: dev-37signals-com
published: 2025-09-04T17:00:00Z
updated: 2025-09-04T17:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Jorge Manrubia
summary: A better Action Text.
content: extracted
html: 2025-09-04-lexxy-a-new-rich-text-editor-for-rails.html
preview:
  file: 2025-09-04-lexxy-a-new-rich-text-editor-for-rails.preview-dd6c53536c7b.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#080808'
images:
- source: https://dev.37signals.com/assets/images/opengraph/announcing-lexxy-a-new-rich-text-editor-for-rails.png
  original:
    file: 2025-09-04-lexxy-a-new-rich-text-editor-for-rails.image-8d306fd55107.png
    width: 2400
    height: 1260
  color: '#000000'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/announcing-lexxy-a-new-rich-text-editor-for-rails.png
---

Today, we are introducing [Lexxy](https://github.com/basecamp/lexxy/), a new rich text editor for Action Text. It’s based on [Lexical](https://lexical.dev/) — Meta’s text editing framework — and it brings a much better text editing experience to Rails:

- Good HTML semantics. Paragraphs are real `<p>` tags, as they should be.
- Markdown support: shortcuts, auto-formatting on paste.
- Real-time code syntax highlighting.
- Create links by pasting URLs on selected text.
- Configurable prompts. Support for mentions and other interactive prompts with multiple loading and filtering strategies.
- Preview attachments like PDFs and Videos in the editor.
- Works seamlessly with Action Text and Active Storage.

We created Lexxy because Trix was falling short of expectations in certain areas, and we encountered technical barriers when attempting to offer the experience we wanted. Lexxy comes with a bunch of juicy improvements, but more than that, we now have a fantastic foundation to build on top of.

Lexxy will also bring a great improvement to [Action Text](https://guides.rubyonrails.org/action_text_overview.html): we will let you configure the editor in Action Text just like you configure the database in Active Record. This will open the door to integrating other editors in Rails.

Text editing is central to our products. We believe Lexxy will let us deliver the editing experience we want. We are launching an early beta today, [give it a try](https://github.com/basecamp/lexxy/)!

Sign up to get posts via email,\
 or [grab the RSS feed](https://dev.37signals.com/feed/posts.xml).
