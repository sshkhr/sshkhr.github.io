---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---


For the most up-to-date list of publications please refer to [my Google Scholar profile](https://scholar.google.fr/citations?user=UpV5wyYAAAAJ&hl=en)

\* denotes equal contribution

<table style="width:100%;max-width:800px;border:none;border-spacing:0px;border-collapse:collapse;margin-right:auto;margin-left:0px;cellpadding:0px;cellspacing:0px;border-bottom-width:0px;border-right-width:0px;">
  <tr style="padding:0px">
    <td style="padding:0px;border-bottom-width:0px;border-right-width:0px;">
      <table style="width:100%;border:none;border-spacing:0px;border-collapse:collapse;margin-right: auto;0px;">

        
        {% for post in site.publications reversed %}
        <tr>
          <td style="padding:2.5%;width:25%;vertical-align:middle;min-width:100px">
            <img src="/{{post.image}}" alt="project image" style="width:auto; height:auto; max-width:100%;" />
          </td>
          <td style="padding:2.5%;width:75%;vertical-align:middle">
            <h3>{{post.title}}</h3>
            <br>
            {{post.authors}}

            <br>
            <em>{{post.venue}}</em>, {{ post.date | date: "%Y" }}
            <br>
            {% if post.paperurl %}
            [<a href="{{post.paperurl}}">Paper</a>]
            {% endif %}
            {% if post.page %}
            [<a href="{{post.page}}">Project Page</a>]
            {% endif %}
            {% if post.video %}
            [<a href="{{post.video}}">Video</a>]
            {% endif %}
            {% if post.code %}
            [<a href="{{post.code}}">Code</a>]
            {% endif %}
            {% if post.poster %}
            [<a href="{{post.poster}}">Poster</a>]
            {% endif %}
            {% if post.slides %}
            [<a href="{{post.slides}}">Slides</a>]
            {% endif %}
            {% if post.dataset %}
            [<a href="{{post.dataset}}">Dataset</a>]
            {% endif %}
            <p></p>
          </td>
        </tr>
        {% endfor %}
      </table>
    </td>
  </tr>
</table>
