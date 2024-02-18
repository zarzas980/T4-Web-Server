# Práctica 5: ficheros htaccess

Los ficheros `.htaccess` permiten configurar el servidor cuando no se tiene acceso al fichero de configuración general. Esto ocurre mucho en servicios de hosting donde un mismo servidor está sirviendo muchos sitios web y por tanto a cada usuario administrador se le da acceso solo a un document root.

## Requisitos

Para realizar esta práctica es necesario crear un sitio web nuevo llamado `www.tuiter.example`. El sitio tendrá la configuración básica que biene por defecto en apache y tendrá como contenidos los ficheros de la [plantillaHtaccess](/recursos/plantillaHtaccess/).

## Desarrollo

Utiliza ficheros `.htaccess` para aplicar las siguientes configuraciones:

* El directorio `datos` debe tener habilitado la tabla de contenidos cuando no se tiene fichero `index.html`.
* El acceso al directorio `restringido` debe estar limitado a equipos desde la red internal.
