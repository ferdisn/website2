---
layout: tag
title: Posts tagged wordpress
---
{% for tag in site.tags %}

{% assign t = tag | first %}
{% assign posts = tag | last %}

{% if t == "wordpress" %}
<ul>
{% for post in posts %}
    {% if post.tags contains t %}
    <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
        <span class="date">{{ post.date | date: "%B %-d, %Y"  }}</span>
    </li>
    {% endif %}
{% endfor %}
</ul>
{% endif %}

{% endfor %}