---
layout: page
title: projects
permalink: /projects/
description: A curated selection of data science, machine learning, and research projects.
nav: true
nav_order: 2
horizontal: false
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
      {% if page.horizontal %}
      <div class="container">
        <div class="row row-cols-1 row-cols-md-2">
        {% for project in section_projects %}
          {% include projects_horizontal.liquid %}
        {% endfor %}
        </div>
      </div>
      {% else %}
      <div class="row row-cols-1 row-cols-md-3">
        {% for project in section_projects %}
          {% include projects.liquid %}
        {% endfor %}
      </div>
      {% endif %}
    </div>
    {% endif %}
  {% endfor %}
{% endfor %}
</div>
