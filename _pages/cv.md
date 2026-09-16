---
layout: archive
title: "CV"
permalink: /cv/
author_profile: false
classes: portfolio-subpage cv-redesign
redirect_from:
  - /resume
---

<p class="subpage-lead">Applied AI research engineering across computer vision, model efficiency, multimodal learning, and open-source software.</p>

[Download PDF](/files/cv_umut_onur_yasar.pdf){: .btn .btn--primary}

## Experience

{% for item in site.data.cv.experience %}
### {{ item.role }} · {{ item.company }}
<span class="cv-meta">{{ item.period }} · {{ item.location }}</span>

{% for point in item.description %}
- {{ point }}
{% endfor %}
{% endfor %}

## Open Source

{% for item in site.data.cv.opensource %}
### [{{ item.name }}]({{ item.link }})
{{ item.description }}
{% endfor %}

## Education

{% for item in site.data.cv.education %}
### {{ item.degree }}
{{ item.institution }} · {{ item.year }}
{% if item.note %}_{{ item.note }}_{% endif %}
{% endfor %}

## Skills

{% for item in site.data.cv.skills %}
**{{ item.category }}** — {{ item.items }}  
{% endfor %}

## Certifications

{% for item in site.data.cv.certifications %}
- {{ item.name }} — _{{ item.issuer }}_
{% endfor %}
