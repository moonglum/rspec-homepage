---
layout: default
title: Welcome
---

# Welcome

* What is RSpec?
* This is page is for beginners, but also a refresher for experienced users
* A few words about the old `should` and the new `expect` syntax
* Some News below

## Blogposts

{% for post in site.posts %}
 * {{ post.date | date_to_string }} &raquo; [{{ post.title }}]({{ post.url }})
{% endfor %}
