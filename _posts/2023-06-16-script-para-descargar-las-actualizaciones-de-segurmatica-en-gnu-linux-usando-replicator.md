---
layout: post
title: "Script para descargar las actualizaciones de Segurmatica en GNU/Linux usando Replicator"
---
![Segurmatica](/assets/img/segurmatica.png)

En esta entrega voy a publicar mi script personalizado de descarga de actualizaciones para el Segurmatica Antivirus para Windows usando la herramienta Replicator en GNU/Linux y luego ser publicada en un webftp de la LAN para que las PC se actualicen y/o cualquiera pueda descargarla y llevársela a casa.

Lo primero es el script:
{% highlight ruby %}
#!/bin/bash

export LC_ALL=C

# Directorio de descarga de la actualización
SAVDIRUPDATEBASE=/opt/webftp/Segurmatica/Update

# Borrar la actualización anterior
rm -rf ${SAVDIRUPDATEBASE}

# Comprobar que existe el directorio. Si no existe, crearlo.
if [ ! -d ${SAVDIRUPDATE} ]; then
	mkdir -p ${SAVDIRUPDATE}
fi

#Ejecutamos la aplicación Replicator con los parámetros de nuestra red
#Poner la ruta completa para evitar errores de no encontrado la ruta del ejecutable
/usr/sbin/replicator -i segav -d ${SAVDIRUPDATEBASE} -f 192.168.10.1 -p 3128

#Estableciendo permisos para que cualquier usuario del sistema pueda leer el contenido de la carpeta
chmod -R 755 ${SAVDIRUPDATEBASE}

# Posicionandonos en la ruta donde esta la actualización
cd ${SAVDIRUPDATEBASE}

# Creando el zip
zip -r updateSAV.zip *

# Regla de oro. Si un script finaliza de forma satisfactoria emite un mensaje de OK == 0
exit 0
{% endhighlight %}

Ahora las notas para mimismo y no volverme loco:

* Todo lo escrito con MAYÚSCULAS se refiere a variables en BASH.
* El directorio root del webftp está en la ruta definida por SAVDIRUPDATE.
* El ejecutable de Replicator debe copiarse en una ruta de binarios ejecutables. Hay varias y algunas pueden cambiar según tu distro, yo escojí esa. Recuerda ponerle permisos de ejecución a ese binario, so pichón de durako.
* La última parte de comprimir la actualización es para comodidad en sistemas donde los rar u otro compresor pueda dar problemas.
* Los parametros -f se refieren al IP del proxy web y -p al puerto de ese proxy web. Si te hiciera falta poner usuario y contraseña, léete el manual. RTFM.

Y por último y no menos importante: «EL FIN».
