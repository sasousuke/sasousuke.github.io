---
layout: post
title: "Agregando lista de exclusión al squish para que la navegación .cu no consuma tu cuota de internet"
---

En los pocos escenarios donde aún sobrevive el sistema de cuotas [SQUISH](http://www.ledge.co.za/software/squint/squish/) 
para el [squid](https://www.squid-cache.org/), se da el caso de que por defecto toda la navegación que se procesa en el **access.log** 
se concidera cuota consumida. El **admin de red** (para que lo consideren *"el buena gente"*) quiere que el **SQUISH** no 
le esté computando la navegación a los dominios cubanos **".cu"** a sus usuarios y así tengan unos *meguitas* más para gastar. 
Por suerte **SQUISH** tiene la forma de hacerlo.

Lo primero es localizar en la carpeta de instalación del **SQUISH** el archivo con nombre **squish.pl**, que si se hizo desde el manual 
de instalación (¿alguien se leé eso?) debe estar en la ruta **/usr/local/squish** y lo editamos.

En la **línea 37** se declara la variable **excludelist** que como su nombre lo indica va a contener las exclusiones de nuestro 
contabilizador. El contenido original es este:

{% highlight ruby %}
@excludelist = (
    { "field" => 3, "pattern" => "TCP_DENIED/" } ,
    { "field" => 3, "pattern" => "NONE/" },
    { "field" => 6, "pattern" => '^http://127\.0\.0\.1/' } # localhost
);
{% endhighlight %}

Como ya supones **adminred**, los campos ahí definidos son los mismos que se definen en la generación de las líneas del **access.log** 
tradicional (para mayor referencia leer la documentación de squid en el apartado de *access.log*) y el que nos interesa es el **6**
donde se registra la [URL](https://www.wikipedia.org/URL) del recurso solicitado. La solución es tan sencilla como agregar en ese 
arreglo una expresión regular *(en inglés "pattern"*) para identificar los dominios cubanos. A mí me quedó de esta forma:

{% highlight ruby %}
@excludelist = (
    { "field" => 3, "pattern" => "TCP_DENIED/" } ,
    { "field" => 3, "pattern" => "NONE/" },
    { "field" => 6, "pattern" => '^http://127\.0\.0\.1/' }, # localhost
    { "field" => 6, "pattern" => '^(https?://)([\w.-]+\.)*[\w-]+\.(cu)' } # Navegacion .cu
);
{% endhighlight %}

Ahora las notas para mimismo y no volverme loco:

* Eso es [Perl](https://www.wikipedia.org/Perl), asegúrate de copiar y pegar bien.
* Con un poco de estudio puedes seguir agregando dominios que son propios de plataformas cubanas que alguien las registró en otros
dominios, ejemplo **ENZONA**.

*«Muerto el perro, la pulga se muda»*
