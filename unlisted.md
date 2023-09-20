---
title: Unlisted Posts and Pages
description: All posts with "unlisted" in their frontmatter
unlisted: true
comments: false
---

# Unlisted Posts and Pages

<ul style="list-style-type: none; padding: 0px;">
{% for post in site.posts %}
  {% if post.unlisted %}
    <li style='margin-top: 1em;'>
      <div><b><a href="{{ post.url | absolute_url }}">{{ post.title }}</a></b></div>
      <div>{{ post.description }}</div>
    </li>
  {% endif %}
{% endfor %}

{% for page in site.pages %}
  {% if page.unlisted %}
    <li style='margin-top: 1em;'>
      <div><b><a href="{{ page.url | absolute_url }}">{{ page.title }}</a></b></div>
      <div>{{ page.description }}</div>
    </li>
  {% endif %}
{% endfor %}
</ul>
