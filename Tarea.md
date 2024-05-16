## Tarea Instalación Docker. Imágenes. Creación de contenedores

El objetivo de este tarea es que comencéis a manejar Docker y os familiaricéis con el uso de contenedores.

### Instalación de Docker en Linux
Docker funciona en la mayoría de las distribuciones Linux. Dependiendo de la distribución los pasos a seguir para la instalación de los paquetes son distintos. En todos los casos habrá que actualizar los repositorios, instalar los certificados de confianza de Docker, añadir el repositorio oficial, actualizarlo y por fin instalar el paquete.
Podéis hacer la instalación:

- Siguiendo las instrucciones del video
- Siguiendo las instrucciones de la documentación (Instalación Docker. Enlace git. Punto 2.2) esta es la opción que os recomiendo porque es la última que he utilizado yo.
- Ejecutando la instalación automática que se encuentra en https://get.docker.com y que ejecuta todos esos comandos por nosotros.

Para ello ejecutamos el siguiente comando:

> curl –fsSL https://get.docker.com/ | sh

### Primeros pasos con Docker

Una vez instalado docker en nuestro equipo ya estamos en condiciones de ejecutar la primera acción.
Para trabajar con docker vamos a abrir el Shell de comandos y a cada instrucción de docker le antepondremos el comando docker. Para poder ejecutar las instrucciones de Docker necesitamos tener privilegios de administrador, por ello también antepondremos el comando sudo a cada comando de docker.
Si deseamos ejecutar Docker con un usuario sin privilegios debemos añadir dicho usuario al grupo docker. Por ejemplo si queremos usar el usuario **ser** lo hacemos con el lanzamiento del siguiente comando:

> usermod -aG docker ser

Una vez añadido, reiniciamos el demonio de Docker con

> /etc/init.d/docker restart

para que los cambios surtan efecto en nuestro sistema, o bien cerramos sesión y volvemos a entrar
Para ver que está todo funcionando podemos empezar por comprobar la información del servidor docker mediante la acción info:

> docker info

La salida del comando nos muestra, entre otras cosas, la versión del servidor.

![imagen](/imagenes/tareaContenedores1.png)

Podemos visualizar la lista de los comandos de docker mediante la ayuda:

> docker help

Para trabajar con contenedores es necesario conocer el concepto de imágenes. Un contenedor depende de una imagen para funcionar. Cuando se crea un contenedor se basa en el contenido de una imagen, como si fuera una plantilla base. Los cambios y tareas que se realicen en el contenedor solo se verán reflejados en él mismo, y no en la imagen original.

Varios contenedores ejecutándose simultáneamente pueden estar basados en la misma imagen. Un ejemplo podría ser una imagen de Debian. Podríamos crear un contenedor sobre esa imagen para poder virtualizar ese sistema operativo y probarlo y añadir servicios. Podríamos crear otro contenedor distinto sobre esa misma imagen de manera que tendríamos otro Debian nuevo sobre el que poder añadir programas y servicios distintos.

