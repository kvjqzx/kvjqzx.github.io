---
layout: page
title: WlanEasy
tagline: Supporting tagline
---
<div id="home">
Here is posts list:
<ul class="posts">
  {% for post in site.posts %}
    &raquo;<span>{{ post.date | date_to_string }}</span><a href="{{ post.url }}">{{ post.title }}</a>
    <br>
  {% endfor %}
</ul>
</div>
