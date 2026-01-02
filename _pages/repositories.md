---
layout: page
permalink: /repositories/
title: Research Projects
description: A showcase of my research projects in public policy, data analysis, and social sciences. Each project includes code, analysis, and detailed documentation.
nav: true
nav_order: 3
---

## Research Projects

Below are my research projects with complete code implementations. Each repository contains data analysis, methodology, and findings.

{% if site.data.repositories.github_users %}
### GitHub Profile
<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center mb-5">
  {% for user in site.data.repositories.github_users %}
    {% include repository/repo_user.html username=user %}
  {% endfor %}
</div>
{% endif %}

{% if site.repo_trophies.enabled %}
### GitHub Achievements
<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center mb-5">
  {% for user in site.data.repositories.github_users %}
    {% include repository/repo_trophies.html username=user %}
  {% endfor %}
</div>
{% endif %}

{% if site.data.repositories.github_repos %}
### Project Details

{% for repo in site.data.repositories.github_repos %}
  {% include repository/repo_with_description.html repository=repo %}
{% endfor %}
{% endif %}



## Technical Skills

- **Statistical Analysis**: R, Julia, regression modeling, fixed effects, logistic regression
- **Data Management**: NSS, IHDS, and other socio-economic datasets
- **Research Tools**: Git version control, LaTeX documentation, reproducible research workflows
- **Policy Research**: Literature review, archival research, fieldwork, data visualization

---

*Note: All projects are open-source and available on GitHub. Each repository contains complete code, data documentation, and analysis methodology.*