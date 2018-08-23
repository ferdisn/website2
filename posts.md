---
layout: generic
title: "List of posts"
---

## All posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>

## All tags

<ul class="tags">
{% for tag in site.tags %}
  {% assign t = tag | first %}
  {% assign posts = tag | last %}
  <li><a href="/tags/{{t | downcase | replace:' ','-' }}.html">{{t | downcase | replace:" ","-" }}</a> has {{ posts | size }} posts</li>
{% endfor %}
</ul>


