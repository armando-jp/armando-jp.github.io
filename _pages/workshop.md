---
layout: page-custom
title: "Workshop"
subtitle: "Engineering projects and hardware builds."
permalink: /workshop/
---

<div class="workshop-grid">
{% assign projects = site.projects | sort: "order" %}
{% for project in projects %}
  <a href="{{ project.url | relative_url }}" class="project-card animate-in">
    <img src="{{ project.cover_image | relative_url }}" alt="{{ project.title }}" class="card-image">
    <div class="card-title-bar">
      <h3 class="card-title">{{ project.title }}</h3>
    </div>
    <div class="card-overlay">
      <h3 class="card-title">{{ project.title }}</h3>
      <p class="card-excerpt">{{ project.excerpt_text | default: project.excerpt | strip_html | truncatewords: 25 }}</p>
      <div class="card-meta">
        <div class="card-tags">
          {% for tag in project.tags limit:3 %}
            <span class="tag">{{ tag }}</span>
          {% endfor %}
        </div>
      </div>
    </div>
  </a>
{% endfor %}
</div>

{% if site.projects.size == 0 %}
<div class="empty-state">
  No projects yet. Check back soon.
</div>
{% endif %}
