---
layout: post
title:  "Simular Botnet con XMPP"
date:   2014-02-25 16:26:47
categories: prueba botnet xmpp
---

#Botnet con XMPP y Raspberry Pi

##Objetivo

El siguiente tutorial es en sentido educativo y de experimentación de manera controlada de Botnets, es decir se usa en un ambiente para fines educativos y experimentales y no hacer un daño o robo de información

##¿Qué es un botnet?

Básicamente son un conjunto de maquinas que contienen un software (malware por lo general) que se conectan a un C&C (Command and Control) controlado por alguien para dirigir ciertas actividades (por lo general criminales).

En nuestro caso es solo por ocio y conocer como funcionan.

Hay muchos que hablan por IRC, en este caso para variarle un poco yo lo quiero hacer con XMPP.

##¿Qué es XMPP?

Es un protocolo para mensajeria instantanea que es totalmente abierto, y muchas aplicaciones y servicios lo utilizan, en el caso de este experimento mi C&C puede ser un cliente Adium, ChatSecure, etc.

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

## Desarrollo de los bots

* Que corran con python 2.7.
* Que se autoactualicen con git
* Que haya tipos diferentes de bots.
* Que entienda comandos normales de bash, y que haya tags de comandos.




## Links de apoyo

* [setup xmpp server](http://arstechnica.com/information-technology/2014/03/how-to-set-up-your-own-private-instant-messaging-server/)
* [raspberry botnet](http://oskarhane.com/make-your-raspberry-pis-and-other-servers-a-botnet-controlled-via-xmpp/)
* [candy chat](http://candy-chat.github.io/candy/)
* [Configuracion dnsmasq](http://wiki.xmpp.org/web/2010_XMPP_Interop_DNSmasq_config)
* [Configuracion prosody](https://wiki.koumbit.net/ProsodyConfiguration#DNS)
* [Ejemplo de configuracion DNS y Prosody](http://buddycloud.com/install)
* [Modulos de prosody](https://code.google.com/p/prosody-modules/w/list)
* [Ejemplo de echo bot con python](http://sleekxmpp.com/getting_started/echobot.html)
* [Ejemplos de sleekXMPP *IMPORANTE*](https://github.com/fritzy/SleekXMPP/tree/develop/examples)
* [Lista de XEP del protocolo XMPP](http://xmpp.org/xmpp-protocols/xmpp-extensions/)
* [Observatorios de IM servers](https://xmpp.net/index.php)
* [Reporte primera vez](https://xmpp.net/result.php?id=45067)
* [liberia para python](http://xmpppy.sourceforge.net/)
