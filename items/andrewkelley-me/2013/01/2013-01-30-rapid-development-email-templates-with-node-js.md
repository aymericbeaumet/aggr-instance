---
title: Rapid Development Email Templates with Node.js
link: https://andrewkelley.me/post/swig-email-templates.html
source: andrewkelley-me
published: 2013-01-30T12:00:00Z
updated: 2013-01-30T12:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Rapid Development Email Templates with Node.js Contents Sending Automated Emails in Node.js node-email-templates Gotchas Fundamental Flaws Includes VS Template Inheritance Sharing CSS Dummy Context Conclusion Sending Automated Emails in Node.js When I was tasked with solving the age-old problem of sending automatic email messages to our users at Indaba Music, I surveyed the Node.js landscape to find out the state of affairs. I was pleased to immediately discover node-email-templates and Nodemailer, and within two days had a proof of concept email notification server deployed to production. node-email-templates helps organize your project and make it easy to render templates for sending via email by using juice to inline css. Nodemailer does the actual email dispatching - given an email dispatch service, a subject, to, and body, Nodemailer will get your mail to its destination. Nodemailer is a wonderful piece of software. It worked, and continues to work, exactly as advertised and without any hiccups. It even has a convenient API that makes integration with common email dispatchers, such as SendGrid (which I also recommend), quite painless. Unfortunately this journey was not without a few obstacles that node-email-templates provided for me to solve. What follows is an explanation of the bumps along the road that caused me to write these modules to improve the state of email templates in node.js: boost swig-dummy-context swig-email-templates More on these in a bit. node-email-templates Gotchas Assume that I have 2 templates named reminder and notice. node-email-templates requires your project to have a folder structure that looks like this: ./templates/reminder/html.ejs ./templates/reminder/text.ejs ./templates/reminder/style.css ./templates/notice/html.ejs ./templates/notice/text.ejs ./templates/notice/style.css There are several problems here. There are some module smells: Regardless of whether or not you need text version of a template, you must have the text.ejs file there or node-email-templates will throw an error. This project is poorly maintained. As of this writing, daeq submitted a pull request to fix the above problem 3 months ago, and despite a promise to merge it, niftylettuce still has not done so. But there are also some fundamental problems with the approach that the module takes. Fundamental Flaws This flavor of ejs is limited. You can do includes, but not layouts or template inheritance, which is where the true value of using templates comes in. The html templates have no way of sharing css between them. Because ejs depends on using eval, it is impossible to, given a template, create a dummy context with which to generate a preview of the template. More on this later. Includes VS Template Inheritance To demonstrate the template inheritance problem, let me give you 2 versions of a template, one using ejs with includes, and one using swig with template inheritance: ejs includes notice.ejs <% include header %> <div> <p>Hey <%= username %>,</p> <p>This is a notice that your offer is about to expire.</p> </div> <% include footer %> reminder.ejs <% include header %> <div> <p>Hey <%= username %>,</p> <p>Don''t forget! You probably wanted to do that thing.</p> </div> <% include footer %> header.ejs <div> <img src="logo.png"> </div> footer.ejs <div> Super Cool &amp; Co., LLC. <a>Privacy Policy</a> </div> swig template inheritance reminder.html {% extends "base.html" %} {% block content %} <p>Don''t forget! You probably wanted to do that thing.</p> {% endblock %} notice.html {% extends "base.html" %} {% block content %} <p>This is a notice that your offer is about to expire.</p> {% endblock %} base.html <div> <img src="logo.png"> </div> <div> <p>Hey {{ username }},</p> {% block content %} {% endblock %} </div> <div> Super Cool &amp; Co., LLC. <a>Privacy Policy</a> </div> Template Inheritance Wins This is an oversimplified example, but even so it starts to become obvious why template inheritance is superior to includes. You can also have includes in swig, by the way. Sharing CSS node-email-templates uses juice to inline css. Give juice html and css, and it returns html with the css inlined on each element for maximum email client compatibility. This setup seems good at first, but it is crippled by the fact that templates are completely unable to share css. Each template has its own independent style.css file. It is not node-email-templates''s fault. Given the way that juice works, it isn''t really possible to share css. This is where boost comes in. boost depends on juice and adds 2 key features that make sharing CSS possible. Ability for the html to have <link rel="stylesheet"> tags which are resolved correctly and have the resulting CSS applied. Ability to have <style>...</style> elements and have that CSS applied as well. When you add this capability with template inheritance, sharing CSS becomes a solved problem. For example: base.html <html> <head> {% block css %} <link rel="stylesheet" href="base.css"> {% endblock %} </head> <body> {% block content %} {% endblock %} </body> </html> reminder.html {% extends "base.html" %} {% block css %} {% parent %} <link rel="stylesheet" href="reminder.css"> {% endblock %} {% block content %} <h1>Reminder</h1> <p>Don''t forget!</p> {% endblock %} notice.html {% extends "base.html" %} {% block css %} {% parent %} <link rel="stylesheet" href="notice.css"> {% endblock %} {% block content %} <h1>Notice</h1> <p>This is a notice.</p> {% endblock %} Dummy Context In order to rapidly build email templates, we need to see what we are building as we build it. Because you need to supply a template with a context in order to render it and see it, this makes seeing what you are doing while you build templates a two step process. We can remove this extra step by taking advantage of swig-dummy-context, a module I wrote which, given a swig template, gives you a "dummy" context - a fill-in-the-blank structure you can use to immediately preview your template. Given: <div> {{ description }} </div> {% if articles %} <ul> {% for article in articles %} <li>{{ article.name }}</li> {% endfor %} </ul> {% else %} <p>{{ defaultText }}</p> {% endif %} swig-dummy-context produces: { "description": "description", "articles": { "name": "name" }, "defaultText": "defaultText" } And if you render the template with the generated dummy context, you get: <div> description </div> <ul> <li>name</li> </ul> Conclusion swig-email-templates gives you all the ingredients you need to build well-organized templates and gives you the tooling that you need to build a live preview tool. At Indaba Music we have such a tool. It lets you select a template from the templates folder and fill in the substitutions to preview how an email will look. To be extra sure of how an email will render, you can use the tool to send a test email to your email address. This tool is currently private as it is not decoupled from SendGrid or even polished up for 3rd party use at all; however if there is sufficient interest I may open source it.'
content: extracted
html: 2013-01-30-rapid-development-email-templates-with-node-js.html
---

