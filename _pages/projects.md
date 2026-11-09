---
title:
layout: default
permalink: /projects/
published: true
---

<div class="ProjectContainer">
  <div class="gallery">

    {% for project in site.projects %}

      {% if project.redirect %}
        {% assign link = project.redirect %}
        {% assign target = '_blank' %}
      {% else %}
        {% assign link = project.url | prepend: site.baseurl | prepend: site.url %}
        {% assign target = '_self' %}
      {% endif %}

      <a class="projectCard" href="{{ link }}" target="{{ target }}"{% if project.redirect %} rel="noopener"{% endif %}>
        <div class="projectCard-media">
          {% if project.image %}
            <img src="{{ project.image | prepend: site.baseurl }}" alt="{{ project.title }}" loading="lazy">
          {% else %}
            <div class="projectCard-placeholder"><span>{{ project.title | slice: 0 }}</span></div>
          {% endif %}
        </div>
        <div class="projectCard-body">
          <h2>{{ project.title }}</h2>
          <p>{{ project.description }}</p>
        </div>
      </a>

    {% endfor %}

  </div>
</div>