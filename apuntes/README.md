# Tema 4: Gestión de servidores Web. Apache.

## ¿Qué es un servidor web?

Los servidores red son el tipo de servidores más usados por los usuarios en internet\.

La estructura de los servicios web sigue la forma cliente\-servidor:
  * <span style="color:#FFFF00">**Servidor**: </span> contiene las páginas web y todos los recursos necesarios para mantenerlas\.
  * <span style="color:#FFFF00">**Cliente**: </span> normalmente es un navegador web\. El usuario utiliza el navegador para realizar peticiones al servidor y luego interpreta las respuestas\.

Para entender cómo funciona un servidor web es importante entender cómo funciona la  <span style="color:#FFFF00"> World Wide Web</span> \.

## Estructura de la World Wide Web

Hay que recordar dos conceptos importantes relacionados con la web\.

* <span style="color:#FFFF00"> **Página web**:  </span> archivo escrito en un lenguaje de marcas \(normalmente HTML\) y que contiene texto\, hipervínculos a otros recursos o páginas web\, imágenes\, scripts\, etc\.

* <span style="color:#FFFF00"> **Sitio web:** </span> conjunto de páginas web y archivos complementarios que se distribuyen bajo un mismo nombre DNS\. Por ejemplo: `www\.educacionyfp\.gob\.es`

La  <span style="color:#FFFF00">World</span>  <span style="color:#FFFF00"> Wide Web </span> es un sistema global de páginas web enlazadas entre sí mediante hipervínculos y almacenadas en servidores web\.

Para navegar la web un usuario usa un software  <span style="color:#FFFF00">**cliente \(navegador\)** </span> para acceder a un recurso web almacenado en un servidor web\.

El usuario puede hacer click en un hipervínculo dentro de la página web y el servidor le redirigirá a otro recurso dentro del propio servidor o de otro distinto\.

## URL

A la hora de acceder a recursos dentro de un servidor web se suele acceder mediante una  <span style="color:#FFFF00"> **URL\.** </span>

La estructura de una URL \(Uniform Resources Locator\) es:

<span style="color:#FFFF00">`Protocolo://usuario:contraseña@máquina:puerto/ruta\_recurso`</span>

Por ejemplo:

`http://www\.mipagina\.com:10000/index\.html`

Veamos una explicación de cada uno de ellos

* <span style="color:#FFFF00"> **Protocolo**: </span> es el protocolo con el que se accede al recurso\. Por ejemplo: http\, https\, ftp…

* <span style="color:#FFFF00"> **Usuario y contraseña** </span> : \(opcional\) se puede acceder al servidor mediante un usuario y contraseña indicados en la URL\. Desaconsejable puesto que las URLs son almacenadas en historiales\, registros y son fácilmente visibles\.

* <span style="color:#FFFF00"> **Máquina** </span> : máquina a la que se accede\. Puede estar en formato IP o por su FQDN\.

* <span style="color:#FFFF00"> **Puerto**:  </span> \(opcional\)\. Si no se especifica se asume 80 para HTTP\, 443 para HTTPS y 21 para FTP

* <span style="color:#FFFF00"> **Ruta\_recurso** </span> : Especifica la ruta del recurso\. Si no se especifica se asume index\.html

* <span style="color:#FFFF00"> **Otros**: </span> \(opcional\) adicionalmente se pueden indiciar parámetros y fragmentos\.

**Ejemplo URL**

`https://www.youtube.com/watch?v=dQw4w9WgXcQ`

* **Protocolo**: HTTPS

* **Usuario y contraseña:** \<ausentes>

* **Máquina:** www\.youtube\.com

* **Puerto:** <ausente\, se asume 443>

* **Recurso:** /watch

* **Parámetros:** ?v=dQw4w9WgXcQ

## Protocolo HTTP

El  <span style="color:#FFFF00"> **protocolo HTTP**  </span> \(HyperText Transfer Protocol\) es un protocolo de transferencia de hipertexto y establece las normas para el intercambio de información contenida en páginas web. Es importante saber que el protocolo HTTP  **no solo**  transmite ficheros de hipertexto\.  También  puede transmitir imágenes\, scripts\, estilos CSS\, etc\.

