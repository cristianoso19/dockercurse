# Curso de DOCKER
This is the notes of docker curse at platzi
## Introducción
### Las tres areas en el desarrollo profesional de software
Existen 3 grandes problemas que estaran presente en toda tecnogía y lenguaje:
* Construir: Escribir es solo una pequeña parte, los problemas complejos necesitan equipos.
  * Entorno de desarrollo.
  * Dependencias, frameworks, bibliotecas.
  * Entorno de ejecución. Versiones de node etc. 
  * Equivalencia con el entorno productivo.
  * Servicios externos. Base de datos. S.o.
* Distribuir: Llevar lo que nosotros hacemos a al lugar donde debe estar.
  * Divergencia de repositorios
  * Divergencia de artefactos
  * Versionado
* Ejecutar: tiene que correr el programa en algun lugar.
  * Compatibilidad con entornos productivos.
  * Dependencias.
  * Disponibilidad de servicios externos.
  * Recursos de hardware.

**Docker**: Permite construir, distribuir y ejecutar cualquier aplicación en cualquier lado.

## Virtualización.
Es la forma de solucionar los tres problemas.
Es la version virtual de algun recurso tecnológico como hardware, so, almacenamiento, o recurso de red.
Permite atacar en simultaneo los tres problemas del software profesional.

Problemas con las maquinas virtuales:
* Tamaño.
* Costo de administración.
* Multiples formatos: VDI, VMDK, VHD raw, etc.

**DOCKER** usa **CONTENEDORES**
El empleo de contenedores para construir y desplegar software.

Los contenedores son:
 * Flexibles
 * Livianos
 * Portables
 * Bajo acoplamiento
 * Escalables
 * Seguros

<img src="https://www.redeszone.net/app/uploads/2016/02/docker-vs-virtual-machines.png" />

### Preparando tu entorno de trabajo.
Pasos a seguir:

* Ir a https://www.docker.com/get-started
* Descargar Docker Desktop e instalarlo
* Crear cuenta en Docker Hub

`docker --version`: Visualizar version de docker
`docker info`: Visualizar información de docker
### Play with docker

* Ir a https://labs.play-with-docker.com/
* Ingresar con nuestra cuenta docker hub.

Es una sesion en la nube que se puede usar por 4 horas.
* Creamos una instancia. Abre una terminal nueva.

### Qué es y cómo funciona Docker.
<img src="https://ualmtorres.github.io/SeminarioDockerPresentacion/images/DockerEngine.png"/>

* `docker daemon` Corazón de docker o servidor de docker. 
* `REST API` Para comunicarse con el mundo exterior o localmente. Es posible exponer el docker daemon **No es recomedable pero es posible**
* `docker-cli` cliente de docker para terminal, habla con el docker daemon. 
* `contenedores` Lo mas importante, donde van a correr nuestras aplicaciones.
* `imagenes` Artefactos que usa docker para empaquetar contendores, para poder distribuir
*LOUF5sqix7nuct*rauhLOUF5sqix7nuct*rauh `volumenes de datos` Es la forma en la que docker nos permite acceder de manera segura y flexible a el sistema de archivos de la maquina anfitriona.
* `network` Le permite a los distintos contenedores de docker entre si o con el mundo exterior.

### Primeros pasos: Hola mundo
Para correr tu primer contenedor:
`docker run hello-world`
Este comando solo arroja el siguiente texto
<img src="https://static.platzi.com/media/user_upload/docker_run-4a1c7a71-25c7-4c35-a996-522dd2cc5345.jpg" />

### Conceptos fundamentales de Docker: CONTENEDORES

Un contenedor es una MV liviana, es una agrupación de procesos que corre nativamente en la máquina pero estan aislados del resto del sistema, es una unidad lógica, por eso puede correr de forma nativa en la maquina anfitriona.
El contenedor esta limitado a que cosas puede ver o acceder del anfitrión. Esto es configurable.

