---
layout: page
permalink: /repositories/
title: Projects & Code
description: Projects where I explore topics of interest.
nav: true
nav_order: 3
---

## My Technical Projects

Below are my GitHub repositories showcasing hands-on work in areas that interest me:
- **Agricultural Market Discrimination**: Data analysis, automation, and algorithmic solutions
- **Social Capital & Employment in Rural India**: Computational optimization and performance evaluation


{% if site.data.repositories.github_users %}
<!-- GitHub Profile Stats -->
<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for user in site.data.repositories.github_users %}
    {% include repository/repo_user.html username=user %}
  {% endfor %}
</div>

<!-- GitHub Achievements -->
{% if site.repo_trophies.enabled %}
<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for user in site.data.repositories.github_users %}
    {% include repository/repo_trophies.html username=user %}
  {% endfor %}
</div>
{% endif %}
{% endif %}

{% if site.data.repositories.github_repos %}
## Featured Code Repositories

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for repo in site.data.repositories.github_repos %}
    {% include repository/repo.html repository=repo %}
  {% endfor %}
</div>
{% endif %}
