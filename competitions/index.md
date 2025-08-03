---
layout: page
title: Competitions
permalink: /competitions/
---

# Competitions

{% for competition in site.competitions %}
- [{{ competition.title }}]({{ competition.url }}) - Deadline: {{ competition.deadline }}
{% endfor %}
