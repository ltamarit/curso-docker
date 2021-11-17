#Gestion de contenedores. Portainer
## 1. ¿Que es Portainer.io?
Portainer es una herramienta web open-source la cual se ejecuta ella misma como un container, por tanto deberemos tener Docker instalado. Esta aplicación nos va a permitir gestionar de forma muy fácil e intuitiva nuestros contenedores Docker a través de una interfaz gráfica.
¿Por qué usar Portainer.io?
Para las tareas de administración de contenedores Docker, a veces la línea de comandos puede hacerse algo dura para realizar ciertas acciones. A través de una interfaz web, un administrador puede tener una visión global más clara de los contenedores que está ejecutando y facilitar su gestión.
## 2. Instalación
En primer lugar, crearemos un volumen donde almacenar la información. Lo haremos con:
docker volume create portainer_data
Una vez creado el volumen, procederemos a lanzar el contenedor que tiene todo lo necesario para que funcione.
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
Lo que hace es escuchar en el puerto 9000. Allí es donde lo tendremos arrancado. Por tanto para acceder a él, simplemente debemos ir a http://localhost:9000 en nuestro navegador. portainer/portainer, es el repositorio de DockerHub de donde descargamos la imagen de Portainer.
El primer acceso, nos solicitará que creemos un password para el usuario “admin”, con al menos 8 caracteres de longitud.
La pantalla que observaremos será similar a la siguiente:
