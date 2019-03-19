---
layout: page
title: Spring
permalink: /spring/
---

<ul class="post-list">
  {% for post in site.spring %}
    <li>
      <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>

      <h2>
        <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
      </h2>
    </li>
  {% endfor %}
<<<<<<< HEAD
</ul>
=======
</ul>
>>>>>>> 7ebe9cb7649432b87fbee59e6f2445c8ced8ef48
