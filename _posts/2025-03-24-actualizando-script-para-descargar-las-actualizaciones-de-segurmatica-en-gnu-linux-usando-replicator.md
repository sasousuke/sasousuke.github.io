---
layout: post
title: "Actualizando nuestro script para descargar las actualizaciones de Segurmática en GNU/Linux usando Replicator"
---

En una [entrada anterior](https://sasousuke.github.io/2023/06/16/script-para-descargar-las-actualizaciones-de-segurmatica-en-gnu-linux-usando-replicator.html) 
expuse el script que usaba para la tarea en cuestión. Luego de varios meses funcional empezaron a suceder cosas que me llevaron a modificarlo. Entre ellas están:
+ Fallas internas de la herramienta al descargar la actualización.
+ Inestabilidad en la conexión mediante proxy.

El script actualizado quedó así:
{% highlight ruby %}
#!/bin/bash
export LC_ALL=C

# Directorio de descarga de la actualizacion
SAVDIRUPDATEBASE=/var/www/antivirus/actualizaciones/segurmatica

# Archivo con la actualizacion manual
ARCHIVOZIP=${SAVDIRUPDATEBASE}/actualizacion.zip

# Se define un tamaño menor a cuanto debe pesar la actualización una vez comprimida para referencia
SIZEFILEMIN=1024 # 1 MB en bytes

# Dirección IP o nombre FQDN del servidor proxy web a usar
PROXYHOST=192.168.10.1
PROXYHOST=proxy.miempresa.cu

# Puerto servidor proxy web a usar
PROXYPORT=3128

# Borrar la actualizacion anterior
rm -rf ${SAVDIRUPDATEBASE}/*

actions() {

    # Ejecutamos la aplicacion Replicator con los parametros de nuestra red
    # Poner la ruta completa para evitar errores de no encontrado la ruta del ejecutable
    /root/replicator_segurmatica/replicator -i segav -d ${SAVDIRUPDATEBASE} -f ${PROXYHOST} -p ${PROXYPORT}
    # /root/replicator_segurmatica/replicator -i segav -d ${SAVDIRUPDATEBASE}

    # Posicionandonos en la ruta donde esta la actualizacion
    cd ${SAVDIRUPDATEBASE}

    # Creando el zip
    zip -r actualizacion.zip *

    # Estableciendo permisos para que cualquier usuario del sistema pueda leer el contenido de la carpeta
    chmod -R 755 ${SAVDIRUPDATEBASE}
}

while true; do
    if [ -f "$ARCHIVOZIP" ]; then
        SIZEFILE=$(stat -c%s "$ARCHIVOZIP")
        if [ "$SIZEFILE" -ge "$SIZEFILEMIN" ]; then
           echo "El archivo ahora pesa 1 MB o mas. Saliendo del ciclo."
           break
        else
           echo "El archivo es menor de 1 MB. Ejecutando tarea..."
           actions 
        fi
    else
        echo "El archivo no existe."
        actions 
    fi
    sleep 10  # Espera 10 segundos antes de verificar nuevamente
done

# Regla de oro. Si un script finaliza de forma satisfactoria emite un mensaje de OK == 0
exit 0
{% endhighlight %}

Ahora las notas para mimismo y no volverme loco:

+ Leer de nuevo lo que puse en el post original. Sí, so penco, estás viejo y se te olvidan las cosas.
+ *actions()* es la forma de crear un procedimiento dentro de un script en [BASH](https://www.wikipedia.org/BASH) con nombre action,
si querías ponerle procedimiento escribes *procedimiento()*.
+ Los procedimientos se llaman sin usar los paréntesis, en este caso *action*.
+ Si eres de los afortunados que no usan proxy web en sus centros laborales porque nadie del nivel central te controla,
usa la línea de abajo que no lleva implicito esos parámetros.
+ Mejorar este script para futuros escenarios. Toda obra humana puede perfeccionarse.
  
*«Ya valió madres, se acabó el espacio»*