El protocolo HTTP funciona mediante mensajes de petición y respuesta codificados en ASCII\. Cada elemento de una página web se transmite de forma independiente con sus mensajes de petición y respuesta. De forma general HTTP funciona de la siguiente manera:

1. **Inicio comunicación:** un cliente \(navegador\) inicia una conexión TCP con un servidor web usando el puerto 80\.

2. **Solicitud HTTP: +** el cliente genera una solicitud HTTP con un método \(GET\, POST\, etc\.\) y la URL del recurso\.

3. **Procesamiento en el servidor:** el servidor procesa que recurso está solicitado y cómo debe responder

4. **Respuesta HTTP:** el servidor devuelve una respuesta HTTP al cliente\. Incluye un código de estado con el resultado de la solicitud y los recursos solicitados \(si los hubiera\)

5. **Renderizado en el cliente:** el cliente interpreta y renderiza los datos recibidos y se los muestra al usuario\.

### Funcionamiento HTTP


1\. El usuario escribe la URL del recurso que quiere buscar\.

![](img%5CT4%20Servidores%20Web0.png)



2\. El cliente se pone en contacto con un servidor DNS para que resuelva el nombre `www.mec.es`

![](img%5CT4%20Servidores%20Web1.png)



3\. El servidor DNS responde con la IP del servidor web asociado a la dirección `www.mec.es`

![](img%5CT4%20Servidores%20Web2.png)



4\. Antes de empezar la conexión HTTP\. Primero\, el cliente solicita una conexión TCP con el servidor web\.

![](img%5CT4%20Servidores%20Web3.png)


5\. El servidor web acepta la conexión TCP

![](img%5CT4%20Servidores%20Web4.png)


6\. El cliente usa la conexión TCP establecida para hacer una petición HTTP del documento `index.html`

![](img%5CT4%20Servidores%20Web5.png)

7\. El servidor web responde con un mensaje de respuesta que incluye el fichero `index.html` más un código de estado\.

![](img%5CT4%20Servidores%20Web6.png)


8\. Finalmente\, una vez que el cliente ha recibido el fichero `index.html`, el navegador se encarga de interpretarlo y representarlo por pantalla\.

![](img%5CT4%20Servidores%20Web7.png)

### Otros recursos en las respuestas HTML

Normalmente los ficheros HTML que se solicitan mediante una solicitud HTML contienen referencias a imágenes u otros recursos\. El servidor servirá estos recursos de dos maneras:

* <span style="color:#FFFF00">En la misma respuesta \(</span>  <span style="color:#FFFF00">inline</span>  <span style="color:#FFFF00">\): </span> el servidor puede enviar el fichero HTML y los recursos\, como imágenes\, incrustados en la misma respuesta\. Esto reduce el número de peticiones necesarias\, pero aumenta el tamaño de las respuestas\.

* Alternativamente\, el servidor puede  <span style="color:#FFFF00">responder solo con el archivo HTML</span> \. El navegador luego verá las referencias a otros recursos multimedia y  <span style="color:#FFFF00">realizará consultas independientes </span> para recuperar esos recursos\. Esto permite que la página se cargue de forma más eficiente\.

## Mensajes HTTP

La estructura de los mensajes HTTP tienen tres partes:
  * Línea de petición/respuesta
  * Encabezados
  * Cuerpo

![](img%5CT4%20Servidores%20Web8.png)

### Mensajes de petición

En un mensaje de petición nos encontramos los siguientes elmentos:

* <span style="color:#FFFF00">Método/verbo: </span> indica la acción que se debe realizar sobre el recurso.

* <span style="color:#FFFF00">Dirección URI </span> del recurso que se solicita.

* <span style="color:#FFFF00">Versión HTTP </span> usada.

* <span style="color:#FFFF00">Encabezados: </span> cada uno en una línea.

* <span style="color:#FFFF00">Cuerpo: </span> contiene el grueso de la información enviada\. Suele estar vacío para las peticiones con las peticiones `GET`\.

![](img%5CT4%20Servidores%20Web9.png)

