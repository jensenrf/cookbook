---
layout: default
title: "The Kitchen"
---

<div class="home-header">
  <p class="home-eyebrow">Personal Cookbook</p>
  <h1 class="home-title">The Kitchen</h1>
  <p class="home-subtitle">Recipes collected over time — simple, reliable, good.</p>
</div>

<ul class="recipe-index">
  {% assign recipes = site.recipes | sort: "title" %}
  {% for recipe in recipes %}
  <li class="recipe-index-item">
    <a href="{{ recipe.url | relative_url }}">{{ recipe.title }}</a>
    <div style="display:flex;align-items:center;gap:.8rem;">
      {% if recipe.cook_time %}<span class="recipe-meta-time">{{ recipe.cook_time }}</span>{% endif %}
      {% if recipe.category %}<span class="recipe-tag">{{ recipe.category }}</span>{% endif %}
    </div>
  </li>
  {% endfor %}
</ul>