### Comprendiendo el estado de Docker
```bash
#Correr el contenedor hello-world
docker run hello-world
#Muestra los contenedores ACTIVOS
docker ps
#Muestra TODOS los contenedores
docker ps -a
#Muestra el detalle de un contedor por el ID
docker inspect f6bf25fd84ac
#Muestra el detalle de un contedor por el NAME
docker inspect vibrant_goldwasser
#Asigna un nombre a el contenedor hello world
docker run --name hello-platzi hello-world 
#Cambio de nombre al contenedor
docker rename hello-platzi hola-platzi
#Elimina el contenedor hola-platzi
docker rm hola-platzi
#Borro todos los contenedores en un solo comando
docker container prune
```
> Docker no permite correr dos contenedores con el mismo `NAME`
### Modo interactivo
```bash
# Correr ubuntu en modo interactivo 
# -i=interactivo t= terminal tty
docker run -it ubuntu bash
# Ver la version del sistema operativo
cat /etc/lsb_release
# Salir del terminal del contenedor
exit
```
### Ciclo de vida de un contenedor.
Cada vez que un contenedor se ejecuta, se ejecuta un proceso del sistema operativo.
* Un contenedor solo funciona mientras este "vivo" su proceso principal (en el caso de ubutu es bash).
> Es importante cuando el MAIN PROCESS falla o se detiene el contenedor tambien para.
Para que no se cierre el contenedor sin entrar al modo interactivo.
`docker run --name alwaysup -d ubuntu tail -f /dev/null`
* Al final se especifica el comando que quiero que se ejecute
* Ahora para conectarse a un contenedor que esta corriendo
`docker exec -it alwaysup bash`
> Mientras el proceso principal este corriendo el contenedor estara corriendo
* Para matar el proceso se busca el process id 
`docker kill alwaysup`
### Exponiendo contenedores
Para exponer el contenedor al sistema exterior.
```bash
#Desplegar nginx en docker
$ docker run -d --name proxy nginx 
#Para el contenedor
$ docker stop proxy 
#Borra el contenedor
$ docker rm proxy
#Para pararar y borrar el contenedor
$ docker rm -f <contenedor> 
#Desplegar nginx y exponerlo en el puerto 8080, ANFITRION:CONTENEDOR
$ docker run -d --name proxy -p 8080:80 nginx 
#Acceder desde el navegador
localhost:8080 
#Ver los logs
$ docker logs proxy
#Follow a los logs
$ docker logs -f proxy 
#Follow de los 10 ultimos logs
$ docker logs --tail 10 -f proxy 

## Datos en DOCKER
### Bind mounts
Los contenedores son entidades que no pueden acceder a los datos de la maquina anfitrion, a no ser que lo permitamos.
> Existen casos en los que se necesita compartir archivos
Esto se llama **BIND MOUNT** Y lo que hace es espejar todo lo que se encuentre de un directorio del contenedor en el directorio asignado en el anfitrion.
```
```bash
#Creo un directorio
mkdir dockerdata 
#Despliego un contenedor -v para hacer el BIND MOUNT
docker run -d --name db -v {DIRECTORIO_ANFITRION}:{DIRECTORIO_CONTENEDOR} mongo   
#Revisamos el contenedor de nombre bd
docker ps 
#Entramos al bash del contenedor
docker exec -it db bash (entro al bash del contenedor)
#Ejecutamos el cli de MONGO.
mongo (me conecto a la BBDD)
```
#### Comandos usados de MONGODB
- shows dbs (listo las BBDD)
- use platzi ( creo la BBDD platzi)
- db.users.insert({“nombre”:“guido”}) (inserto un nuevo dato)
- db.users.find() (veo el dato que cargué)
> Si listamos los archivos dentro de la carpeta docker data esta tendra varios archivos esto por que se todo el contenido del directorio contenedor fue linkeado o creado en el directorio anfitrión.

> Tiene un riesgo, se da acceso a un contenedor a una parte del disco, puede ser peligroso.

