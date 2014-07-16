#Botnet con XMPP y Raspberry Pi

##Objetivo

El siguiente tutorial es en sentido educativo y de experimentación de manera controlada de Botnets.

##¿Qué es un botnet?


##¿Qué es XMPP?


##Requerimientos


##Configuración del Servidor XMPP

###Instalacion

Nos conectamos por ssh a nuestra Raspberry y ejecutamos el siguiente comando:

```
$sudo apt-get update
$sudo apt-get install prosody
```

Generamos los archivos para poder cifrar las comunicaciones del chat.

__NOTA__: tarda unos minutos en generara los archivos.

```
$ sudo openssl dhparam -out /etc/prosody/certs/dh-2048.pem 2048

```

Configuramos y activamos los módulos que se necesitan dentro de *Prosody* ``/etc/prosody/prosody.cfg.lua``

__Poner los snipplets que cambie del archivo de configuración__


Luego checamos que el archivo de configuración sea correcto con:

```prosodyctl restart ```

Si todo va chido hay que agregar usuarios.

```prosodyctl adduser minuevousuario@midominio.loc```

En el directorio ```/var/lib/prosody/```se iran guardando los datos de los usuarios que vayamos creando.

Checar puertos 5222 (conexion de clientes) 5269 (conexion de otros servidores)

Checar los registros SRV para no tener problemas de conexion tanto de fuera del internet como por dentro.

Checamos el log ``tail -f /var/log/prosody/prosody.log``

Y verificamos con adium que todo este corriendo correctamente, si existiera un erro tendria que salir dentro del log.


## Links de apoyo

* [setup xmpp server](http://arstechnica.com/information-technology/2014/03/how-to-set-up-your-own-private-instant-messaging-server/)
* [raspberry botnet](http://oskarhane.com/make-your-raspberry-pis-and-other-servers-a-botnet-controlled-via-xmpp/)
* [candy chat](http://candy-chat.github.io/candy/)
* [Configuracion dnsmasq](http://wiki.xmpp.org/web/2010_XMPP_Interop_DNSmasq_config)
<<<<<<< Updated upstream
* [Configuracion prosody](https://wiki.koumbit.net/ProsodyConfiguration#DNS)
* [Ejemplo de configuracion DNS y Prosody](http://buddycloud.com/install)
* [Modulos de prosody](https://code.google.com/p/prosody-modules/w/list)
* [Ejemplo de echo bot con python](http://sleekxmpp.com/getting_started/echobot.html)
* [Ejemplos de sleekXMPP *IMPORANTE*](https://github.com/fritzy/SleekXMPP/tree/develop/examples)
* [Lista de XEP del protocolo XMPP](http://xmpp.org/xmpp-protocols/xmpp-extensions/)
* [Observatorios de IM servers](https://xmpp.net/index.php)
* [Reporte primera vez](https://xmpp.net/result.php?id=45067)
=======
* [liberia para python](http://xmpppy.sourceforge.net/)
>>>>>>> Stashed changes
