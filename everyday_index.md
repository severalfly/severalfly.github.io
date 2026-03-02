---
layout: default
title: 日更挑战
---

<div class="section">
  <h2 class="section-title">日更挑战</h2>
  <ul class="post-list">
    {% for post in site.everyday %}
    <li class="post-item">
      <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
      <a href="{{ post.url }}" class="post-link">{{ post.title }}</a>
    </li>
    {% endfor %}
  </ul>
</div>
