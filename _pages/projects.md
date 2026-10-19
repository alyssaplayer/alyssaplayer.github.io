---
layout: page
title: projects
permalink: /projects/
description: A curated selection of data science, machine learning, and research projects.
nav: true
nav_order: 2
published: true

# Each group renders as a block of sections. A section only appears once it has
# at least one published project whose `category` matches `key`.
project_groups:
  - title: Projects to Highlight
    sections:
      - key: business insights
        title: Business Insights
        tag: Machine Learning Focused
      - key: media and entertainment
        title: Media & Entertainment
        tag: Machine Learning Focused
      - key: tools
        title: Tools
  - title: Other
    divider: true
    sections:
      - key: scientific research
        title: Scientific Research
        tag: Statistics Focused
---

<!-- pages/projects.md -->
<div class="projects">
{% for group in page.project_groups %}
  {% if group.divider %}
  <div class="project-group-divider"><span>{{ group.title }}</span></div>
  {% else %}
  <h1 class="project-group-title">{{ group.title }}</h1>
  {% endif %}

  {% for section in group.sections %}
    {% assign section_projects = site.projects | where: "category", section.key | sort: "importance" %}
    {% if section_projects.size > 0 %}
    {% assign anchor = section.key | slugify %}
    <div class="project-section">
      <a id="{{ anchor }}" href=".#{{ anchor }}" class="project-section-heading">
        <h2 class="category">{{ section.title }}</h2>
        {% if section.tag %}<span class="project-tag">{{ section.tag }}</span>{% endif %}
      </a>
      <div class="project-list">
        {% for project in section_projects %}
        <a class="project-row" href="{% if project.redirect %}{{ project.redirect }}{% else %}{{ project.url | relative_url }}{% endif %}">
          <div class="project-thumb">
            {% if project.img %}
              {% include figure.liquid loading="lazy" path=project.img sizes="200px" alt=project.title class="project-thumb-img" %}
            {% else %}
              <div class="project-thumb-placeholder"><i class="fa-solid fa-chart-line"></i></div>
            {% endif %}
          </div>
          <div class="project-info">
            <h3 class="project-title">{{ project.title }}</h3>
            <p class="project-summary">{{ project.summary | default: project.description }}</p>
            <span class="project-more">View project <i class="fa-solid fa-arrow-right"></i></span>
          </div>
        </a>
        {% endfor %}
      </div>
    </div>
    {% endif %}
  {% endfor %}
{% endfor %}
</div>