Podríamos incluso descargar una imagen en la que ya está instalado, configurado y listo para utilizar un servidor con un Apache, o un Moodle o cualquier otro programa o servicio que se nos ocurra sin importarnos siquiera sobre qué sistema operativo corre.
Existen multitud de imágenes listas para ser descargadas en el Hub de docker
(https://hub.docker.com/). Este es un repositorio donde es posible buscar imágenes, ver cómo utilizarlas y obtener información de ellas. Podemos acceder a este repositorio desde la web y copiar el comando necesario para descargar la imagen o directamente buscar la imagen y descargarla desde el Shell de docker.

En primer lugar podemos comprobar qué imágenes tenemos descargadas:

> docker images

Obviamente si acabamos de instalar docker no nos aparecerá ninguna imagen descargada en nuestro equipo.
Es interesante comprobar la información que nos muestra, como el ID de imagen y el tamaño de la misma.
Para buscar imágenes del repositorio oficial lo podemos hacer de 2 maneras. La primera consiste en acceder a la web del Hub de Docker (https://hub.docker.com/) y ahí buscar la imagen deseada. Nos dirá el nombre de la misma y el comando (pull) a ejecutar para la descarga.
La otra opción es la de buscarla directamente desde la línea de mandatos mediante el comando search. En esta tarea vamos a buscar las imágenes que aparezcan con el nombre ubuntu:

> docker search --limit 5 ubuntu

Le estamos indicando que sólo nos muestre los 5 primeros resultados de la búsqueda.

![imagen](/imagenes/tareaContenedores2.png)

Una vez localizada la imagen que deseamos, sólo falta descargarla mediante el comando pull. En nuestro csaso el nombre es ubuntu:

> docker pull ubuntu

Comprobamos que la tenemos descargada:

> docker images

![imagen](/imagenes/tareaContenedores3.png)

### Contenedores en Docker

Una vez descargada la imagen es hora de crear nuestro contenedor.
Para crear un contenedor tenemos 2 comandos. El comando create crea un contenedor a partir de una imagen pero no lo ejecuta. El comando run crea y ejecuta un contenedor.
La sintaxis del comando create es la siguiente:

> docker create –[opciones] imagen [comando]

En caso de que la imagen no estuviera descargada, el comando create la descargará. Al contenedor creado se le asignará un identificador de 64 caracteres (se suelen mostrar 12) que nos mostrará por pantalla y que será necesario para futuras operaciones que hagamos con él.
Como ejemplo:

> docker run –ti debian cat/etc/debian_version

Este comando creará un contenedor y lo ejecutará a partir de una imagen llamada debian. Si la imagen no está descargada, automáticamente docker la descargará del repositorio.
Una vez creado el contenedor se ejecutará dentro del mismo la orden cat/etc/debian_version. Cuando este comando termina su ejecución, visualiza la versión de la distribución y el contenedor se detiene. Las opciones -ti indican que se ha de iniciar el contenedor con la posibilidad de acceder al terminal (-t) y que se ha de iniciar el contenedor en modo interactivo (-i).
En nuestro caso vamos a crear un contenedor

> docker run -d ubuntu /bin/echo “hola mundo”

Con la opción –d ejecutamos el contenedor en segundo plano nos ocurrirá lo mismo que en el video, como ya está la imagen de ubuntu bajada directamente ejecuta el hola mundo.

Como veis podíamos habernos saltado el paso de hacer el pull de la imagen ubuntu, pero quería que conocierais los comandos para bajarte una imagen y visualizarla

Con la opción -–name asignamos un nombre al contenedor. No es obligatorio, en caso de no especificarlo se le asigna un nombre autogenerado.

> docker run -d -–name ubuntuLourdes ubuntu /bin/echo “hola otra vez”

Al ejecutar otra vez el run con nombre me parecen dos contenedores, si ejecutamos:
docker ps -a (la imagen inferior está cortada por la izda para que aparezcan los nombres a la derecha claramente) se ve un contenedor creado a partir de la imagen ubuntu de nombre ubuntuLourdes y otro creado a partir de la imagen ubuntu y el nombre que le ha asignado docker vibrant_ishizaka

![imagen](/imagenes/tareaContenedores4.png)

Con estas opciones arrancamos el contenedor pero se parará automáticamente luego de ejecutar el “echo”. Ahora haremos lo mismo con la opción -ti para que el contenedor se quede corriendo

> docker run -d -ti --name ubuLourdes ubuntu

si te fijas en la imagen inferior veras que hago un docker ps para ver los contenedores que tengo activos y sólo me parece corriendo este último

![imagen](/imagenes/tareaContenedores5.png)

### Ejecución de contenedores

Una vez creado el contenedor hemos de ponerlo en ejecución. Como lo hemos creado con el comando run, el mismo mandato además de crearlo puede dejarlo en ejecución
Si hubiéramos utilizado create habríamos de iniciarlo mediante el comando start.
Para iniciar el contenedor es necesario indicarle o bien el nombre que le hemos asignado o bien el ID que nos mostró al crearlo. Si optamos por especificar el identificador es suficiente con indicarle los cuatro primeros caracteres y no los 12.

> docker start 4488

que es lo mismo que docker start ubuntuLourdes

Los contenedores se crean en una red por defecto 172.17.0.0/16 asignándoles una IP automática en ese rango. Como no la sabemos podemos inspeccionar el contenedor y ver qué IP privada le ha asignado de manera automática Docker:

> docker inspect ubuntuLourdes

Para ver solo la información que nos interesa (en Linux):

> docker inspect ubuntuLourdes | grep ‘"IPAddress"'

Un mandato interesante de docker es el que nos da la posibilidad de ejecutar un comando dentro del sistema operativo que está corriendo en el contenedor que se está ejecutando.
En nuestro ejemplo vamos a ver la versión de Linux (uname -a) que corre bajo nuestro ubuntuLourdes con el comando exec:

> docker exec ubuntuLourdes uname -a

Algunos comandos: 

> contenedores que tenemos creados docker ps -a
> contenedores que están en ejecución docker ps
> detener contenedores en ejecución docker stop ubuntuLourdes
> Iniciar contenedores docker start ubuntuLourdes
> Ejecutar comando docker exec ubuntuLourdes comando
> abrir una terminal en contenedor docker exec -ti ubuntuLourdes /bin/bash


Existe una herramienta interesante que nos ayudará a controlar el espacio de disco que está siendo utilizado por los elementos de Docker. Es el comando siguiente:

 > docker system df

![imagen](/imagenes/tareaContenedores6.png)

Muestra el espacio usado por Docker, detallando cuánto es ocupado por las imágenes de Docker, cuánto por los contenedores y por los volúmenes. Incluye el número total de elementos y los activos actualmente.
En la imagen se ve que tengo dos contenedores y dos imagenes

### Volver a arrancar el servicio.

Cuando apaguemos el equipo el servicio se parará y no se arranca de forma automática deberemos arrancar los contenedores para poder trabajar con ellos:
Eliminación de contenedores e imágenes
Para eliminar un contenedor lo primero que tengo que hacer es pararlo

> docker stop ubuntuLourdes

Y para borrarlo utilizo el comando:

> docker rm ubuntuLourdes

Si visualizamos los contenedores veremos que ya no está el nuestro:

> docker ps -a

Con el comando rm hemos eliminado el contenedor pero no la imagen sobre la que se creó. Para eliminar la imagen de nuestro disco utilizamos el mandato rmi:

> docker rmi ubuntu

Comprobamos que ha desaparecido la imagen con:

> docker images

Limpieza de elementos en Docker
Además del borrado manual de elementos, existe otra opción que es limpiar Docker con el comando prune. El comando prune se aplica a contenedores, volúmenes, redes o imágenes y sirve para eliminar aquellos elementos que no están en uso. Por ejemplo, para eliminar las imágenes que no estan siendo utilizadas por ningún contenedor activo ejecutamos:

> docker image prune -a

Para eliminar todos los elementos que no estén en uso utilizamos el siguiente comando:

> docker system prune

Este unifica todas las opciones prune de contenedores, volúmenes, redes e imágenes, borrando todas aquellas que no estén en uso.
