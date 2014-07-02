---
layout: default
title: Blog
---

<ul id="posts" class="index">
  {% for post in site.posts %}
    {% if post.status == "publish" %}
    <p>
      <strong><a href="{{ post.url }}">{{ post.title | xml_escape }}</a></strong>
      <span>
      	<em><time datetime="{{ post.date | date: "%Y-%m-%d" }}">
      		{{ post.date | date: "%B %d, %Y" }}
      	</time></em>
      </span>
	  <br />{{ post.content | strip_html | truncatewords: 50 }} <a href="{{ post.url }}">continue reading...</a>
    </p>
	{% endif %}
  {% endfor %}
</ul>

