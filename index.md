---
layout: default
---

<!-- Featured Links -->
<div class="links-section">
  <h2 class="section-title">精选内容</h2>
  <div class="links-grid">
    <a href="JavaThreadCore" class="link-card">
      <div class="link-card-icon">☕</div>
      <div class="link-card-content">
        <div class="link-card-title">Java 多线程编程核心</div>
        <div class="link-card-desc">深入理解 Java 多线程编程</div>
      </div>
    </a>

    <a href="tensorflow/tfIndex" class="link-card">
      <div class="link-card-icon">🤖</div>
      <div class="link-card-content">
        <div class="link-card-title">TensorFlow 入门</div>
        <div class="link-card-desc">机器学习框架学习笔记</div>
      </div>
    </a>

    <a href="./other/qtjs/qtjsindex" class="link-card">
      <div class="link-card-icon">💪</div>
      <div class="link-card-content">
        <div class="link-card-title">囚徒健身</div>
        <div class="link-card-desc">健身训练指南</div>
      </div>
    </a>

    <a href="elk/elkindex" class="link-card">
      <div class="link-card-icon">📊</div>
      <div class="link-card-content">
        <div class="link-card-title">ELK 技术栈</div>
        <div class="link-card-desc">Elasticsearch, Logstash, Kibana</div>
      </div>
    </a>

    <a href="stock_index" class="link-card">
      <div class="link-card-icon">📈</div>
      <div class="link-card-content">
        <div class="link-card-title">股票分析</div>
        <div class="link-card-desc">投资与理财学习</div>
      </div>
    </a>

    <a href="everyday_index" class="link-card">
      <div class="link-card-icon">📅</div>
      <div class="link-card-content">
        <div class="link-card-title">日更挑战</div>
        <div class="link-card-desc">每日学习记录</div>
      </div>
    </a>

    <a href="english_si_wei" class="link-card">
      <div class="link-card-icon">🗣️</div>
      <div class="link-card-content">
        <div class="link-card-title">英语思维</div>
        <div class="link-card-desc">英语学习方法分享</div>
      </div>
    </a>

    <a href="interview/" class="link-card">
      <div class="link-card-icon">💼</div>
      <div class="link-card-content">
        <div class="link-card-title">面试总结</div>
        <div class="link-card-desc">技术面试经验分享</div>
      </div>
    </a>
  </div>
</div>

<!-- Recent Posts -->
<div class="section">
  <h2 class="section-title">最新文章</h2>

  <!-- My Posts -->
  <h3 style="margin-top: 2rem; font-size: 1.25rem; color: var(--primary);">个人博客</h3>
  <ul class="post-list">
    {% for post in site.my_posts limit: 5 %}
    <li class="post-item">
      <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
      <a href="{{ post.url }}" class="post-link">{{ post.title }}</a>
    </li>
    {% endfor %}
  </ul>

  <!-- All Posts -->
  <h3 style="margin-top: 2.5rem; font-size: 1.25rem; color: var(--primary);">技术博客</h3>
  <ul class="post-list">
    {% for post in site.posts limit: 10 %}
    <li class="post-item">
      <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
      <a href="{{ site.baseurl }}{{ post.url }}" class="post-link">{{ post.title }}</a>
    </li>
    {% endfor %}
  </ul>
</div>

<div style="text-align: center; margin-top: 3rem;">
  <a href="{{ site.baseurl }}/posts_index" style="display: inline-block; padding: 0.75rem 2rem; background: linear-gradient(135deg, var(--primary), var(--accent)); color: white; border-radius: var(--radius); text-decoration: none; font-weight: 500; transition: transform 0.2s;">
    查看更多文章 →
  </a>
</div>
