---
layout: archive
title: "Research/Projects"
permalink: /projects/
author_profile: false
entries_layout: list
---

{% for post in site.projects reversed %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
