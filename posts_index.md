---
layout: default
title: 技术博客
---

<div class="section">
  <h2 class="section-title">技术博客</h2>
  <ul class="post-list">
    {% for post in site.posts reversed %}
    <li class="post-item">
      <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
      <a href="{{ site.baseurl }}{{ post.url }}" class="post-link">{{ post.title }}</a>
      {% if post.excerpt %}
      <p style="color: var(--text-secondary); font-size: 0.9rem; margin-top: 0.5rem;">{{ post.excerpt | strip_html | truncatewords: 20 }}</p>
      {% endif %}
    </li>
    {% endfor %}
  </ul>

  {% if site.posts.size == 0 %}
  <p style="color: var(--text-secondary); text-align: center; padding: 2rem;">暂无文章</p>
  {% endif %}
</div>
