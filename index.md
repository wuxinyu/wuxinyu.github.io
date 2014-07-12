---
layout: index
title: 简单记录!
tagline: 
---
{% include JB/setup %}

{% for post in site.posts limit:3 %}
<div class="page-header">
  <h1>{{ post.title }} {% if post.tagline %} <small>{{ post.tagline }}</small>{% endif %}
  <span class="pull-right">{{ post.date | date_to_string }}</span></h1>
</div>

<div class="row">
  <div class="col-xs-12">
    {{ post.content }}
  </div>
</div>
 {% endfor %}
