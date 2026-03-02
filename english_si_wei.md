---
layout: default
title: 英语思维
---

<div class="section">
  <h2 class="section-title">英语思维</h2>
  <ul class="post-list">
    {% for post in site.english_si_wei %}
    <li class="post-item">
      <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
      <a href="{{ post.url }}" class="post-link">{{ post.title }}</a>
    </li>
    {% endfor %}
  </ul>
</div>
