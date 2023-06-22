---
layout: post
title: "Script para rotar logs de squid3 en GNU/Linux"
---
![Segurmatica](/assets/img/squid3.png)

A raíz de la necesidad de tener una forma de rotar los logs de squid3, para llevar un control de la navegación y el análisis de las trazas orientadas por la Oficina para la Seguridad de Redes Informáticas (OSRI), me dispongo a implementar este script que muestro a continuación:

{% highlight ruby %}
!/bin/bash
#Rotar el archivo de registro de trazas de navegacion de squid
#en un directorio destinado para ello

#Permisos de ejecucion
#chmod a+rx /usr/local/sbin/rotar-logs.sh

#Agregar a la lista de tareas en crontab
#La tarea se realiza a las 11 y 59 pm de cada dia
#59 23 * * * /usr/local/sbin/rotar-logs.sh

#Ruta del directorio base de logs de squid
LOGFILEDIR=/var/log/squid

#Ruta del log de acceso
LOGFILE=${LOGFILEDIR}/access.log

#Ruta del log de cache
CACHEFILE=${LOGFILEDIR}/cache.log

#Ruta del log de store
STOREFILE=${LOGFILEDIR}/store.log

#Ruta del directorio base para la rotacion
BASEDIR=/var/squid-logs

#Ruta del directorio de almacenamiento de logs
LOGDIR=${BASEDIR}/$(date +%Y)/$(date +%m).$(date +%B)

#Nombre de archivo del log
FILENAME=access-$(date +%d%H%M).log

#Comprobar que existe el directorio de almacenamiento de logs
#Si no existe, crearlo.
if [ ! -d ${LOGDIR} ]; then
mkdir -p ${LOGDIR}
fi

#Detener el servicio de squid
/etc/init.d/squid stop

#Pausa interna de bash de ejecucion
sleep 30s

#No dejar vivo a ningun hilo de demonio suelto por ahi
killall squid

#Esperamos otro poquito para garantizar que ya es el otro dia
sleep 30s

#Repetimos el llamado por si tu kernel es sopenco y no acaba de realizar la tarea
killall squid

#Mover el log hacia el directorio de almacenamiento de logs
cp ${LOGFILE} ${LOGDIR}/${FILENAME}

#Vaciar access.log
cat /dev/null > ${LOGFILE}

#Vaciar store.log
cat /dev/null > ${STOREFILE}

#Vaciar cache.log
cat /dev/null > ${CACHEFILE}

#Iniciamos el servicio de squid
/etc/init.d/squid start

#Regla de oro. Si un script finaliza de forma satisfactoria emite un mensaje de OK == 0
exit 0{% endhighlight %}

Ahora las notas para mimismo y no volverme loco:

* Todo lo escrito con MAYÚSCULAS se refiere a variables en BASH.
* El directorio ruta definido por BASEDIR es donde vamos a crear la salva diaria del access.log
* Se hace una espera ya que a veces el proceso no cierra bien o se quida en el infinito y más allá.
* La tarea se debería programar en un horario (puede ser el mismo que sugiero al inicio) ya tú sabrás tus razones.

Y como diría mi ex: «HASTA AQUÍ LLEGAMOS».
