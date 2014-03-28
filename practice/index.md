---
layout: default
title:  Practice
---

<div id="practice">
  <h3>Practice</h3>
  <section>
    <p>
      These are some practice and for fun problems that I have done. I hope to cover each problem in a
      few languages, mainly ones I am interested in at the moment. Most of these have come from old
      textbooks or just things I thought would be interesting to mess around with, contact me a leave
      me some suggestions for stuff to try out.All source for these is available on Github, feel free to fork them!
    </p>
  </section>
  <ul class="posts">
  {% for entry in site.categories.practice %}
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
      <a href="{{ entry.url }}/">{{ entry.title }}</a><br/>
      <em>{{ entry.description }}</em>
    </li>
  {% endfor %}
  </ul>
  Coming Soon!
</div>