### Volumenes
Es una evolución de los BIND MOUNT por problemas de seguridad o privacidad.
Ya que no se sabe donde estan los archivos
```bash
#Listo todos los volumenes creados
docker volume ls 
#Creamos un nuevo volumen
docker volume create dbdata
#Desplegamos el contenedor, con --mount usara el volumen creado, se le debe especificar el destiono
docker run -d --name db --mount src=dbdata,dst=/data/db mongo 
#Podemos revisar todos los datos creados
docker inspect db
mongo 
```
Es una manera muy practica de compartir archivos entre contenedores sin compartir un directorio
### Insertar y extraer datos de volumenes o contenedores.
Es independiente si usamos bindmounts o volumenes
```bash
#Creamos un archivo 
touch prueba.txt
#Corremos un nuevo contenedor llamado copytest
docker run -d --name copytest ubuntu tail -f /dev/null
#Copiamos el archivo dentro del directorio testing del contenedor
#y le cambiamos el nombre
#PARA COPIAR DESDE LOCAL A CONTENEDOR 
docker cp prueba.txt copytest:/testing/test.txt
#PARA COPIAR DESDE CONTENEDOR AL LOCAL 
docker cp copytest:/testing localtesting
```
>NO ES NECESARIO QUE EL CONTENEDOR ESTE CORRIENDO PARA COPIAR

## IMAGENES
### CONCEPTOS FUNDAMENTALES DE DOCKER: IMAGENES
Es como DOCKER intenta solucionar los dos problemas restantes:
* Construccion
* Distribución
Las imagenes son plantillas a partir de las cuales se crean contenedores
Contiene todo lo necesario para que pueda correr normalmente como:
* Dependencias 
* Codigo
* Herramientas
* Configuración

<table><tr><td>Una imagen de docker se conforma de distintas capas de personalización, partiendo de la imagen base o BASE IMAGE que es la imagen de un SO</td></tr></table>

```bash
#Repository: El repositorio que se usa al ejecutar docker run
#Tag: version de la imagen, si no se especifica usa la "latest"
#size: tamaño de la imagen
docker image ls
```
> 😀Una imagen vive como un conjunto de archivos o "CAPAS"
Para traer una imagen ejecutamos: 
`docker pull ubuntu:20.04`

### CONSTRUYENDO UNA IMAGEN PROPIA
El proceso de construcción se basa en el archivo llamada **DOCKERFILE**
con el comando **build** construimos contenedores
> Desde una imagen podemos generar infinitos contenedores
> 🚨Toda lo que esta en dockerfile se ejecuta en **BUILD** 

Contexto de build: a que parte del disco tiene acceso build mientras se essta ejecutando
Cada run genera una nueva capa **LAYER** que genera como resultado una **IDD**
<img src="https://static.platzi.com/media/user_upload/carbon%20%283%29-975f4d64-9144-4a75-a8ba-0eceba66db50.jpg"/>
<img src="https://static.platzi.com/media/user_upload/Screenshot%20from%202020-11-06%2019-53-30-a305c998-0991-44ad-9319-80cacb1a4bc7.jpg"/>

> Para publicar un contenedor ejecutar: `docker push` pero antes retagear

> Para retagear una imagen ejecuta: `docker tag ubuntu:platzi cristianoso19/ubuntu:platzi`

Al retagear creamos un nombre de un LAYER, no crea una nueva imagen.

<img src="https://i.ibb.co/JBL946b/Screenshot-at-Feb-05-15-26-18.png"/>

### EL SISTEMA DE CAPAS

Docker image es un conjunto de capas ordenadas, es fácil entender como esta hecha una imagen por su dockerfile y lo mejor es que las imagenes publicas tienen dockerfiles publicos 

> 🙄 cada comando como RUN en el docker file crea una capa 

Para ver todo las capas
`docker history ubuntu:platzi`

Para entender mejor como funciona las capas usar la herramienta dive:

<a href="https://github.com/wagoodman/dive"> DIVE </a>

