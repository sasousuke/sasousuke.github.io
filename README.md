<nav>
   <a href="/">Inicio</a>
   <a href="#">Acerca de mi</a>
</nav>
<p></p>
<div class="wellcome">
   Bienvenidos a mi Sitio Web creado con <a href="https://pages.github.com" target="_blank">GitHub Pages</a>
</div>
<p></p>
<div class="content">
  <div class="related">
    {% for post in site.posts %}
    <div>
        <span>{{ post.date | date_to_string }}</span> <a href="{{ post.url }}">{{ post.title }}</a>
    </div>
    {% endfor %}
  </div>
</div>
<p></p>
<div class="footer">
   No te preocupes si no encuentras lo que buscas, la mayoría de las veces tampoco lo logro.
</div>
