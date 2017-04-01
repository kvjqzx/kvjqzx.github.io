---
layout: page
title: WlanEasy
tagline: Supporting tagline
---
<div id="home">
<h2 class="fade">Here is posts list:</h2>
<ul class="posts">
  {% for post in site.posts %}
    <li>&raquo; <a href="{{ post.url }}">{{ post.title }}</a><span>{{ post.date | date_to_string }}</span></li>
    <br>
  {% endfor %}
</ul>
</div>
