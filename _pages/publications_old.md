---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}


<table width="100%" align="center" border="0px" cellspacing="0" cellpadding="20">
<!-- Paper --> 
<tbody>
<td width="25%" border="0px">
<!-- Image -->
<img src='/images/p1-mask-rcnn.png'  class="" width="100%" class="img1">
</td>
<!-- Paper Info -->
<td valign="top" width="70%" border="0px">
<p><papertitle>
<b>Road Damage Detection And Classification In Smartphone Captured Images Using Mask R-CNN</b>
</papertitle>
<br>
Janpreet Singh*, <u>Shashank Shekhar*</u>
<br>
<em>IEEE BigData Cup Workshop 2018</em>
<br>

<!-- Additional Links -->
[<a href="https://arxiv.org/abs/1811.04535">paper</a>]
</p>

<p> </p>
</td> </tbody>
</table>
<br>

