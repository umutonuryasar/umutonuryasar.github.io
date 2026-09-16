---
layout: archive
title: "Work"
permalink: /portfolio/
author_profile: false
classes: portfolio-subpage
---

<p class="subpage-lead">Selected research and engineering projects spanning multimodal learning, computer vision, model efficiency, and deployment.</p>

{% include base_path %}

{% for post in site.portfolio reversed %}
  {% include archive-single.html %}
{% endfor %}
