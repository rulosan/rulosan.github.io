---
layout: post
title:  "Instalando OpenVAS en Ubuntu 18.04"
date:   2019-01-19 19:36:56 -0600
categories: tutorial
---

# Introducción.

En las pruebas de penetración los escaners de vulnerabilidades nos ayudan a poder comparar las vulnerabilidades conocidas contra los servicios que tienen el cliente en su infraestructura.

Dentro de mi actividad diaria me suelo encontrar que al instalar las herramientas que se necesitan para realizar las pruebas de penetración existen problemas.

Este post va mas dirigido a poder tener algún tipo de manual para poder instalar y configurar __OpenVAS 9__ de manera adecuada.

# Pasos.

Primero se debe de agregar el siguiente repositorio.

```
#add-apt-repository ppa:mrazavi/openvas
```

Inmediatamente se debe de actualizar los repositorios e instalar __sqlite3__ y __openvas9__.

```
#apt-get update -y

#apt-get upgrade -y

#apt-get install sqlite3 openvas9 -y
```

Se pueden instalar los siguientes paquetes en el caso que se requieran extraer los reportes en formato de PDF.

```
#apt-get install texlive-latex-extra --no-install-recommends -y
#apt-get install texlive-fonts-recommended --no-install-recommends -y
```

Si se requiere utilizar __OpenVAS NASL scripts__ se requiere instalar el siguiente paquete:

```
#apt-get install libopenvas9-dev
```

Para poder tener actualizada la base de datos de CVS y vulnerabilidades se tiene que ejecutar los siguientes comandos:

```
# greenbone-nvt-sync
# greenbone-scapdata-sync
# greenbone-certdata-sync
```

Los comandos anteriores pueden tardar un tiempo ya que depende mucho de la velocidad de Internet que se tenga.

Una vez ya descargados todos los componentes se deben de levantar los servicios para poder hacer funcionar todo el sistema de escaneo de vulnerabilidades.

```
# systemctl restart openvas-scanner
# systemctl restart openvas-manager
# systemctl restart openvas-gsa
```

Si todo marcha correcto, los 3 procesos deben de estar corriendo dentro de la máquina.

Se debe de reconstruir la base de datos de NVT's, por lo tanto se debe de ejecutar el siguiente comando.

```
#openvasmd --rebuild --progress --verbose
```

Para poder validar que la instalación se correcta se puede utilizar la herramienta __openvas-check-up__, la cual se tiene que instalar de la siguiente manera.


```
# wget --no-check-certificate https://svn.wald.intevation.org/svn/openvas/branches/tools-attic/openvas-check-setup -P /usr/local/bin/
# chmod +x /usr/local/bin/openvas-check-setup
```

Con esto ya se puede utilizar la herramienta dentro de la maquina de la siguiente manera:

```
# openvas-check-setup --v9
```

Con lo cual debemos de obtener un reporte de lo que nos falta instalar, o lo que esta mal configurado.


# Bonus

En mi caso particular necesito que el protocolo OMP para poder correr mis scripts que automatizan la configuración de la herramienta, lo cual me costo trabajo encontrar en donde habilitarlas.

Leyendo las scripts de inicialización que estan dentro de __/etc/init.d/openvas-scanner__ y __/etc/init.d/openvas-gsa__ se encuentra que en los archivos __/etc/default/openvas-gsa__ y __/etc/default/openvas-manager__ se pueden definir las variables de inicializacion de los procesos.

Para mi caso me funcionan la siguiente configuración:

+ Dentro del archivo __/etc/default/openvas-gsa__ descomentar las siguientes lineas:

```
# To set manager address
#
MANAGER_ADDRESS="0.0.0.0"

# To set manager port number
#
MANAGER_PORT_NUMBER=9390
```

+ Dentro del archivo del __/etc/default/openvas-manager__ descomentar las siguientes lineas:

```
# To set listening port number:
#
PORT_NUMBER=9390
```

Estas configuraciones son para que nos podamos conectar mediante la API al puerto TCP 9390 ya sea con el cliente desde una consola o con librerías con __openvas_lib__ que esta escrita en Python.

# Puntos importantes

+ Hay que tener cuidado donde se despliega el __OpenVAS__ ya que todas estas configuraciones lo pueden hacer vulnerable.

+ Hay que cambiar las credenciales por default.

+ En otro post enumerare los pasos para poderlo fortificar.


# Enlaces de referencia.

[Principal referencia](https://kifarunix.com/how-to-install-and-setup-openvas-9-vulnerability-scanner-on-ubuntu-18-04/)

[openvas_lib de Golismero](https://github.com/golismero/openvas_lib)
