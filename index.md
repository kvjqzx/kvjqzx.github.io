---
layout: page
title: WlanEasy
tagline: Supporting tagline
---
Here is posts list.
<ul class="posts">
    {% for post in site.posts %}
        <li><span>{{ post.date | date: "%Y-%m-%d" }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
        <br>
    {% endfor %}
</ul>
