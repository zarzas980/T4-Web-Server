# Práctica 2: Directivas de mapeo de URL y directiva Indexes.

En esta práctica se pondrá en práctica algunas de las directivas de Apache para el mapeo de URL. CUando hablamos de Mapeo de URL nos referimos a cómo gestiona el servidor el recurso de pedido en una URL. Cómo se mencionó en los apuntes conviene tener a mano el [índice de directivas](https://httpd.apache.org/docs/2.4/mod/directives.html). 

## Requisitos

Para hacer esta práctica se necesita el sito web `www.mipagina.example` creado en la práctica anterior y cuyo `DocumentRoot` tenga los contenidos de la [plantilla](/recursos/plantilla/) que se encuentra en recursos. 

## Desarrollo

El desarrollo de la práctica incluye comprobar el funcionamiento de varias de las directivas de apache mediante pequeños ejercicios:

#### Ejercicio 1: Directiva Indexes

Cambia el nombre del index de la página a `index2.html` y comprueba que ahora al entrar en la página lo que se muestra es un tabla de contenidos con los elementos del `DocumentRoot`. Dentro del fichero de configuración de `apache2.conf` elimina la directiva `Indexes`de la siguiente línea.
```
<Directory /var/www>
    Options Indexes FollowSymLinks
    ...
</Directory>
```
Comprueba que ahora ya no se muestra la tabla de contenidos al entrar en el sitio web.

Añade una directiva `<Directory>`al archivo de configuración de tu sito web para que muestre la tabla de contenidos cuando se accede a recursos en la ruta `/var/www/pagina`.

Nota: volver al nombre original de `index.html`,

#### Ejercicio 2: FollowSymLinks

Crea un archivo de prueba `prueba.html` en tu directory home. Luego crea un enlace simbólico a este fichero desde tu sitio web y comprueba si es posible seguirlo. Elimina la directiva `FollowSymLinks` para que no se posible seguir enlances simbólicos.

#### Ejercicio 3: Alias

Crea el directorio `/srv/images` y el archivo `prueba2.html` dentro de el. Utiliza la directiva `Alias` para mapear la URL `www.mipagina.example/imagenes` al directorio `/srv/images`. 

Recuerda que es necesario añadir una directiva `<Directory>` para que el servidor tenga permiso de acceso al directorio `/srv/images`. 

#### Ejercicio 4: Redirect 302 (Temporal)

Cambia el nombre al fichero `otra_pagina.html` por `modificado.html`. Utiliza la directiva `Redirect` para crear una redirección temporal a `modificado.html`. 

#### Ejercicio 5: Redirect 301 (Permanente)

Crea el sitio web `www.antigua.example`. No crees un DocumentRoot para este sitio y no incluyas la directiva `DocumentRoot` en su fichero de configuración.

Crea una redirección permanente de toda la raíz de `www.antigua.example` a la raíz de `www.mipagina.example`.

#### Ejercicio 6: Rewrite

Utiliza las directivas `RewriteEngine` y `RewriteRule` para mapear el fichero `otra_pagina.html` por el nuevo `modificado.html`. 

Nota: tendrás que comentar la directiva `Redirect` del ejercicio 4. 

