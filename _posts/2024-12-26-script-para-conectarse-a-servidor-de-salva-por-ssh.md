---
layout: post
title: "Script para conectarse a servidor de salva por ssh usando rsync y sshpass en GNU/Linux"
---

Este sencillo script utiliza rsync para sincronizar las carpetas de salvas de una PC en GNU/Linux para en otra PC también con GNU/Linux. 
Se hace uso del paquete sshpass para cuando no es posible utilizar el login por intercambio de llaves privadas SSH entre origen y destino.


{% highlight ruby %}
!/bin/bash

#Rotar el archivo de registro de trazas de navegacion de squid
#en un directorio destinado para ello

#Credenciales para conectarse por SSH alñ servidor de salvas. Esta la proporciona el administrador.
IPSERVER=X.X.X.X
USUARIO=userssh
PASSWORD=MySecretPass

#Directorio Origen
FOLDERFROM=/ruta/directorio/origen/

#Directorio Destino
FOLDERTOGO=/ruta/directorio/destino/

rsync -rvz -e 'sshpass -p "$PASSWORD" ssh -p 22 -o StrictHostKeyChecking=no' $FOLDERFROM $USUARIO@$IPSERVER:$FOLDERTOGO

#Regla de oro. Si un script finaliza de forma satisfactoria emite un mensaje de OK == 0
exit 0{% endhighlight %}

Ahora las notas para mimismo y no volverme loco:

* Todo lo escrito con MAYÚSCULAS se refiere a variables en BASH.
* Si no funciona la variable PASSWORD es porque la doc de sshpass dice que va con comillas.

Como dicen por ahí: «SE ACABÓ LO QUE SE DABA».
