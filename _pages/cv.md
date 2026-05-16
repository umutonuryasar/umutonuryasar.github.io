---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

[Download PDF](/files/cv_umut_onur_yasar.pdf){: .btn .btn--primary}

---

## Education

{% for item in site.data.cv.education %}
**{{ item.degree }}**
{{ item.institution }} · {{ item.year }}
{% if item.note %}_{{ item.note }}_{% endif %}

{% endfor %}

---

## Experience

{% for item in site.data.cv.experience %}
**{{ item.role }}** · {{ item.company }}
_{{ item.period }} · {{ item.location }}_

{% for point in item.description %}
- {{ point }}
{% endfor %}

{% endfor %}

---

## Skills

{% for item in site.data.cv.skills %}
**{{ item.category }}:** {{ item.items }}
{% endfor %}

---

## Certifications

{% for item in site.data.cv.certifications %}
- {{ item.name }} — _{{ item.issuer }}_
{% endfor %}