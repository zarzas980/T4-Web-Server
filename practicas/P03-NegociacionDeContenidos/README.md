# Práctica 3: Negociación de contenidos

La negociación de contenidos consiste en servir diferentes recursos en función de las características que del cliente. Un ejemplo clásico es servir páginas en un idioma en función del idioma por defecto del cliente.

## Requisitos

Para la realizar esta práticas se usuará nuevamente el sitio `www.mipagina.example`.

## Desarrollo 

#### Mensajes de error

Vamos a crear mensajes de error personalizados para nuestro sitio web. En el document root crea el fichero `error/404.html` con un mensaje de error apropiado.

Habilita el mensaje de error personalizado usando la directiva `ErrorDocument`.

* Comprueba el funcionamiento buscando un recurso que no exista dentro del sitio web. Como por ejemplo: `www.mipagina.example/inventado`.

#### Negociación de contenidos

Renombra el fichero `index.html` a `index.html.es`, este será el índice en español. Realiza una copia de este fichero llamada `index.html.en`, éste será el índice en inglés. Realiza cambios en el índice en inglés para que contenga un mensaje de bienvenida en inglés. Habilita la negociación de contenidos usando la directiva `Options` y su parámetro `Multiviews`.

* Comprueba el correcto funcionamiento entrando en el sitio web con el navegador configurado en español y luego configurado en inglés.

Otra forma de habilitar la negociación de contenidos es usando la directiva `AddHandler` junto a ficheros de extensión `var`. Comprueba su funcionamiento habilitando los mensajes de error localizados. 

Para ello habilita el fichero de configuración `conf-available/localized-error-pages.conf`. Dentro del fichero de configuración descomenta todo el contenido en las líneas
```
<IfModule mod_negotiation.c>
    <IfModulo mod_include.c>
        ...
    </IfModule>
</Ifmodule>
```
* Comprueba el funcionamiento de las páginas de error buscando un recurso que no existe mientras tienes el navegador en español y en inglés.