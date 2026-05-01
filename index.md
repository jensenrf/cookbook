---
layout: default
title: "The Kitchen"
---

<div class="wrap">

  <div class="home-hero">
    <div class="home-hero-text">
      <h1>Simple recipes.<br>Made to be used.</h1>
      <p>A collection of our favorite recipes, organized for real life.</p>
    </div>
    <div class="home-hero-img-placeholder">🌿</div>
  </div>

  <p class="section-label">Recipes</p>

  <div class="recipe-grid">
    {% assign recipes = site.recipes | sort: "title" %}
    {% for recipe in recipes %}
    <a class="recipe-card" href="{{ recipe.url | relative_url }}">
      {% if recipe.image %}
        <img class="recipe-card-img" src="{{ recipe.image | relative_url }}" alt="{{ recipe.title }}">
      {% else %}
        <div class="recipe-card-img-placeholder">🍽</div>
      {% endif %}
      <div class="recipe-card-body">
        <p class="recipe-card-title">{{ recipe.title }}</p>
        {% if recipe.description %}
          <p class="recipe-card-desc">{{ recipe.description }}</p>
        {% endif %}
        <span class="recipe-card-arrow">→</span>
      </div>
    </a>
    {% endfor %}
  </div>

  <div class="home-quote">
    <blockquote>"When someone cooks for you, they are saying something. They are telling you about themselves: where they come from, who they are, what makes them happy."</blockquote>
    <cite>— anthony bourdain</cite>
  </div>

</div>
