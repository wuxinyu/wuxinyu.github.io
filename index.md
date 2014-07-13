---
layout: index
title: 简单记录!
tagline: 
---
{% include JB/setup %}

<ul class="media-list">
{% for post in site.posts limit:3 %}

  <li class="media">
    <a class="pull-left" href="{{post.url}}">
      {{post.index}}
    </a>
    <div class="media-body">
      <h4 class="media-heading">{{post.title}}</h4>
      <p>{{ post.content }}</p>
      <p>post at {{ post.date | date_to_string }}</p>
    </div>
  </li>
 {% endfor %}
 
</ul>
