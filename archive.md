---
layout: page
title: Archive
---

## Blog Posts

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }}) - <a href="{{ post.url }}#disqus_thread" data-disqus-identifier="{{ post.url }}"></a>
{% endfor %}
