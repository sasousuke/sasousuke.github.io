---
layout: post
title: "Script para borrar archivos y carpetas que ya caducaron por su tiempo en GNU/Linux"
---

Resulta que cuando creas una estructura de directorio donde almacenas las bitacoras (logs) por un espacio de tiempo determinado, pues llega el momento de borrar los más viejitos y aprovechar ese espacio en los nuevos generados. 

{% highlight ruby %}
#!/bin/bash

# Directorio base donde estan almacenados los logs que se irán eliminando cuando su fecha de caducidad llegue
BASEDIR=/home/trazas/logs_rotados 

# Elimino todos los archivos de un directorio y subdirectorios dentro de este que tengan más de 366 dáas de antiguo con respecto a la fecha del sistema.
find ${BASEDIR} -type f -mtime +366 -exec rm {} \;

# Eliminar los directorios que están vacíos
find ${BASEDIR} -type d -empty -delete

# Regla de oro. Si un script finaliza de forma satisfactoria emite un mensaje de OK == 0
exit 0
{% endhighlight %}

Ahora las notas para mimismo y no volverme loco:

* Todo lo escrito con MAYÚSCULAS se refiere a variables en BASH.

Y listo, vaya a continuar con lo suyo. «FINITO FINITO».
