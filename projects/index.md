---
layout: default
title:  Projects
---

<div id="projects">
  <h3>Projects</h3>
  <section>
    <p>
      Take a look at some of the projects I am working on. Feel free to leave me some feedback!
    </p>
  </section>
  <ul class="posts">
  {% for entry in site.categories.projects %}
    <li>
      {% if entry.github %}
        <a href="{{ entry.github }}" target="_blank">
          <i class="fa fa-github"> </i>
        </a>
      {% endif %}
      {% if entry.bitbucket %}
        <a href="{{ entry.bitbucket }}" target="_blank">
          <i class="fa fa-bitbucket"> </i>
        </a>
      {% endif %}
      <a href="{{ entry.url }}/">{{ entry.title }}</a></br>
      <em>{{ entry.description }}</em>
    </li>
  {% endfor %}
  </ul>
</div>
