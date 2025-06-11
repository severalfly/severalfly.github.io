layout: default
---------------



 
<ul>
  {% for post in site._stock %}
    <li>
      {{ post.date | date_to_string }}
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>


