---
layout: default
title: Jason Truluck
---

<div id="home">
  <h3>Latest</h3>
  <ul class="posts">
    {% for post in site.posts %}
      <li>
        <span>{{ post.date | date_to_string }}</span> &raquo;
        <a href="{{ post.url }}/">{{ post.title }}</a>
        posted in
        {% for category in post.categories %}
          <a href="{{ category }}/">{{ category }}</a>
        {% endfor %}
      </li>
    {% endfor %}
  </ul>
</div>
