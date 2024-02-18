# Práctica 4: Control de acceso

En esta prática veremos que formas de control de acceso a usuarios permite Apache. En concreto veremos el método *basic* y el método *digest*.

Cabe comentar que en la actualidad no se suele realizar la autenticación a nivel de Apache. Lo más común es tener otro tipo de servicio que gestione la autenticación e integrarlo en las páginas.

## Requisitos

Como siempre partimos del sitio web `www.mipagina.example` que hemos estado usando hasta ahora. 
 

## Desarrollo 

Realiza las siguiente configuraciones al tu sitio web. **Nota importante:** después de cada configuración elimina/comenta las lineas utilizadas para que no alteren las demás configuraciones.

#### Ejercicio 1:

Configura el servidor para que solo se pueda acceder desde la red interna.

#### Ejercicio 2:

Configura el servidor para que no se pueda acceder a los archivos `.txt` desde ningún lugar.

#### Ejercicio 3:

Crea el directorio `privado` dentro del document root de tu sitio web. Crea al usuario alicia usando el comando `htpasswd` para que pueda ser autorizado en apache.

Configura la autenticación *basic* de apache para que solo pueda acceder a los recuros dentro de `privados` el usuario alicia.

Nota: requiere el módulo `auth_basic`.

#### Ejercicio 4:

Crea el grupo `usuariosApache` según lo indica la directiva `AuthGroupFile` al que pertenezca Alicia. A continuación utiliza las directivas `AuthGroupFile` y `Require Group` para darle acceso a l grupo al directorio `privado`.

#### Ejercicio 5:

Configura el servidor para que solo se pueda acceder al sitio web `www.mipagina.example` si se hace desde la red interna **o** si se usa el grupo `usuariosApache`.

#### Ejercicio 6:

Configura el servidor para que solo se pueda acceder al sitio web `www.mipagina.example` si se hace desde la red interna **y además** se usa el grupo `usuariosApache`.

