---
layout: default
permalink: /blog/
title: blog
nav: true
nav_order: 2
pagination:
  enabled: true
  collection: posts
  permalink: /page/:num/
  per_page: 10
  sort_field: date
  sort_reverse: true
  trail:
    before: 1
    after: 3
---

<div class="post">

  <div class="writing-header">
    <h1 class="post-title">Writing</h1>
    <p class="writing-subtitle">Decisions and tradeoffs that tutorials never cover. Published on <a href="https://dev.to/ayanarshad02" target="_blank" rel="noopener noreferrer">Dev.to</a>.</p>
  </div>

  <ul class="writing-list">

    {% if page.pagination.enabled %}
      {% assign postlist = paginator.posts %}
    {% else %}
      {% assign postlist = site.posts %}
    {% endif %}

    {% for post in postlist %}
    {% assign tags = post.tags | join: "" %}

    <li class="writing-item">
      <span class="writing-date">{{ post.date | date: '%b %Y' }}</span>
      <div class="writing-content">
        <h3 class="writing-title">
          {% if post.redirect == blank %}
            <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
          {% elsif post.redirect contains '://' %}
            <a href="{{ post.redirect }}" target="_blank" rel="noopener noreferrer">{{ post.title }}<span class="writing-external"> ↗</span></a>
          {% else %}
            <a href="{{ post.redirect | relative_url }}">{{ post.title }}</a>
          {% endif %}
        </h3>
        {% if post.description %}
          <p class="writing-desc">{{ post.description }}</p>
        {% endif %}
        {% if tags != "" %}
          <div class="writing-tags">
            {% for tag in post.tags %}
              <a href="{{ tag | slugify | prepend: '/blog/tag/' | relative_url }}" class="writing-tag">{{ tag }}</a>
            {% endfor %}
          </div>
        {% endif %}
      </div>
    </li>

    {% endfor %}

  </ul>

{% if page.pagination.enabled %}
{% include pagination.liquid %}
{% endif %}

</div>
