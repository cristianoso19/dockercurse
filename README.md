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
### El sistema de capas

