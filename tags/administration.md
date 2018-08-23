---
layout: tag
title: Posts tagged administration
---
{% for tag in site.tags %}

{% assign t = tag | first %}
{% assign posts = tag | last %}

{% if t == "administration" %}

<ul>
{% for post in posts %}
    {% if post.tags contains t %}
    <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
        <span class="date">{{ post.date | date: "%B %-d, %Y"  }}</span>
        {{ post.excerpt }}
    </li>
    {% endif %}
{% endfor %}
</ul>
{% endif %}

{% endfor %}