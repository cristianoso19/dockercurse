# dockercurse
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
`docker info`: Visualizar información de docker`
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
Es mas practico, no podemos acceder a esos archivos ya que cuando los datos estan en un volumen son accesibles para el contenedor asignado, no para un usuario nomral directamente.
```bash
#Listo todos los volumenes creados
docker volume ls 
#Creamos un nuevo volumen
docker volume create dbdata
#Desplegamos el contenedor, con --mount usara el volumen creado, se le debe especificar el destiono
docker run -d --name db --mount src=dbdata,dst=/data/db mongo 
#Podemos revisar toda la información con la que se creo el contenedor
docker inspect db
mongo 

```
Es una manera muy practica de compartir archivos entre contenedores sin compartir un directorio, es mas seguro.
### Insertar y extraer archivos de un contenedor.
Esta es otra forma de acceder a archivos del contenedor, indepeniente de que se use BIND MOUNT, volumenes o nada podamos ingresar o extraer datos de un contenedor
> Esta operacion no importa si tiene BIND MOUNT O VOLUMENES
<img src="https://i1.wp.com/cdn-images-1.medium.com/max/800/1*bo6IOrBjaHbtkPgTKT08NA.png?w=1170&ssl=1">
`TMPFS Mount: Guarda los archivos temporalmente y persiste los datos en la memoria del contenedor, cuando muera sus datos mueren con el contenedor.`
```bash
#Crear un archivo de prueba
touch prueba.txt 
#Desplegamos ubuntu con el nombre copytest
$ docker run -d --name copytest ubuntu tail -f /dev/null 
#Ingresamos al contenedor
$ docker exec -it copytest bash 
#Creamos un contenedor dentro del contenedor
$ mkdir /testing 
#SUBIR un archivo al contenedor
$ docker cp prueba.txt copytest:/testing/test.txt 
#DESCARGAR un archivo del contenedor
$ docker cp copytest:/testing localtesting 
```
> No hace falta que el contenedor este **CORRIENDO** para poder ingresar o sacar datos

## Imágenes

### Conceptos fundamentes de imagenes docker
Es la solución de docker a los problemas de construcción y distribución de software, son plantillas a partir de las cuales se puede crear contenedores.
Es una pieza de software empaquetada de forma liviana que contiene todo lo necesario para que un contenedor pueda ejecutarse exitosamente, como dependencias, codigo, etc.
> `docker image ls` la columna "TAG" es la version, en este caso la ultima.
> Una imagen vive como si fuera un archivos o capas.
> Docker se descarga las imagenes de https://hub.docker.com
```bash
#Revisar las imagenes disponibles localmente (instaladas)
$ docker image ls
#Instalar una versión especifica.
$ docker pull ubuntu:20.04
#Eliminar una imagen
$ docker image rm -f {id_image}
```

### Construyendo una imagen propia
Para esto usamos **DockerFile**, es un archivo que describe lo que pasara cuando se cree la imagen, con el comando **Build**.
Una imagen es la plantilla para correr un contendor.
> Desde una imagen podremos crear infintos contenedores
* FROM: Siempre vamos a partir de algo mas.
> Todo lo que se ejecuta en el docker file ocurre en el build.
> Una imagen es un conjunto de **LAYERS** o capas, como un arbol de dependencias.
```dockerfile
#FROM: Empezar desde alguna imagen.
FROM ubuntu:latest
#RUN: corre en el build.
RUN touch /usr/src/hola-platzi.txt
```

```bash
#Crear una imagen propia
docker build -t <base_img>:<tag_image_name> <Dockerfile context>
docker build -t ubuntu:my_image .
#Correr un contendor
docker run -it ubuntu:my_image .
#Iniciar sesion en docker hub
docker login -u <username> -p 
#Verificar el logeo
cat ~/.docker/config.json
#Retag para subir al repositorio
docker tag ubuntu:my_image <username>/ubuntu:my_image
#Subir la imagen
docker push <username>/<image>:<tag>
```
### El sistema de capas
Como ya sabemos una imagen es un conjunto de capas ordenadas.
Es muy facil a partir de su dockerfile.
> Las imagenes publicas tienen su dockerfile publico.
La herramienta dive nos sirve para revisar cada capa de una imagen
https://github.com/wagoodman/dive
> tambien se puede correr en **Docker** con:. `docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock wagoodman/dive:latest $IMAGE`
La importancia de entender el sistema de capas consiste en la optimización de la construcción del contenedor para reducir espacio ya que cada comando en el dockerfile crea una capa extra de código en la imagen.

Agregando Capas

Con docker commit se crea una nueva imagen con una capa adicional que modifica la capa base.

Ejemplo: crear una nueva imagen a partir de la imagen de Ubuntu.

docker pull ubuntu

docker images

docker run -it cf0f3ca922e0 bin/bash

(modificar el contenedor: Ej apt-get install nmap)

docker commit deddd39fa163 ubuntu-nmap
```bash
$ docker history ubuntu:platzi (veo la info de como se construyó cada capa)
$ dive ubuntu:platzi (veo la info de la imagen con el programa dive)
```

