# Unlisted Posts

<ul style="list-style-type: none; padding: 0px;">
{% for post in site.posts %}
  {% if post.unlisted %}
    <li style='margin-top: 1em;'>
      <div><b><a href="{{ post.url | absolute_url }}">{{ post.title }}</a></b></div>
      <div>{{ post.description }}</div>
    </li>
  {% endif %}
{% endfor %}
</ul>