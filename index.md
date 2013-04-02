---
layout: default
title: "Home"
group: "navigation"
tagline: "test"
---
{% include JB/setup %}

{% for post in site.posts limit: 1 %}
  <div class="page-header">
    <h1>{{ post.title }} {% if post.tagline %}<small>{{post.tagline}}</small>{% endif %}</h1>
  </div>

  <div class="row-fluid post-full">
    <div class="span12">
      <div class="date">
        <strong>{{ post.date | date_to_long_string }}</strong>
      </div>
      <div class="content">
        {{ post.content }}
      </div>
    </div>
  </div>
{% endfor %}