El **método/verbo** indica lo que se quiere hacer con el recurso\. Los más comunes son `GET` y `POST`\.

`GET`: solicita un recurso como páginas web\, imágenes\, etc

`POST`: envía datos al servidor\. Ejemplo: formularios\, envíos de datos\, etc\.

`PUT` actualiza o crea un recurso en el servidor

`DELETE` elimina un recurso en el servidor\.

Existen otros métodos como `HEAD`\, `OPTIONS`\, `PATCH` y `TRACE`\. Consultar [documentación.](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)



En los **encabezados** se introduce información relativa  a la petición\. Algunos de los encabezados son:

* `HOST` equipo al que se envía la petición.

* `user-agent` nombre y versión del cliente usado.

* `accept` tipo de contenido que acepta el navegador.

* `accept-languages` idiomas esperados.

* `accept-encoding` sistema de codificación usado.

* `referer` </span> URL desde donde se originó la petición\.

* `cookie` contenido de la [cookie](https://www.kaspersky.es/resource-center/definitions/cookies)\.

Para más headers consular la [documentación.](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)


### Mensajes de respuesta



En un  mensaje de respuesta nos encontramos campos muy parecidos a los de la petición:

* <span style="color:#FFFF00">Versión del protocolo</span>

* <span style="color:#FFFF00">Código de respuesta</span>: indica el tipo de respuesta recibida. Éxito, redirección, error, etc.

* <span style="color:#FFFF00">Encabezados: </span> Cada uno en una línea

* <span style="color:#FFFF00">Cuerpo. </span> 

![](img%5CT4%20Servidores%20Web11.png)

El  **código de respuesta** es un número de tres cifras que indica si una petición se ha recibido y atendido correctamente o si ha habido algún problema\. Se agrupan según el primer dígito:

* `1XX` Información

* `2XX` Éxito

* `3XX` Redirección

* `4XX` Error del cliente

* `5XX` Error del servidor



Los códigos de respuesta más comunes son:

<img style="float: right;" src="img%5CT4%20Servidores%20Web14.png">

* `200` OK

* `204` No content

* `301` Moved Permanently

* `302` Found (Moved Temporaly)

* `404` Not Found

* `401`Unauthorized

* `500` Internal Server Error

* `503` Service Unavailable

## Virtual Hosting

El concepto de  <span style="color:#FFFF00"> **virtual hosting** </span> consiste en hacer funcionar más de un sitio web en el mismo servidor\. Por ejemplo\,` www.pagina1.com` y `www.pagina2.com`.
Esto se puede configurar de dos maneras:
  * <span style="color:#FFFF00">Basados en direcciones IP: </span> en este caso el mismo servidor tiene varias interfaces de red y cada una atiende a un sitio web distinto
  * <span style="color:#FFFF00">Basado en nombres diferentes</span> : dos sitios web con distinto dominio conviven en la misma máquina bajo la misma IP
Nosotros nos centraremos en el segundo caso\.

## Apache2

Existen muchos tipos de servidores disponibles para montar un servicio Web. En nuestro caso vamos a usar [Apache2](https://httpd.apache.org/). El motivo por el que vamos a usar este servidor es porque nos ofrece muchas ventajas:
* **Amplia experiencia**: Apache lleva mucho tiempo en desarrollo lo que hace que el servidor tenga muchas funcionalidades y sea muy estable.
* **Código abierto**: lo que nos permite modificar librevemente el servidor según nuestras necesidades.
* **Multi-plataforma**: Apache está en varias plataformas aunque nosotros lo usaremos en Linux
* **Modular**: Apache es modular lo que facilita mucho la configuración y el mantenimiento del servidor.
* **Popular**: Apache es uno de los servicios web más usados lo que facilita mucho la búsqueda de ayuda en foros, documentación, solución de errores, etc.

Antes de empezar la instalación y configuración del servidor conviene tener a mano el [índice de directivas](https://httpd.apache.org/docs/2.4/mod/directives.html) que nos va a servir mucho de ayuda para comprobar rápidamente la sintaxis de una directiva y los atributos que puede tomar.

También añadiré aquí un enlace a los video turoriales de YouTube cuando estén disponibles.