> 🥃 Una capa es simplemente un cambio a la capa anterior, por consiguiente el tamaño de cada cambio es infimo.

## DOCKER COMO HERRRAMIENTA DE DESARROLLO

### USANDO DOCKER PARA EL DESARROLLAR APLICACIONES

Para ayudarnos descargar el siguiente repositorio:

```bash
git clone https://github.com/platzi/docker
#Creamos la imagen local
docker build -t platziapp . 
#Muestra las imagenes locales
docker image ls 
#Creamos un contenedor y cuando se detenga se borra, lo expone en el puerto 3000
docker run --rm -p 3000:3000 platziapp 
docker ps #veo los contenedores activos
```
Analisis del repositorio:

<img src="https://static.platzi.com/media/user_upload/carbon%20%2813%29-61ed207f-53ce-4792-8c09-8fe7225679be.jpg" />

### Aprovechando el caché de capas para estructurar correctamente tus imágenes
Una de las herramientas que nos da docker para optimizar tiempo es 

**LAYER CACHE**

Si queremos crear una capa que tenemos en el sistema docker la usara en vez de construirla

<img src="https://static.platzi.com/media/user_upload/dock9-1190f4d1-0a1e-4126-b422-fca6b894e286.jpg"/>

### Docker networking: colaboración entre contenedores
```bash
#Listar redes de docker
docker network ls 
#Crear una nueva red de nombre platzinet
docker network create --atachable plazinet 
#Ver detalles de la red creada
docker inspect plazinet 
#Creamos otro contenedor llamado db
docker run -d --name db mongo 
#Conecto el contenedor db a la red platzinet
docker network connect plazinet db 
#Conecto el contenedor platziapp a la red platzinet
docker network connect plazinet app 
#Iniciamos el contenedor platziapp, le pasamos una variable de entorno con --env
docker run -d -name app -p 3000:3000 --env MONGO_URL=mondodb://db:27017/test platzi 
```
>❕ Docker puede asignar una red al nombre de la imagen, no es necesario agregar la ip a la imagen

>❕ Existen 3 tipos de red que maneja docker
  * Null: red para contenedores aislados
  * Bridge: red para redes locales dentro del pc
  * Host: red hacer como si fuese un equipo dentro de la LAN fisica

## Docker compose
### La herramienta todo en uno
Lo que nos permite es describir de forma declarativa la arquitectura de servicios que nuestra necesita, como se comunican entre si o como se manejan archivos, todo esto en un archivo.

Decimos que servicios tiene nuestra aplicacion y algunos parametros para pasarle.
empieza con version, es la version del compose file.
services: indica los servicios que componenen nuestra aplicacion. Pensando en microservicios.

```bash
# Versión del compose file
version: "3.8"
# Servicios que componen nuestra aplicación.
## Un servicio puede estar compuesto por uno o más contenedores.
services:
# nombre del servicio.
  app:
  # Imagen a utilizar.
    image: platziapp
	# Declaración de variables de entorno.
    environment:
      MONGO_URL: "mongodb://db:27017/test"
	# Indica que este servicio depende de otro, en este caso DB.
	# El servicio app solo iniciara si el servicio debe inicia correctamente.
    depends_on:
      - db
	# Puerto del contenedor expuesto.
    ports:
      - "3000:3000"

  db:
    image: mongo
```

> `docker-compose up` Corre la configuracion del .dockerfile

> `docker-compose up -d` Corre la configuracion del .dockerfile sin detach

> En YAML es muy importante el espaciado y tabulado.

<table><tr><td>⚠️**Despues de realizar CUALQUIER cambio en el DOCKERFILE o en el docker-compose.yml rebuildear la imagen con `docker-compose build`**</td></tr></table>

### Subcomandos de Docker Compose
Docker compose crea un nombre único, basado en la carpeta que esta contenido el dockerfile.
Lo hace para diferenciar a distintos containers de un mismo servicio
Docker compose trabaja con servicios y no con containers
Docker compose conecta todos los contenedores del mismo servicio a una red.

