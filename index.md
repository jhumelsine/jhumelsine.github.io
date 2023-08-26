<ul style="list-style-type: none; padding: 0px;">
{% for post in site.posts %}
  <li style='margin-top: 1em;'>
  <div><b><a href="{{ post.url | absolute_url }}">{{ post.title }}</a></b></div>
  <div><i>{{ post.date | date: "%A, %B %e, %Y" }}</i></div>
  <div>{{ post.description }}</div>
  </li>
{% endfor %}
</ul>