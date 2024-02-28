# Práctica 6: TLS

En esta práctica vamos a ver como se implementa el SSL/TLS en el servidor. Es decir, como activamos la encriptación para que se pueda acceder al servidor mediante https.

El proceso de configurar el TLS tiene cuatro partes:

1. Creación de la autoridad certificadora (CA).
2. Creación de una solicitud de certificado en el servidor apache. Luego este certificado debe ser firmado por la CA.
3. Configuración del servidor apache con el certificado firmado
4. Instalación del certificado de la CA en los navegadores de los clientes.

Veamos los pasos uno a uno.

## Creación de la autoridad certificadora (CA)

Crearemos la autoridad certificadora en otra máquina Ubuntu. Una vez creada la máquina debemos instalar el paquete permite su creación:
```
sudo apt update
sudo apt install easy-rsa
```
Luego vamos a crear un directorio para la CA. Por ejemplo en la carpeta home:
```
mkdir ~/easy-rsa
```
Copiaremos los paquetes instalados por el paquete anterior en nuestra carpeta:
```
cp /usr/share/easy-rsa/* ~/easy-rsa
```
Por último devemos iniciar la infraestrucutra de clave pública (PKI) usando el escrip del directorio:
```
cd ~/easy-rsa
./easyrsa init-pki
```
Ya estamos listos para crear la CA. Primero crearemos un fichero `vars` con los datos de la CA:
```
cd ~/easy-rsa
nano vars
```
y en él escribimos los siguientes datos:
```
set_var EASYRSA_REQ_COUNTRY    "ES"
set_var EASYRSA_REQ_PROVINCE   "Madrid"
set_var EASYRSA_REQ_CITY       "Madrid"
set_var EASYRSA_REQ_ORG        "CApublica"
set_var EASYRSA_REQ_EMAIL      "admin@CApublica.es"
set_var EASYRSA_REQ_OU         "Comunidad"
set_var EASYRSA_ALGO           "ec"
set_var EASYRSA_DIGEST         "sha512"
```
En el bloque de arriba los únicos valores importantes son los dos últimos que corresponden al tipo de algoritmo y al método de encriptación usado. Todos los demás se pueden ajustar a gusto.

Una vez creado el archivo ya podemos iniciar la CA:
```
./easyrsa build-ca
```
se os pedirá que introduzcáis un nombre para la CA o podéis dejar el de por defecto.
```
. . .
Enter New CA Key Passphrase:
Re-Enter New CA Key Passphrase:
. . .
Common Name (eg: your user, host, or server name) [Easy-RSA CA]:

CA creation complete and you may now import and sign cert requests.
Your new CA certificate file for publishing is at:
/home/sammy/easy-rsa/pki/ca.crt
```
Veréis que tras la creación de la CA se habrá creado un archivo muy importante. `/home/sammy/easy-rsa/pki/ca.crt`. Este archivo contiene el certificado de la CA y deberemos instalarlo en los navegadores de los clientes para que acepten nuestros certificados.

## Creación de una solicitud de certificado en el servidor apache.

Ahora que tenemos nuestra CA construida y operativa podemos crear una solicitud de certificado para que sea firmada por ella.

**IMPORTANTE**: En esta sección cambiaré entre el servidor apache y la CA. Será indicado en todo momento donde se ejecuta cada comando mediante un comentario.
```
# Apache

# CA
```

Empezamos asegurándonos que tenemos instalado el paquete `openssl` en nuestro servidor apache.
```
# Apache
sudo apt update
sudo apt install openssl
```
Ahora ya podemos iniciar el proceso de creación del certificado. Primero crearemos una carpeta para ello y dentro de ella la clave privada del certificado.
```
# Apache
mkdir ~/certificado
cd ~/certificado
openssl genrsa -out apache.key
```
Esto nos habrá generado el archivo `apache.key` que contiene la clave privada del servidor. Ahora generaremos una solicitud usando la clave privada:
```
# Apache
openssl req -new -key apache.key -out apache.req
```
En este punto ya tendremos nuestro certificado, `apache.req`. Ahora simplemente debemos mandar el certificado  a la CA para que lo firme. Para ello podemos usar por ejemplo SSH y el comando `scp`.
```
# Apache
scp apache.req administrador@<IP_de_la_CA>:/tmp
```
Tras este comando el certificado del apache debería estar guardado en la carpeta `/tmp` de la CA.