## Contents

1. [Sending Automated Emails in Node.js](https://andrewkelley.me/post/swig-email-templates.html#automated-emails-nodejs)
2. [node-email-templates Gotchas](https://andrewkelley.me/post/swig-email-templates.html#nodeemailtemplates_gotchas)
3. [Fundamental Flaws](https://andrewkelley.me/post/swig-email-templates.html#fundamental_flaws)
   1. [Includes VS Template Inheritance](https://andrewkelley.me/post/swig-email-templates.html#includes_vs_template_inheritance)
   2. [Sharing CSS](https://andrewkelley.me/post/swig-email-templates.html#sharing_css)
   3. [Dummy Context](https://andrewkelley.me/post/swig-email-templates.html#dummy_context)
4. [Conclusion](https://andrewkelley.me/post/swig-email-templates.html#conclusion)

## Sending Automated Emails in Node.js

When I was tasked with solving the age-old problem of sending automatic email messages to our users at [Indaba Music](http://indabamusic.com/), I surveyed the [Node.js](http://nodejs.org/) landscape to find out the state of affairs.

I was pleased to immediately discover [node-email-templates](https://github.com/niftylettuce/node-email-templates/) and [Nodemailer](https://github.com/andris9/Nodemailer), and within two days had a proof of concept email notification server deployed to production.

**node-email-templates** helps organize your project and make it easy to render templates for sending via email by using [juice](https://github.com/LearnBoost/juice) to [inline css](http://www.campaignmonitor.com/css/).

**Nodemailer** does the actual email dispatching - given an email dispatch service, a subject, to, and body, Nodemailer will get your mail to its destination.

Nodemailer is a wonderful piece of software. It worked, and continues to work, exactly as advertised and without any hiccups. It even has a convenient API that makes integration with common email dispatchers, such as [SendGrid](http://sendgrid.com) (which I also recommend), quite painless.

Unfortunately this journey was not without a few obstacles that node-email-templates provided for me to solve.

What follows is an explanation of the bumps along the road that caused me to write these modules to improve the state of email templates in node.js:

- [boost](https://github.com/andrewrk/boost)
- [swig-dummy-context](https://github.com/andrewrk/swig-dummy-context)
- [swig-email-templates](https://github.com/andrewrk/swig-email-templates)

More on these in a bit.

## node-email-templates Gotchas

Assume that I have 2 templates named `reminder` and `notice`.

**node-email-templates** requires your project to have a folder structure that looks like this:

```
./templates/reminder/html.ejs
./templates/reminder/text.ejs
./templates/reminder/style.css
./templates/notice/html.ejs
./templates/notice/text.ejs
./templates/notice/style.css
```

There are several problems here. There are some module smells:

- Regardless of whether or not you need text version of a template, you must have the `text.ejs` file there or node-email-templates will throw an error.
- This project is poorly maintained. As of this writing, [daeq](https://github.com/daeq) submitted a [pull request](https://github.com/niftylettuce/node-email-templates/pull/17) to fix the above problem 3 months ago, and despite a promise to merge it, [niftylettuce](https://github.com/niftylettuce/) still has not done so.

But there are also some fundamental problems with the approach that the module takes.

## Fundamental Flaws

- This flavor of ejs is limited. You can do includes, but not layouts or [template inheritance](https://docs.djangoproject.com/en/dev/topics/templates/#template-inheritance), which is where the true value of using templates comes in.
- The html templates have *no way* of sharing css between them.
- Because ejs depends on using `eval`, it is impossible to, given a template, create a dummy context with which to generate a preview of the template. More on this later.

### Includes VS Template Inheritance

To demonstrate the template inheritance problem, let me give you 2 versions of a template, one using ejs with includes, and one using [swig](https://github.com/paularmstrong/swig/) with template inheritance:

#### ejs includes

##### notice.ejs

```
<% include header %>
<div>
  <p>Hey <%= username %>,</p>
  <p>This is a notice that your offer is about to expire.</p>
</div>
<% include footer %>
```

##### reminder.ejs

```
<% include header %>
<div>
  <p>Hey <%= username %>,</p>
  <p>Don't forget! You probably wanted to do that thing.</p>
</div>
<% include footer %>
```

```
<div>
  <img src="logo.png">
</div>
```

```
<div>
  Super Cool &amp; Co., LLC.
  <a>Privacy Policy</a>
</div>
```

#### swig template inheritance

##### reminder.html

```
{% extends "base.html" %}

{% block content %}
  <p>Don't forget! You probably wanted to do that thing.</p>
{% endblock %}
```

##### notice.html

```
{% extends "base.html" %}

{% block content %}
  <p>This is a notice that your offer is about to expire.</p>
{% endblock %}
```

##### base.html

```
<div>
  <img src="logo.png">
</div>
<div>
  <p>Hey {{ username }},</p>
  {% block content %}
  {% endblock %}
</div>
<div>
  Super Cool &amp; Co., LLC.
  <a>Privacy Policy</a>
</div>
```

#### Template Inheritance Wins

This is an oversimplified example, but even so it starts to become obvious why template inheritance is superior to includes.

You can also have includes in swig, by the way.

### Sharing CSS

**node-email-templates** uses [juice](https://github.com/LearnBoost/juice) to inline css. Give juice html and css, and it returns html with the css inlined on each element for [maximum email client compatibility](http://www.campaignmonitor.com/css/).

This setup seems good at first, but it is crippled by the fact that templates are completely unable to share css. Each template has its own independent `style.css` file.

It is not **node-email-templates**'s fault. Given the way that juice works, it isn't really possible to share css.

This is where [boost](https://github.com/andrewrk/boost) comes in. **boost** depends on juice and adds 2 key features that make sharing CSS possible.

- Ability for the html to have `<link rel="stylesheet">` tags which are resolved correctly and have the resulting CSS applied.
- Ability to have `<style>...</style>` elements and have that CSS applied as well.

When you add this capability with template inheritance, sharing CSS becomes a solved problem. For example:

#### base.html

```
<html>
<head>
  {% block css %}
    <link rel="stylesheet" href="base.css">
  {% endblock %}
</head>
<body>
  {% block content %}
  {% endblock %}
</body>
</html>
```

#### reminder.html

```
{% extends "base.html" %}

{% block css %}
  {% parent %}
  <link rel="stylesheet" href="reminder.css">
{% endblock %}

{% block content %}
  <h1>Reminder</h1>
  <p>Don't forget!</p>
{% endblock %}
```

#### notice.html

```
{% extends "base.html" %}

{% block css %}
  {% parent %}
  <link rel="stylesheet" href="notice.css">
{% endblock %}

{% block content %}
  <h1>Notice</h1>
  <p>This is a notice.</p>
{% endblock %}
```

### Dummy Context

In order to rapidly build email templates, we need to see what we are building as we build it.

Because you need to supply a template with a context in order to render it and see it, this makes seeing what you are doing while you build templates a two step process.

We can remove this extra step by taking advantage of [swig-dummy-context](https://github.com/andrewrk/swig-dummy-context), a module I wrote which, given a swig template, gives you a "dummy" context - a fill-in-the-blank structure you can use to immediately preview your template.

Given:

```
<div>
  {{ description }}
</div>
{% if articles %}
  <ul>
  {% for article in articles %}
    <li>{{ article.name }}</li>
  {% endfor %}
  </ul>
{% else %}
  <p>{{ defaultText }}</p>
{% endif %}
```

swig-dummy-context produces:

```
{
  "description": "description",
  "articles": {
    "name": "name"
  },
  "defaultText": "defaultText"
}
```

And if you render the template with the generated dummy context, you get:

```
<div>
  description
</div>
<ul>
  <li>name</li>
</ul>
```

## Conclusion

[swig-email-templates](https://github.com/andrewrk/swig-email-templates) gives you all the ingredients you need to build well-organized templates and gives you the tooling that you need to build a live preview tool.

At Indaba Music we have such a tool. It lets you select a template from the templates folder and fill in the substitutions to preview how an email will look. To be extra sure of how an email will render, you can use the tool to send a test email to your email address.

This tool is currently private as it is not decoupled from SendGrid or even polished up for 3rd party use at all; however if there is sufficient interest I may open source it.

Thanks for reading my blog post.
