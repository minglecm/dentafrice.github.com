---
layout: page
title: "Home"
group: "navigation"
---
{% include JB/setup %}

### Articles

<ul class="posts">
{% for post in site.posts limit: 5 %}
  <div class="post_info">
    <li>
            <a href="{{ post.url }}">{{ post.title }}</a>
            <span>({{ post.date | date_to_long_string }})</span>
    </li>
    </br> <em>{{ post.excerpt }} </em>
    </div>
  {% endfor %}
</ul>

### Pages

<ul class="pages">
  {% for page in site.pages %}
  	{% if page.group != 'navigation' and page.group != 'feeds' %}
    <li>
  	  <a href="{{ page.url }}">{{ page.title }}</a>
    </li>
    {% endif %}
  {% endfor %}
</ul>
