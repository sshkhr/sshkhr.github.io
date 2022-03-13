---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---


For the most up-to-date list of publications please refer to [my Google Scholar profile](https://scholar.google.fr/citations?user=UpV5wyYAAAAJ&hl=en)

\* denotes equal contribution

<table style="width:100%;border:0px;border-spacing:0px;border-collapse:separate;margin-right:auto;margin-left:auto;">

  {% for post in site.publications reversed %}
  <tr>
    <td style="padding:2.5%;width:25%;vertical-align:middle;min-width:120px">
      <img src="/tn{{post.image}}" alt="project image" style="width:auto; height:auto; max-width:100%;" />
    </td>
    <td style="padding:2.5%;width:75%;vertical-align:middle">
      <h3>{{post.title}}</h3>
      <br>
        {{post.authors}}
      <br>
      <em>{{post.venue}}</em>, {{ post.date | date: "%Y" }}
      <br>
        {% if post.paperurl %}
          <a href="{{post.paperurl}}">paper</a> /
        {% endif %}
        {% if post.page %}
          <a href="{{post.page}}">website</a> /
        {% endif %}
        {% if post.video %}
          <a href="{{post.video}}">video</a> /
        {% endif %}
        {% if post.code %}
          <a href="{{post.code}}">code</a> /
        {% endif %}
        {% if post.poster %}
          <a href="{{post.poster}}">poster</a> /
        {% endif %}
        {% if post.slides %}
          <a href="{{post.slides}}">slides</a> /
        {% endif %}
        {% if post.dataset %}
          <a href="{{post.dataset}}">dataset</a> /
        {% endif %}
      <p></p>
      {{ post.excerpt }}
    </td>
  </tr>
  {% endif %}
  {% endfor %}
  {% endfor %}
</table>
