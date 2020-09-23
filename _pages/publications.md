---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---


You can find my papers at <u><a href="https://scholar.google.fr/citations?user=UpV5wyYAAAAJ&hl=en">my Google Scholar profile</a>.</u>

{% include base_path %}

\* denotes equal contribution

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
