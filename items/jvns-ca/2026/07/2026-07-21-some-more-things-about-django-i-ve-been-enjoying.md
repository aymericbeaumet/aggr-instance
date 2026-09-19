---
title: Some more things about Django I've been enjoying
link: https://jvns.ca/blog/2026/07/21/more-nice-django-things/
source: jvns-ca
published: 2026-07-21T00:00:00Z
updated: 2026-07-21T00:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-07-21-some-more-things-about-django-i-ve-been-enjoying.html
---

Hello! I’m on a funny journey right now where I’m trying to learn how to make websites in a sort of 2010 style, where I have an SQL database and render some HTML on the backend.

It’s kind of an interesting journey because it doesn’t necessarily feel “easy” to me to make websites in this way: I never learned how to do it in the 2000s or 2010s, and there’s a lot I need to learn.

So here are some Django features that make building this kind of site feel more achievable than when I was trying and failing to use Go’s standard library or Flask. And I’ll talk about a couple of issues with Django I’ve run into.

### [why learn to make websites like it’s 2010?](https://jvns.ca/blog/2026/07/21/more-nice-django-things/#why-learn-to-make-websites-like-it-s-2010)

Previously the toolkit I felt confident with for making websites was:

- static site generators (like for this blog)
- static sites that do some fun stuff with Javascript (like this [sql playground](https://sql-playground.wizardzines.com/))
- simple Vue.js single page apps with either a Lambda as a backend or a Go backend (like [mess with dns](https://messwithdns.net/))

I really liked this frontend-heavy approach for these super simple applications but when I started thinking about making something with a lot of different pages (instead of literally just one page), I didn’t feel so excited about the options I saw that involved a lot of frontend code. So I figured I’d try the backend.

Writing a backend-focused site that uses as little JS as possible feels the same to me in a way as writing a single-page JS website that does as little on the backend as possible, even though they might seem like opposites. In both cases I’m just trying to keep as much of the logic as possible in one place.

Now for some thoughts about Django!

### [I’m enjoying query builders](https://jvns.ca/blog/2026/07/21/more-nice-django-things/#i-m-enjoying-query-builders)

I learned that I can define a “query set” class in Django with a bunch of methods with different `WHERE` statements I might want to use while constructing a query:

Here’s how I use it in my view code once I’ve defined what all the methods mean:

```
Events.objects.approved()
  .for_tab(tab)
  .with_festivals(tab_params.festival_slugs)
  .is_free(tab_params.free)
  .is_outdoors(tab_params.outdoors)
```

and here’s how I define the methods:

```
class EventQuerySet(SearchableQuerySetMixin, models.QuerySet):
    def approved(self):
        return self.filter(approved_at__isnull=False)

    def future(self):
        today = timezone.localdate()
        return self.filter(end__gt=self._midnight(today))

    def with_tags(self, tags):
        if tags:
            return self.filter(tags__name__in=tags).distinct()
        return self
```

The syntax for defining the filters isn’t my favourite, but I spend most of my time just using the methods, and it feels super readable and nice to use, and it makes me want to look into other query builder libraries in the future. In the past I thought “I know SQL, who needs a query builder?”, but this kind of structure does make it really nice to read.

I found an example of someone who [wrote their own small query builder in Python](https://github.com/lemon24/reader/blob/15121f667a6f2e388f0072a3fcd715f533883899/src/reader/_sql_utils.py) that I want to read later to think about whether I would enjoy using a more minimal version of this.

### [the template filters are awesome](https://jvns.ca/blog/2026/07/21/more-nice-django-things/#the-template-filters-are-awesome)

There are a bunch of little quality of life [filters available in Django templates](https://docs.djangoproject.com/en/6.0/ref/templates/builtins/) that are super useful for generating HTML. The ones I’ve used so far are:

- translating plain text URLs into links, or line breaks into `<br>` (`{{ event.description|urlize|linebreaksbr }}`)
- formatting dates (`{{ row.date|date:"M j" }}`)
- `json_script`, which takes a Python dictionary and automatically converts it to JSON and inserts it into the HTML as a `<script>` tag in a safe way

These are all small things individually but I feel like it makes a big difference somehow to just have them available.

### [`querystring` is cool](https://jvns.ca/blog/2026/07/21/more-nice-django-things/#querystring-is-cool)

I think my favourite template filter is `querystring`: in this site sometimes we use filters like `?date=2026-06-01` to decide what’s displayed. `querystring` that will make a link to the same query string with one change, like this to link to the previous date:

```
<a href="{% querystring date=nav.prev_date%}">
```

Or to remove the `outdoors` parameter:

```
<a href="{% querystring outdoors=None %}">
```

### [automatic database migrations are still great](https://jvns.ca/blog/2026/07/21/more-nice-django-things/#automatic-database-migrations-are-still-great)

I still really love Django’s automatic database system. It’s amazing to be able to just edit a model to add a new field or whatever, and then Django automatically generates the migration.

So far we have done 19 database migrations and I think there will probably be more! It makes a huge difference for me to be able to just easily change the database as my understanding of the problem changes.

### [I do not want to organize my code with inheritance](https://jvns.ca/blog/2026/07/21/more-nice-django-things/#i-do-not-want-to-organize-my-code-with-inheritance)

Django’s documentation sometimes offers the option of using class-based views and inheritance to organize the code in your views. For example I have four views that share a lot of code, and I could use inheritance to manage that by defining some kind of parent class and then having my other views inherit from it.

I tried it out and I did not enjoy the experience of using inheritance to share code between views. I switched to using functions instead, sort of how [this post](https://spookylukey.github.io/django-views-the-right-way/) advocates, and that was a lot more straightforward. I’ve never had a good experience using inheritance in Python and I don’t think I’ll try to use it again.

But I don’t mind using inheritance to use the interfaces Django itself provides: for example if I want to define a query set I need to write something like `class EventQuerySet(SearchableQuerySetMixin, models.QuerySet)`. I don’t think too hard about it and it seems to work.

(as a meta comment: I’ve been working on talking about my programming opinions by just saying “THING does not feel good to me, I prefer OTHER THING instead”. That post I linked to says that function-based views are the “right way”. I’m not very invested in whether it’s “right”, but it’s validating to know that other people feel similarly to me about inheritance)

### [I don’t know how to think about Django performance](https://jvns.ca/blog/2026/07/21/more-nice-django-things/#i-don-t-know-how-to-think-about-django-performance)

At some point the LLM scrapers discovered our site, and started sending us maybe 10 requests per second. I blocked them which is working for now, but it made me think about what the site’s capacity is. I’m used to writing Go backends where the performance situation is pretty straightforward (usually everything is just fast enough), and a Django site is very different.

Some light load testing (with (`ab -n 1000 -c 1`) shows that right now we can serve about 2-3 requests per second (on a ~$10/month VM).

It’s tempting for me to go down a rabbit hole where I do a bunch of profiling to figure out what’s slow and try to make it faster (there’s [py-spy](https://github.com/benfred/py-spy) for that, and py-spy is great and super easy to use, and profiling is fun!) But I really don’t understand what I should expect in terms of performance from a Django site and how I should be thinking about at a higher level.

Some things I haven’t figured out yet:

- If I have a site that’s going to be getting occasional bursts of traffic, do I want to be able to scale up?
- Do I want to design the site so that more things can be cached? (and do I really have to? caches are so annoying to get right!)
- The [django performance docs](https://docs.djangoproject.com/en/6.0/topics/performance/) say that Jinja is faster for templating, do I want to think about switching templating systems?
- Those docs also say “{% block %} is faster than using {% include %}”, I wonder if it’s a big difference and if so why

### [template caching might be important](https://jvns.ca/blog/2026/07/21/more-nice-django-things/#template-caching-might-be-important)

I think one thing I’m learning about Django is that because it’s a Framework (tm), it’s easy to accidentally misconfigure it. For example, when I was thinking about why my site was slow just now, I read the [django performance docs](https://docs.djangoproject.com/en/6.0/topics/performance/) and I noticed a comment saying:

> Enabling the cached template loader often improves performance drastically, as it avoids compiling each template every time it needs to be rendered.

When I’d done CPU profiling I’d noticed that it was spending a lot of time rendering templates! Maybe this could help me!

Clicking through the link, I saw that the cached template loader was supposed to be on by default, but I’d turned it off by accident while trying to do something else. I think this “I turned off the cached template loader by default” things is an example of how I still find the django settings file to be pretty confusing and difficult. I guess I should just be careful when I go in there.

After turning on template caching, it seems like the site can now pretty easily handle 12 requests per second or so without using all of the CPU. I have not carefully benchmarked the before and after but it seems like it’s made a pretty big difference.

One thing that’s been surprising to me about Django performance is that I’ve always heard the advice “if you have a performance problem, check your database queries! Maybe add an index!”. But I’ve been running into a variety of performance issues (like this template caching thing) that are not because of slow queries, so instead it’s been more useful for me so far to start by running a CPU profile. And since I’m using SQLite, any slow database query problem will show up on the CPU profile anyway.

Anyway I don’t want to get too far into site performance. Like I said it’s easy for me to get interested in profiling, but actually I know a lot about profiling and it’s not the most important thing for me to learn about.

### [that’s all for now!](https://jvns.ca/blog/2026/07/21/more-nice-django-things/#that-s-all-for-now)

I might say more about what I’m enjoying (or having a hard time with!) about Django later. Trying to write some shorter blog posts recently.
