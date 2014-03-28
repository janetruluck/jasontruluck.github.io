---
layout: default
title: Jason Truluck
---

<div id="blog">
  <h3>Blog Posts</h3>
  <section>
    <p>
      Here you can get a peak at just about anything I care to write about (most often something
      relating to programming).
    </p>
  </section>
  <ul class="posts">
  {% for entry in site.categories.blog  %}
    <li>
      <span>{{ entry.date | date_to_string }}</span> &raquo;
      <a href="{{ entry.url }}/">{{ entry.title }}</a></br>
      <em>{{ entry.description }}</em>
    </li>
  {% endfor %}
  </ul>
</div>
