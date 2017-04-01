---
layout: page
title: WlanEasy
tagline: Supporting tagline
---
Here is posts list.
<ul class="posts">
    {% for post in site.posts %}
        <li><span>{{ post.date | date_to_string }}</span> <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
        <br>
    {% endfor %}
</ul>
