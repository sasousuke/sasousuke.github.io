Bienvenidos a mi Sitio Web creado con GitHub Pages. Estos son mis contenidos.

<div class="content">
  <div class="related">
    {% for post in site.posts %}
    <div>
        <span>{{ post.date | date_to_string }}</span> <a href="{{ post.url }}">{{ post.title }}</a>
    </div>
    {% endfor %}
  </div>
</div>
