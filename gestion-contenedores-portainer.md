# Gestion de contenedores. Portainer
## 1. ¿Que es Portainer.io?
Portainer es una herramienta web open-source la cual se ejecuta ella misma como un container, por tanto deberemos tener Docker instalado. Esta aplicación nos va a permitir gestionar de forma muy fácil e intuitiva nuestros contenedores Docker a través de una interfaz gráfica.
¿Por qué usar Portainer.io?
Para las tareas de administración de contenedores Docker, a veces la línea de comandos puede hacerse algo dura para realizar ciertas acciones. A través de una interfaz web, un administrador puede tener una visión global más clara de los contenedores que está ejecutando y facilitar su gestión.
## 2. Instalación
En primer lugar, crearemos un volumen donde almacenar la información. Lo haremos con:

> docker volume create portainer_data

Una vez creado el volumen, procederemos a lanzar el contenedor que tiene todo lo necesario para que funcione.

> docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer

Lo que hace es escuchar en el puerto 9000. Allí es donde lo tendremos arrancado. Por tanto para acceder a él, simplemente debemos ir a http://localhost:9000 en nuestro navegador. portainer/portainer, es el repositorio de DockerHub de donde descargamos la imagen de Portainer.
El primer acceso, nos solicitará que creemos un password para el usuario “admin”, con al menos 8 caracteres de longitud.
La pantalla que observaremos será similar a la siguiente:
![imagen](/imagenes/portainer1.png)

Un vez creado el usuario administrador con una contraseña válida para la política de password, pasaréis a la siguiente ventana donde nos preguntará si queremos levantarlo en local o en remoto, en mi caso he seleccionado en local para así manejar los containers que tengo ejecutando en mi propio ordenador.
Si todo ha ido bien, tendremos Portainer en funcionamiento tal como se ve aquí:
![imagen](/imagenes/portainer2.png)

La primera sección, llamada Dashboard, es la página principal de nuestra instancia de Portainer. En ella se muestra un resumen de manera visual de nuestro sistema de Docker: número total de contenedores, de imágenes, redes o volúmenes.  Esta sección es de gran utilidad para mostrarnos de manera global cual es el estado actual de Docker en nuestra máquina.

![imagen](/imagenes/portainer3.png)

El menú de Containers, nos mostrará la lista de todos nuestros contenedores, y podremos ejecutar a golpe de click varias de las típicas instrucciones que solemos ejecutar a través de la línea de comandos, como arrancarlos, pararlos o eliminarlos.

![imagen](/imagenes/portainer4.png)

Acudiremos  al menú Containers para crear un contenedor con una imagen de las que tenemos en nuetro docker. Prepararemos el contenedor, indicando la información solicitada (nombre del contenedor, red). En “Show advanced options” podemos indicar información tal como mapeo de puertos y volúmenes.
Con todo listo, haremos click en “Deploy the container” para desplegarlo.
![imagen](/imagenes/portainer5.png)

Una vez desplegado, nos redirigirá automáticamente a la pestaña con información de los contenedores.
También podemos ver detalles del propio contenedor. Si hacemos click en el nombre de un contenedor, entonces podemos conocer la información del mismo.

![imagen](/imagenes/portainer6.png)

Esa pantalla llamada Container details nos permite

- Todas las operaciones mas habituales como parar, pausar, matar o borrar el contenedor
- Ver informaciones del contenedor (docker inspect)
- Crear una imagen nueva desde el mismo contenedor y añadirla a un registro (docker commit)
- Ver los logs del contenedor (docker logs)
- Ver las estadísticas del contenedor (docker stats)
- Entrar en el contenedor pudiendo elegir el shell o el usuario (docker exec)
- Conectar/Desconectar el contenedor con una red (docker network connect)
