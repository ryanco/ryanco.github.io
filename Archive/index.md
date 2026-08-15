---
layout: page
title: Archive
---

### Posts

<div class="archive__grid">
{%- for node in site.posts %}
  <a class="archive__item" href="{{ node.url | relative_url }}">
    <span class="archive__date">{{ node.date | date: "%-d %b %Y" }}</span>
    <span class="archive__title">{{ node.title }}</span>
  </a>
{%- endfor %}
</div>

### Reading

* [Current reading list]({{ '/Books' | relative_url }})
* [2014 Books]({{ '/Books2014' | relative_url }})
* [2013 Books]({{ '/Books2013' | relative_url }})
