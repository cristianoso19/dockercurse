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
<img src="https://ualmtorres.github.io/SeminarioDockerPresentacion/images/DockerEngine.png"

* `docker daemon` Corazón de docker o servidor de docker. 
* `REST API` Para comunicarse con el mundo exterior o localmente. Es posible exponer el docker daemon **No es recomedable pero es posible**
* `docker-cli` cliente de docker para terminal, habla con el docker daemon. 
* `contenedores` Lo mas importante, donde van a correr nuestras aplicaciones.
* `imagenes` Artefactos que usa docker para empaquetar contendores, para poder distribuir
* `volumenes de datos` Es la forma en la que docker nos permite acceder de manera segura y flexible a el sistema de archivos de la maquina anfitriona.
* `network` Le permite a los distintos contenedores de docker entre si o con el mundo exterior.
