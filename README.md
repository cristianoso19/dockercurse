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

```

