---
layout: post
category : develop
tagline: "configurar ambiente desarrollo"
tags : [vagrant, phpstorm, xdebug, configuracion]
---

#Configurando PHP 5.5 + Vagrant + Apache2 + MySQL + XDebug + PHPStorm
Nota: Tutorial para Mac OS X 10.8.5

##Pasos previos

1. Instalar VirtualBox [Descargar aquí](https://www.virtualbox.org/)
2. Instalar Vagrant [Descargar](http://www.vagrantup.com/)
3. Generar el script con lo que tu desees que traiga la VM en [PUPHPET](https://puphpet.com/)
4. Ya que completes el wizard de configuración te va descargar un archivo zip con el contenido para armar la VM.
5. Se descomprime el archivo zip.
6. Abres la __Terminal__ y entras al directorio donde se localiza el __Vagrantfile__ (que es el que se descomprimió):
	* Tienes que ejecutar el siguiente comando:```$vagrant up```
	* Va a bajar el box que hayas elegido en el wizard, tardará un rato dependiendo de la velocidad de tu conexión.
	* Si ocurre algun error se mostrará de color rojo, por lo general son problemas de sintaxis en .yaml si le editaste algo.
	* Si todo va chido, saldrá un bonito elefantito en ASCII.
7. Lo que le meti de mi cosecha es en el directorio de ```puphpet/config.yaml``` modifique una configuración del Xdebug para que envie el paquete udp al host que esta haciendo la peticion y __PhpStorm__ pueda funcionar de manera correcta.

Esta es la configuracion del xdebug

     xdebug:
        install: '1'
        settings:
            xdebug.remote_connect_back: '1'
            xdebug.remote_enable: '1'
            xdebug.remote_port: '9000'
            xdebug.remote_handler: 'dbgp'
            xdebug.remote_mode: 'req'
            xdebug.profiler_enable: '0'
            xdebug.profiler_enable_trigger: '1'
            xdebug.remote_autostart: '1'
            xdebug.idekey: 'PHPSTORM'
            xdebug.remote_log: '/var/log/xdebug.log'


De la siguiente manera cuando actives __PhpStorm__ para que escuche peticiones puedas debuggear correctamente