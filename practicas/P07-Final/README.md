# Práctica 7: Configuración completa de un servidor

En ésta práctica vas a reunir todos los conocimientos de las demás prácticas. El objetivo es configurar un sitio web completo que reúna toda la configuración vista. 

## Requisitos

Se necesitan tres máquinas:

* Servidor Apache
* Cliente
* Autoridad Certificadora

Tu sitio web usará los archivos que están dentro de [plantillaFinal](/recursos/plantillaFinal/) y además se deberá usar también los ficheros dentro de [errors.](/recursos/errors/)

## Desarrollo

Crea el sitio web `www.faisbuc.example` con la siguiente configuración:

* El sitio web tendrá su document root en `/var/www/faisbuc`. 
* Al sitio web solo se debe poder acceder desde equipos de la red interna y conectándose a la interfaz de red de la red interna.
* El directorio `/news` ahora se llama `/noticias`. Crea un redirect permanente al nuevo nombre.
* Guarda la carpeta `errors` en la ruta `/srv/errors`. Debes configurar el servidor para que use los ficheros en su interior en los errores correspondientes.
* A la carpeta `/personal` solo deben tener acceso los usuarios Félix y Clara. (Son usuarios de Apache) 
* Se debe configurar el TLS.
