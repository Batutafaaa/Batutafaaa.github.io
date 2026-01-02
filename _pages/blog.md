---
layout: default
title: blog  # This should match your config
pagination:
  enabled: true
  collection: posts
  permalink: /page/:num/
  per_page: 5
  sort_field: date
  sort_reverse: true
  trail:
    before: 1
    after: 3
nav: true  # Make sure this is set to true
nav_order: 2  # Set the order (e.g., 2 for after About, 3 for after Repositories)
---