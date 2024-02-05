# Práctica 1: Virtual Hosts

El concepto Virtual Host nos permite tener varios sitios web funcionando en el servidor. Estos dos sitos web no tienen porqué pertenecer al mismo dominio. Por ejemplo, `www.misitio.com` y `www.tusitio.com`. 

## Requisitos

Crea los Virtual Hosts que cumplan las siguientes características:

* `www.mipagina.com`: se accede a través del nombre de dominio `www.mipagina.com`. 
* `www.simple.com`: se debe acceder desde la IP `10.33.99.4` la raiz del sitio debe encontrase en `/srv/www/`.
* `www.doble.com`: se debe acceder desde la IP `10.33.99.4` o `172.16.0.4`.
* `www.puerto80.com` y `www.puerto1000.com`: virtual hosts que se acceden desde los puertos `80` y `1000` respectivamente desde la red `10.33.99.4`.

**Consejo:** utiliza la plantilla de situada en `/recursos/plantilla` para crear los Virtual Hosts.

**Consejo**: si no tienes un servidor DNS configurado para resolver los nombres de dominio puedes incluir la dirección en el archivo `hosts`.

**Consejo**: para reducir problemas recomiendo desactivar el sitio por defecto `000-default.conf`.