```bash
#Listar las redes.
docker network ls 
#Inspeccionamos detalles de red
docker network inspect docker_default
#Ver los logs
docker-compose logs 
#Ver solo logs de la app
docker-compose logs app 
#Hago un follow de los logs de app y db
docker-compose logs -f app db
#Entro a la shell del contenedor app
docker-compose exec app bash 
#Ver los contenedores generados por docker compose
docker-compose ps 
#Borrar todo lo generado por docker compose
docker-compose down
```

### Docker Compose como herramienta de desarrollo
Dentro del docker-compose.yml se puede mandar a buildear una imagen con `build: .` y luego ejecutar el comando `docker-compose build`

Archivo docker-compose.yml
```bash
version: "3.8"

services:
  app:
	# crea una imagen con los ficheros del directorio actual.
    build: .
    environment:
      MONGO_URL: "mongodb://db:27017/test"
    depends_on:
      - db
    ports:
      - "3000:3000"
	# Sección para definir los bindmount.
    volumes: 
			#<local path>:<container path> # el directorio "<.>" actual   se montará en "/usr/src" en el contenedor.
      - .:/usr/src
			# indica que ficheros debe ignorar
      - /usr/src/node_modules
	# Permite pasarle un comando a ejecutar al servicio app.
    command: npx nodemon  index.js

  db:
    image: mongo
```

>El nombre que elige docker para esta imagen es el nombre del directorio-nombreaplicacion-numero, en este ejemplo quedaria asi: docker-app-1

<table><tr><td>⚠️Podemos cambiar el comando del build en el archivo docker-compose.yml con el comando `command: npx nodemon -L index.js` </td></tr></table>

<img src="https://static.platzi.com/media/user_upload/carbon%20%2822%29-a2940c20-5014-4af0-a654-39208e9879b6.jpg"\>
 
### Compose en equipo: override
Esto resuelve el problema de desarrollo en equipo.
> compose override: sirve para personalizar pequeños cambios propios para nuestro ambiente sobre el compose file original.

Existe la posibilidad que docker-compose trate de hacer un merge (una las configuraciones), ese es el caso de la variable environment que une las variables que esten dentro de esta, y si existe la sobreescribe.

Los ports, son recomendables usarlos en un mismo archivo, no en los dos, genera un problema por que docker-compose lo compila en etapas distintas.

Esta es otra forma de personalizar sin tener miedo de complicar la vida a nuestros compañeros de trabajo.

>Es recomendable agregarlo al .gitignore

### Escalabilidad

`docker-compose up -d --scale app=2`

Esto levanta dos contenedores de app en cada uno de los puertos.

Para ejecutar esto antes debemos editar la parte de los puertos para esto se debe definir un rango de puertos
```bash
ports:
  - "3000:3001:3000"
```

Es una forma practica de simular problemas de concurrencia

## Docker Avanzado

### Administrando tu ambiente de docker

Las imagenes van ocupando espacio, y consumen recursos, debemos hacer limpieza para no tener cosas que no deseamos.

```bash
#Ver los contedores en mi máquina
docker ps -a 

#Borra los contenedores inactivos
docker container prune 

#Borra todos los contenedores, activado o apagados
docker rm -f $(docker ps -aq) 

#Lista todas las redes
docker network ls 

#Lista todos los volumenes 
docker volume ls 

#Lista todas las imágenes
docker image ls

#Borra lo que no se este usando
docker system prune 

#Limitar el uso de memoria
docker run -d --name app --memory 1g platziapp 

#Ver stats de recursos consumidos por docker
docker stats

#Ver si el proceso muere por falta de recursos
docker inspect app 

```

<img src="https://static.platzi.com/media/user_upload/carbon%20%2823%29-91140de2-d380-44ba-9175-c03284cecd51.jpg" />

### Deteniendo contenedores correctamente: SHELL vs EXEC

