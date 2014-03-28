---
layout: default
title:  Practice
---

<div id="practice">
  <h3>Practice</h3>
  <section>
    <p>
      These are some practice and for fun problems that I have done. There are several different
      languages and problem types. Most of these have come from old textbooks or just things I thought would be
      interesting to mess around with, contact me a leave me some suggestions for stuff to try out.
      All source for these is available on Github, feel free to fork them!
    </p>
  </section>
  <ul class="posts">
  {% for entry in site.categories.practice %}
    <li>
      <span>{{ entry.date | date_to_string }}</span> &raquo;
      <a href="{{ entry.url }}/">{{ entry.title }}</a></br>
      <em>{{ entry.description }}</em>
    </li>
  {% endfor %}
  </ul>
  Coming Soon!
</div>