El siguiente paso es importar la solicitud del certificado:
```
# CA
cd ~/easy-rsa
./easyrsa import-req /tmp/apache.req apache
```
Por último firmamos el certificado mediante el siguiente comando:
```
# CA
./easyrsa sign-req server apache
```
Saldrá un mensaje informándonos que nos aseguremos que queremos firmar el certificado. Además nos pedirá la contraseña.
```
You are about to sign the following certificate.
Please check over the details shown below for accuracy. Note that this request
has not been cryptographically verified. Please be sure it came from a trusted
source or that you have verified the request checksum with the sender.

Request subject, to be signed as a server certificate for 3650 days:

subject=
    commonName                = apache


Type the word 'yes' to continue, or any other input to abort.
  Confirm request details: yes
. . .
Certificate created at: /home/administrador/easy-rsa/pki/issued/apache.crt
```
Si todo ha salido correctamente se habrá generado el certificado ya firmado en la ruta:
```
/home/administrado/easy-rsa/pki/issued/apache.crt
```
Solo faltaría mandar una copia del certificado firmado del apache y una copia del propio certificado de la CA:
```
#CA
cd ~/easy-rsa
scp pki/issued/apache.crt administrador@<IP_servidor_apache>:/tmp
scp pki/ca.crt administrador@<IP_servidor_apache>:/tmp
```
Si las transferencias se han realizado correctamente los dos certificados estarán dentro de la carpeta `/tmp` del servidor apache. Recomiendo que los copiéis dentro de la carpeta `~/certificados` que creamos anteriormente. 

En este punto se puede apagar la CA, no la volveremos a necesitar.

## Configuración del servidor apache con el certificado firmado

Ahora ya tenemos los certificados en nuestro servidor apache. Solo hace falta crear el sitio e incluir el certificado creado.

Primero vamos a mover a el certificado y la clave privada al lugar que les corresponde dentro de la carpeta `/etc/ssl`:
```
cd ~/certificados
sudo cp apache.crt /etc/ssl/certs/apache.crt
sudo cp apache.key /etc/ssl/private/apache.key
```
Ahora que están el certificado y la clave donde deben estar ya podemos crear el sitio web. Para ello vamos a usar el archivo de configuración por defecto preparado para TLS que se encuentra dentro de la carpeta de sitios del apache:
```
cd /etc/apache2/sites-available
sudo cp default-ssl.conf <nombre>.conf
```
Configuraremos el fichero con toda la información necesaria: nombre del sitio, document root, ficheros de log, etc. Por último debemos cambiar las líneas que especifican cuáles son los ficheros de la clave privada y el certificado.
```
SSLCertificateFile /etc/ssl/certs/apache.crt
SSLCertificateKeyFile /etc/ssl/private/apache.key
```
Además para que funcione el sitio debemos asegurarnos que está habilitado el módulo del TLS:
```
sudo a2enmod ssl
```
¡No te olvides también de activar el sitio web y  reiniciar el servidor!

Si has conseguido seguir los pasos hasta aquí ya tendrás el servidor listo para recibir peticiones HTTPS. Solo faltaría configurar instalar el certificado de la CA en el navegador del cliente

##  Instalación del certificado de la CA en los navegadores de los clientes.

Para instalar el certificado de la CA en el navegador del cliente primero deberemos mandarle una copia del mismo. Podemos hacerlo usando SSH como anteriormente:
```
cd ~/certificados
scp ca.crt administrador@<IP_cliente>:/tmp
```
Una vez transferido el certificado deberemos instalarlo en nuestro navegador. Cada navegador tendrá unos pasos diferentes para la instalación del certificado. En el caso de Firefox el proceso es simple:

1. Vamos a Ajustes
2. En el cuadro de búsqueda buscamos certificados
3. Hacemos click en `ver certificados...`
4. Hacemos click en `importar` y buscamos el certificado.
5. Le damos a aceptar

Por último comprobamos si funciona la conexión conectándonos a nuestra página mediante https
```
https://<nombre_sitio_web>
```
No os olvidéis de incluir el `https` para forzar el uso del TLS.

Y con esto concluye el proceso de configuración del TLS.

