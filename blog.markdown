---
layout: default
title: Blog
---

<ul id="posts" class="index">
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title | xml_escape }}</a>
      <span>
      	<time datetime="{{ post.date | date: "%Y-%m-%d" }}">
      		{{ post.date | date: "%B %d, %Y" }}
      	</time>
      </span>
	  <br />{{ post.content | strip_html | truncatewords: 50 }} <a href="{{ post.url }}">continue reading...</a>
    </li>
  {% endfor %}
</ul>
