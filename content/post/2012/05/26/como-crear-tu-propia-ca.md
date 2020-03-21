---
categories:
- Off-topic
date: "2012-05-26T21:05:44+02:00"
title: Como crear tu propia CA
---
Bueno, pues me he decidido a escribir este artículo por una razón. Que la gente no se vuelva loca buscando por internet como yo he hecho. No voy a discutir las ventajas de pasarse al SSL, y voy a ir al grano.

En primer lugar, necesitamos tener instalada la librería y los comandos de openssl. Después, ya solo tienes que elegir el sitio en el que quieres poner tu AC (autoridad de certificación, CA en adelante).

Ten en cuenta que tiene que ser un sitio de máxima seguridad, ya que si tu clave privada de la CA se ve comprometida, eso es irreparable. Tendrías que cambiar todos los ordenadores en los que hayas instalado el citado certificado de CA, y recrear todos los certificados que hayas creado.

Antes de nada, un pequeño consejo y una aclaración. Ten siempre a mano los docs de openssl ([http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html)), y si ves que los documentos son de hace 10 años, no te asustes, que casi seguro que siguen siendo válidos.

# Creación de la carpeta de CA

Pues allá vamos. Hay dos maneras de hacer las cosas actualmente en openssl, y no tiene mucha pinta de cambiar. Una a mano, y otra semiautomática. Como no queremos tener que hacerlo todo a mano, vamos a dejar que se encargue él de lo que pueda.

Para empezar, ya tienes que haber elegído la carpeta en la que vas a poner tu CA. Ten en cuenta que tiene que tener backups (depende del trote que le des, cada vez que haya cambios o periodicamente) y que tienen que ser protegidos. Ahí, monta el siguiente esquema de directorios:

```
./                          # Esta es la ruta en la que has decidido poner tu CA
./private/                  # Aquí vamos a guardar la clave privada del certificado
./newcerts/                 # Aquí guardamos los certificados expedidos,
                            # pero nosotros no tocaremos la carpeta una vez creada
```

La idea básica es que nosotros, utilizando la clave privada que hay en private/, crearemos un certificado firmando nuestra clave pública con nuestra clave privada. Esto significará que el certificado es certificado por sí mismo, lo que viene a ser una CA (entidad que se autentica a sí misma). No depende de ningún tercero.

Esta es la razón por la que a la hora de querer que los clientes no encuentren extrañas nuestros certificados, debemos hacer que tenga como Autoridad Certificadora nuestro certificado (root).

Ahora, vamos a crear unos cuantos archivos que nos serán necesarios para que el comando semiautomático funcione correctamente.

El primero, es crear un archivo ./serial. Este archivo, como dice el nombre, tiene algo que ver con un número de serie. Y en caso de que te preguntes que es, solo te diré que todos los certificados emitidos por una CA llevan un número de serie único. De esta manera, luego al hacer una lista de certificados revocados, hay una forma en la que identificar a los certificados de una manera inequívoca.

Por lo tanto creamos en ./ el archivo _serial_ en el que pondremos un número por el que queramos empezar los certificados. Yo empezé por el 1, pero este hay que ponerlo de una manera extraña (por alguna razón que desconozco), algo como "0000001". deja que esa sea la única línea en el archivo.

Después vamos a crear el archivo ./index.txt este vale para ir guardando todos los certificados que se han firmado, con sus correspondientes peticiones. Lo más importante es que ese archivo esté vacío (al principio). Basta con hacer un touch ./index.txt

Y por el momento, ya tenemos casi preparado el directorio. En teoría, ya lo tenemos preparado, pero como soy un poco mío, me gusta que todas las cosas necesarias estén en el mismo sitio para que al hacer un backup/restore no pierda información vital.

El siguiente paso, trata de crear un archivo de configuración para nuestra entidad certificadora

# Creación del archivo de configuración

Aqui vamos a crear el archivo de configuración línea a línea.

Lo primero que vamos a hacer es crear la sección default del CA.

```
# OpenSSL configuration file

# Voy a separar las diferentes partes entre sí por 3 filas de almohadillas ( ca, request etc.)

# y las diferentes configuraciones 2 filas de almohadillas y las secciones suplementarias a

# cada configuración, por 1 fila.



# La única variable global va a ser esta. Tiene que señalar al directorio en el que se está

# Se tiene que poner el valor de PWD, sin la barra al final, ej: /home/txomon/.sslca

# Como mi configuración la tengo hecha en una unidad montada, no me puedo permitir asignarle

# una ruta absoluta, pero yo lo recomiendo, para que así todo lo que se haga, no quepa duda de

# que puede fallar por esto. Tener una ruta relativa tiene el problema de que es relativa al

# directorio del usuario, lo que es un problemón.

dir = .

###############################################################################################

###############################################################################################

###############################################################################################

# Esta es la sección que se lee cuando se utiliza el comando ca en ella se especifican algunas

# cosas, pero la única que nos vale es default_ca

[ ca ]

default_ca = CA_default # cuando no se especifique una sección de referencia, se utilizará esta

###############################################################################################

###############################################################################################

# Esta es la sección que será llamada por defecto (por que está especificada en [ca])

[ CA_default ]

# El archivo en el que se va a guardar el último número de serie expedido

serial = $dir/serial

# El archivo que se va a utilizar como base de datos para guardar los certificados expedidos

database = $dir/index.txt

# El directorio en el que se van a guardar los certificados expedidos

new_certs_dir = $dir/certs

# El certificado de la CA autofirmado.

certificate = $dir/ca-certificate.pem

# La clave privada de la CA, a proteger con la máxima seguridad

private_key = $dir/private/ca-key.pem

# Los días por los que el certificado que se expida será válido

default_days = 730 # dos años de 365 días

# El algoritmo de checksum que se va a utilizar de la clave pública para

# despues firmarlo. Los tipos están documentados en el man de openssl

# MESSAGE DIGEST COMMANDS

default_md = sha512 # Este es el que más me gusta (más largo, menos colisiones)

# Esta opción vale para especificar si hay que respetar o no el orden de los CN (common name)

# básicamente es compatibilidad hacia atrás. Como no nos molesta, lo dejamos en yes

preserve = yes

# Esta opción es para la inclusión o no de los emails que vienen en las request.

# Hasta el momento no he encontrado una justificación, pero si lo recomiendan, supongo yo que

# por algo será.

email_in_dn = no

# Aquí, se podría especificar como queremos que se nos muestren los requests. Como no tengo

# ningún tipo de referencia, me limitaré a dejar aquí los campos comentados. Las opciones que

# admiten estos campos son las del man x509, en las secciones:

# NAME OPTIONS para

# nameopt = ca_default #por pillar uno

# TEXT OPTIONS para

# certopt = ca_default

# Este es ya el último apartado, en él, vamos definir las políticas de
```
