---
layout: default
permalink: /repositories/
title: repositories
nav: true
nav_order: 4
---

<div class="post">

  <div class="writing-header">
    <h1 class="post-title">Repositories</h1>
    <p class="writing-subtitle">Open source work on <a href="https://github.com/AyanArshad02" target="_blank" rel="noopener noreferrer">GitHub ↗</a></p>
  </div>

  <div class="repos-grid">
    {% for item in site.data.repositories.github_repos %}
    <div class="repo-card">
      <div class="repo-card-header">
        <a href="https://github.com/{{ item.repo }}" target="_blank" rel="noopener noreferrer">
          <svg class="repo-card-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16" aria-hidden="true"><path d="M2 2.5A2.5 2.5 0 0 1 4.5 0h8.75a.75.75 0 0 1 .75.75v12.5a.75.75 0 0 1-.75.75h-2.5a.75.75 0 0 1 0-1.5h1.75v-2h-8a1 1 0 0 0-.714 1.7.75.75 0 1 1-1.072 1.05A2.495 2.495 0 0 1 2 11.5Zm10.5-1h-8a1 1 0 0 0-1 1v6.708A2.486 2.486 0 0 1 4.5 9h8Z"/></svg>
          <span class="repo-card-name">{{ item.name }}</span>
          <span class="repo-card-arrow">↗</span>
        </a>
      </div>
      <p class="repo-card-desc">{{ item.description }}</p>
      <div class="repo-card-footer">
        <span class="repo-card-lang">
          <span class="repo-card-lang-dot"></span>{{ item.language }}
        </span>
        {% for topic in item.topics %}
          <span class="writing-tag">{{ topic }}</span>
        {% endfor %}
      </div>
    </div>
    {% endfor %}
  </div>

</div>
