---
layout: page-custom
title: "Writing"
subtitle: "Technical deep dives and personal notes."
permalink: /writing/
---

<div class="writing-list">
{% for post in site.posts %}
  <a href="{{ post.url | relative_url }}" class="writing-item">
    <span class="writing-date">{{ post.date | date: "%B %d, %Y" }}</span>
    <h3 class="writing-title">{{ post.title }}</h3>
    <p class="writing-excerpt">{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
  </a>
{% endfor %}
</div>

{% if site.posts.size == 0 %}
<div class="empty-state">
  Nothing here yet. Check back soon.
</div>
{% endif %}