Como docker se comunica con los procesos para terminarlos:
1. Primero envia SIGNTERM es un stardard de linux
2. Si no responde le envia una SIGNKILL apagando el proceso

Para que el proceso corra como el proceso principal en el contenedor docker colocar en el dockerfile:
`CMD ["/loop.sh"]`

Para enviar un SIGNTERM con docker, con esto se realiza un gracefull shutdown

`docker stop looper`

Para enviar un KILLTERM con docker

`docker kill looper`

Esto es muy simple pero poderoso

### Contenedores ejecutables: Entrypoint vs CMD

Para ejecutar programas como si fuesen ejecutables dentro del sistema asemos uso de:

>ENTRYPOINT: es el comando por defecto que correra siempre y usara comand como parametro del entrypoint.

```bash
#######Dockerfile#######
FROM ubuntu:trusty
ENTRYPOINT ["/bin/ping", "-c", "3"]
CMD ["localhost"]
########################

```

```bash
#en este comando <hostname> pisa o reescribe el CMD del dockerfile de esta manera podremos ejecutar el comando ping desde un docker
docker run --name pinger ping <hostname>
```

Esta es la manera mas facil de tener binarios autocontenidos que pueden correr en cualquier docker sin necesidad de instalarlos nativamente

>Usar la forma de EXEC y no la de SHELL, no combinarlos, siempre una de las dos. 

### El contexto de build

Es cada vez que hacemos el build de una imagen, docker va a montar en un file system temporal todo los archivos disponibles en la ruta que se le pasa como último parametro en `docker build`.

De esta manera se asegura de que dentro de un build el proceso no accedera a una parte no permitida.

Esto puede generar el contexto de build sea muy grande. 

El archivo .dockerignore podemos ignorar archivos o carpetas como si fuese .gitignore

### Multi-stage build.

Esto pasa en todos los proyectos grandes, no queremos que todo el codigo del proyecto este en la imagen final, especialmente en lenguajes compilados.

¿Como hacemos para que este codigo no este en el buil final?

Usamos dos archivos:
* developer.Dockerfile, el mismo de toda la vida
* production.Dockerfile, este lo vemos a continuacion
```bash
# Define una "stage" o fase llamada builder accesible para la siguiente fase
FROM node:12 as builder
# copiamos solo los archivos necesarios
COPY ["package.json", "package-lock.json", "/usr/src/"]

WORKDIR /usr/src
# Instalamos solo las dependencias para Pro definidas en package.json
RUN npm install --only=production

COPY [".", "/usr/src/"]
# instalamos dependencias de desarrollo
RUN npm install --only=development

# Pasamos los tests
RUN npm run test
## Esta imagen esta creada solo para pasar los tests.

# Productive image
FROM node:12

COPY ["package.json", "package-lock.json", "/usr/src/"]

WORKDIR /usr/src
# instar las dependencias de PRO
RUN npm install --only=production

# Copiar  el fichero de la imagen anterior.
# De cada stage se reutilizan las capas que son iguales.
COPY --from=builder ["/usr/src/index.js", "/usr/src/"]
# Pone accesible el puerto
EXPOSE 3000

CMD ["node", "index.js"]
### En tiempo de build en caso de que algún paso falle, el build se detendrá por completo.
```

`docker build -t prodapp -f build/production.Dockerfile . `

Al trabajar con el archivo production, tomamos como un script de integracion continua ya que cualquiera de los test falla se va a detener el proceso de build y no creara la imagen. Se optimiza las imagenes productivas en un solo docker file, reusando el cache de las capas para que se construya rapido, se encuentra en la mayoria de las imagenes productivas en proyectos reales. Es un concepto importante.

### Docker-in-Docker

Existe la forma de usar docker dentro de docker, concepto DOCKER IN DOCKER.

`docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock docker`

De esta manera montamos el sock de docker en el contenedor de docker, esto nos permite que dentro de la imagen podamos correr otros contenedores, haciendole una herramienta muy poderosa.
