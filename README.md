<nav>
   <a href="/">Home</a>
   <a href="/about/me">About me</a>
</nav>

<div class="wellcome">
   Bienvenidos a mi Sitio Web creado con [GitHub Pages](https://pages.github.com/). Estos son mis contenidos.
</div>

<div class="content">
  <div class="related">
    {% for post in site.posts %}
    <div>
        <span>{{ post.date | date_to_string }}</span> <a href="{{ post.url }}">{{ post.title }}</a>
    </div>
    {% endfor %}
  </div>
</div>

<div class="footer">
   No te preocupes si no encuentras lo que buscas, la mayoría de las veces tampoco lo logro.
</div>
