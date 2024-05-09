# Índice de contenido
1. Introducción
2. Instalación de Docker en sistemas Linux (ubuntu)
- 2.1. Instalación desde el repositorio oficial de Ubuntu (No recomendado)
- 2.2. Instalación desde el repositorio de Docker-CE (Recomendado)
    - 2.2.1. Paso 1: Eliminando versiones antiguas de Docker engine 
    - 2.2.2. Paso 2: Incluyendo el repositorio de Docker CE
    - 2.2.3. Paso 3: Instalando Docker engine CE 
- 2.3. Post instalación
    - 2.3.1. Permitir administrar Docker con usuarios sin privilegios
    - 2.3.2. Activar/desactivar arranque al inicio
- 2.4. Desinstalando Docker en Ubuntu
3. Instalación de Docker en sistemas Windows
- 3.1. Pasos previos Windows 10 Pro y Windows Server: activando Hyper-v
- 3.2.Pasos previos Windows 10 Home: Instalando WSL2
- 3.3. Instalación de Docker Desktop
- 3.4.Resolviendo problemas en la instalación de Docker Desktop
4. Instalación de Docker en sistemas MacOS 
5. Playgrounds de Docker 
6. Conclusión 
7. Bibliografía 

## 1. INTRODUCCIÓN

En esta unidad se explican diversos itinerarios para la instalación de Docker, realizando nuestras recomendaciones. Aunque puede realizarse en sistemas Windows y MacOS, recomendamos siempre que sea posible realizar el curso (y en general, usar Docker) en sistemas Linux, ya que su implementación a día de hoy es más robusta y puede evitarnos muchos quebraderos de cabeza. 

## 2. INSTALACIÓN DE DOCKER EN SISTEMAS LINUX (UBUNTU) 

En este apartado vamos a hablar de la instalación de Docker engine CE (Community Edition) en sistemas Linux en distribuciones basadas en **Ubuntu** (Ubuntu, KUbuntu, Linux Mint, etc.). 

El procedimiento para instalar Docker engine CE en otras distribuciones es similar y aquí dejo algunos enlaces con instrucciones para instalarlo en algunas de las más populares: 

- **CentOS**: <https://docs.docker.com/engine/install/centos/> 
- **Debian**: <https://docs.docker.com/engine/install/debian/> 
- **Fedora**: <https://docs.docker.com/engine/install/fedora/> 

Asimismo, también es posible la realización de una instalación desde los binarios, siguiendo las instrucciones de <https://docs.docker.com/engine/install/binaries/> 

### 2.1. Instalación mediante el script de instalación rápida

La forma más sencilla de instalación es mediante el script de instalación rápida descargado de la página oficial. Hace de forma automática todos los pasos que se encuentran en la documentación de instalación, sin importarnos la plataforma ni la distribución de Linux sobre la que ejecutamos el script.

sudo curl –fsSL https://get.docker.com/ | sh

Para comprobar la información de funcionamiento (en algunos casos es necesario reiniciar el sistema):

docker info

### 2.2. Instalación desde el repositorio oficial de Ubuntu 

Es  posible  instalar  Docker  engine  desde  el  repositorio  oficial  de  Ubuntu, aunque instala versiones antiguas. 

### 2.3. Instalación desde el repositorio de Docker-CE (Recomendado) 

A  continuación  detallaremos  los  pasos  para  instalar  Docker  engine  CE  en  Ubuntu  desde  el repositorio oficial de Docker CE. Las versiones de Ubuntu soportadas (todas de 64 bits) son: 

- Ubuntu Groovy 20.10. 
- Ubuntu Focal 20.04 (LTS). 
- Ubuntu Bionic 18.04 (LTS). 
- Ubuntu Xenial 16.04 (LTS). 
#### 2.3.1. Paso 1: Eliminando versiones antiguas de Docker engine 

En primer lugar, deberemos eliminar otras versiones de Docker, por si estuvieran instaladas (por ejemplo, una versión del repositorio oficial de Ubuntu) y pudieran provocar conflictos. 

Para eliminar las versiones antiguas puede bastarnos con la orden: 

sudo apt-get remove docker docker-engine docker.io containerd runc ![](Aspose.Words.8b363e28-3f1b-4535-b085-ac3243b892d1.007.png)

#### 2.3.2. Paso 2: Incluyendo el repositorio de Docker CE 

Para poner en marcha el repositorio, lo primero que haremos será actualizar nuestro índice de paquetes y tras ello, instalar los paquetes necesarios (si no lo estaban ya) para que se puedan utilizar repositorios con HTTPS. 

sudo apt-get update 

sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common 

Una  vez  realizado  este  paso,  descargamos  la  clave  GPG  del  repositorio  de  Docker  CE  y  la incluiremos. Podemos hacer todo con la siguiente línea: 

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - 

Opcionalmente, podemos verificar que la clave está en nuestro sistema con la siguiente orden, buscando la “huella” 0EBFCD88. 

sudo apt-key fingerprint 0EBFCD88 

Si es correcto, verás algo similar a la siguiente captura: 

![imagen](/imagenes/instalacionL1.png)

Ahora,  solo  nos  queda  añadir  el  repositorio  de  Docker  CE  como  fuente  para  instalación  de paquetes. **MUCHO OJO en este paso en distribuciones derivadas, como Linux Mint**. El motivo  es el siguiente. Al configurar la fuente de paquetes indicamos la versión de Ubuntu. El comando que utilizamos para obtener la versión de Ubuntu es el siguiente: 

lsb\_release -cs 

Este comando nos dirá qué distribución tenemos. Por ejemplo, si tenemos “Ubuntu Bionic 18.04 (LTS)”, este comando imprimirá por pantalla “bionic”. 

En algunas versiones derivadas de Ubuntu, como Linux Mint, aunque la distribución esté basada en  Ubuntu  Bionic,  no  devolverá  el  texto “bionic”, sino otro diferente. Si estáis en este caso, deberéis introducir a mano la versión de Ubuntu en que se basa vuestra distribución. 
Aclarado esto, con el siguiente comando podéis añadir el repositorio: 

sudo add-apt-repository "deb [arch=amd64] <https://download.docker.com/linux/ubuntu> $(lsb\_release -cs) stable" 

O  en  el  caso  que  tengáis  una  distribución  basada  en  Ubuntu  con  el  problema  comentado anteriormente, sustituir “*$(lsb\_release -cs)*” a mano por el nombre, de una forma similar a: 

sudo add-apt-repository "deb [arch=amd64] <https://download.docker.com/linux/ubuntu> bionic stable" 

#### 2.3.3. Paso 3: Instalando Docker engine CE 

Por último, ya con el repositorio oficial de Docker en nuestro sistema, solo nos queda actualizar el índice de paquetes e instalar la última versión de Docker engine CE de la siguiente forma: 

sudo apt-get update 

sudo apt-get install docker-ce docker-ce-cli containerd.io 

Finalmente, comprobaremos que Docker engine CE se ha instalado correctamente ejecutando: 

sudo docker version 

y obteniendo un resultado similar al siguiente: 

![imagen](/imagenes/instalacionL2.png)

Para más información sobre este comando podéis visitar <https://docs.docker.com/engine/reference/commandline/version/> 
### 2.4. Post instalación 

En  la  documentación  de Docker, nos proponen algunos pasos de post instalación. Los podéis consultar en <https://docs.docker.com/engine/install/linux-postinstall/> 

En esta sección vamos a comentar dos de ellos: administrar Docker con usuarios sin privilegios (no root ni sudoers) y arrancar Docker desde al inicio. 

#### 2.4.1. Permitir administrar Docker con usuarios sin privilegios 

Docker  utiliza  sockets  Unix.  Para  la creación y reserva de un socket Unix, es necesario tener permisos de root, por lo cual Docker engine necesita permisos de root para ejecutarse. 

A veces, en algunos contextos puede sernos útil que Docker se ejecute por usuarios sin permisos de root. Pongamos un contexto de un aula de ordenadores, que será utilizada por alumnos con los que queremos realizar actividades que utilizan Docker. 

Podemos configurar el aula para que dichos alumnos puedan utilizar Docker sin necesidad de proporcionarles una cuenta con permisos de root. 

Para ello, en primer lugar crearemos un grupo llamado “docker”. 

sudo groupadd docker 

Tras ello añadiremos a los usuarios que queremos que usen Docker sin permisos de root al grupo con un comando similar al siguiente, donde $USER es el nombre de usuario: 

sudo usermod -aG docker $USER 

Para  probar  que  esto  funcione,  será  necesario  que  el/los  usuario  afectados  cierren  sesión completamente (si la tenían abierta) y la vuelvan a abrir, para que se re-evalúe su pertenencia al grupo “docker”. Con esto podrán utilizar comandos Docker sin necesidad de permisos de root. 

**MUCHO OJO** : si lanzaste comandos de Docker usando sudo con uno de estos usuarios antes de esta operación, es posible que al lanzar Docker te aparezca un mensaje similar a este: 

WARNING: Error loading config file: /home/user/.docker/config.json - stat /home/user/.docker/config.json: permission denied 

Para arreglar este error, las opciones son eliminar el directorio “.docker” con  

sudo rm -rf ~/.docker/ 

o cambiar el propietario y permisos del directorio, usando 

sudo chown "$USER":"$USER" /home/"$USER"/.docker -R 

sudo chmod g+rwx "$HOME/.docker" -R 

### 2.4.2. Activar/desactivar arranque al inicio 

Para indicar que el servicio de Docker se inicie al arrancar la máquina, podemos indicarlo mediante los siguientes comandos: 

sudo systemctl enable docker.service sudo systemctl enable containerd.service 

Si lo que queremos es deshabilitar este arranque automático, podemos usar: 

sudo systemctl disable docker.service 

sudo systemctl disable containerd.service 

Para iniciar/parar/reiniciar los servicios manualmente, podemos usar: 

sudo systemctl start/stop/restart docker.service 

sudo systemctl start/stop/restart containerd.service 

### 2.5. Desinstalando Docker en Ubuntu 

Si en algún momento queremos desinstalar Docker en Ubuntu, podemos usar el comando 

sudo apt-get purge docker-ce docker-ce-cli containerd.io 

Este comando eliminará Docker engine, pero no eliminará contenedores e imágenes presentes en el sistema. Si queremos eliminar estos datos, tenemos dos opciones. 

La primera consiste en, previamente a la eliminación de Docker engine, ejecutar el comando: sudo docker system prune -a 

La segunda opción, se debe realizar tras eliminar Docker y consiste en el borrado manual de las mismas, con el comando: 

sudo rm -rf /var/lib/docker 

## 3. INSTALACIÓN DE DOCKER EN SISTEMAS WINDOWS

En este apartado veremos cómo instalar Docker  Desktop en sistemas Windows. Docker posee dos guías diferenciadas de instalación en sistemas Windows: 

- Guía para Windows 10 Pro y Windows Server 
  - <https://docs.docker.com/docker-for-windows/install/> 
- Guía para Windows 10 Home 
- <https://docs.docker.com/docker-for-windows/install-windows-home/> 
La  principal  diferencia  entre  ellas,  es  que  el  primer  grupo  requiere  que  se  activen  las características de Windows para Hyper-V, mientras que la segunda guía requiere la activación de WSL2 (Windows Subsystem for Linux 2). 

### 3.1. Pasos previos Windows 10 Pro y Windows Server: activando Hyper-v 

En este enlace se explica cómo habilitar Hyper-V en: 

- Windows 10 Pro 
  - [https://docs.microsoft.com/es-es/virtualization/hyper-v-on-windows/quick-start/e nable-hyper-v](https://docs.microsoft.com/es-es/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) 
- Windows Server 
- [https://docs.microsoft.com/es-es/windows-server/virtualization/hyper-v/get-starte d/install-the-hyper-v-role-on-windows-server](https://docs.microsoft.com/es-es/windows-server/virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server) 
### 3.2. Pasos previos Windows 10 Home:  Instalando WSL2 

Antes  de  comenzar,  nuestro  sistema  debe  tener  instalado  WSL2.  La  guía  para  instalarlo  se encuentra aquí <https://docs.microsoft.com/en-us/windows/wsl/install-win10> 

A continuación un resumen de los pasos a realizar.  

En  primer  lugar,  utilizando una consola powershell **con permisos de administrador**, debemos habilitar WSL. Lo podemos hacer con el comando. 

dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart 

Una vez habilitado, debemos asegurarnos que tenemos nuestro Windows 10 Home actualizado, al menos hasta la versión “Versión 1903, Build 18362”. Puedes comprobar tu versión ejecutando el comando “winver”. 

Una  vez  actualizado  y  reiniciado  el  sistema,  debemos  habilitar  la  característica  de  “Virtual Machine”, lo cual podemos hacerlo con una consola powershell **con permisos de administrador**: 

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart 

Tras ello, **deberás reiniciar tu máquina antes del próximo paso.** 

Una vez completado el reinicio, deberás instalar la última versión del Kernel de Linux para WSL2 aquí <https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi> 

Finalizada la instalación, con una powershell **con permisos de administrador,** deberás establecer WSL2 como la versión por defecto al instalar una distribución de Linux. 

wsl --set-default-version 2 

Con esto ya tienes listo WSL2 y una versión de Ubuntu instalada en tu sistema Windows. 

**OPCIONAL:** una vez instalado WSL2, puedes instalar distribuciones de Linux mediante la tienda Microsoft  Store  <https://aka.ms/wslstore>  (por  ejemplo  Ubuntu  20).  **Una  vez  instalada,  es obligatorio que la inicies al menos una vez** para que descomprima parte del sistema y después te pida establecer un usuario administrador de la distribución Linux virtualizada. ![](Aspose.Words.8b363e28-3f1b-4535-b085-ac3243b892d1.007.png)

### 3.3. Instalación de Docker Desktop 

La instalación de Docker Desktop, que es la versión de Docker CE para sistemas Windows, es sencilla  y  básicamente  consiste  en  descargar el instalador desde Docker Hub, en el siguiente enlace  <https://hub.docker.com/editions/community/docker-ce-desktop-windows/>  y  seguir  las instrucciones de instalación en pantalla. 

En  este  enlace  se  puede  ver  un  video  en  el  que  se  realiza  la  instalación <https://www.youtube.com/watch?v=_9AWYlt86B8> 

Finalmente, con **Docker Desktop instalado y lanzado**, comprobaremos que Docker engine CE se ha instalado correctamente ejecutando: 

docker version 

obteniendo un resultado similar al siguiente: 

![imagen](/imagenes/instalacion3.jpeg)

Para más información sobre este comando podéis visitar <https://docs.docker.com/engine/reference/commandline/version/> 

### 3.4. Resolviendo problemas en la instalación de Docker Desktop 

En  mi experiencia con Docker Desktop, he sufrido problemas, incluso simplemente instalando actualizaciones. Hay cantidad de bugs típicos como los que os enlazo aquí: 

- <https://github.com/docker/for-win/issues/7629> 
- [https://forums.docker.com/t/docker-starts-but-trying-to-do-anything-results-in-error-duri ng-connect/49007/4](https://forums.docker.com/t/docker-starts-but-trying-to-do-anything-results-in-error-during-connect/49007/4) ![](Aspose.Words.8b363e28-3f1b-4535-b085-ac3243b892d1.007.png)

**Este es el motivo de que, si os es posible, os recomiendo utilizar Docker en un sistema Linux, donde su implementación es más robusta.** 

En cualquier caso, en estos problemas de instalación en sistemas Windows, una solución que siempre me ha funcionado es hacer un “borrado completo” de Docker Desktop. Las instrucciones son las siguientes: 

- Desinstala Docker Desktop. 
- Ve a tu directorio de usuario (algo como C:\Users\tuUsuario) y elimina todo el contenido de la carpeta “.docker”, si es que está. 
- Elimina  todas  las  “variables  de  entorno”  del  sistema  relacionadas  con  Docker. Suelen empezar  con  “DOCKER\_”.  Algunas  de  ellas  pueden  ser DOCKER\_TLS\_VERIFY, DOCKER\_CERT\_PATH o DOCKER\_HOST. 
  - Aquí  puedes  ver como eliminar  las  variables  de  entrono  de Windows 10 [https://answers.microsoft.com/es-es/windows/forum/windows_10-other_settings/ windows-10-variables-de-entorno-windows-10-version/703ea5fa-1db4-46da-8ff7-6 261140bf58b](https://answers.microsoft.com/es-es/windows/forum/windows_10-other_settings/windows-10-variables-de-entorno-windows-10-version/703ea5fa-1db4-46da-8ff7-6261140bf58b) 
- Una vez hecho esto, reinicia el sistema. 
- Una vez reiniciado, instala de nuevo “Docker Desktop”. 
## 4. INSTALACIÓN DE DOCKER EN SISTEMAS MACOS 

Las  instrucciones  para  la  instalación  de  Docker  Desktop  en  MacOS  están  descritas  en <https://docs.docker.com/docker-for-mac/install/>. 

Para  realizar  esta  instalación,  básicamente  debe  descargarse  el  paquete  “.dmg”  de <https://hub.docker.com/editions/community/docker-ce-desktop-mac/>  y  seguir  las  instrucciones de instalación en pantalla. 

## 5. PLAYGROUNDS DE DOCKER

En  Internet  existen  varios  sitios  que  nos  permiten utilizar un “playground” de  distintas herramientas, para que juguemos con ellas online, sin necesidad de instalar ni configurar nada (y sin el riesgo de romper cosas). 

El sitio web Katacoda <https://katacoda.com/> posee un “playground” de Docker disponible aquí: <https://www.katacoda.com/courses/docker/playground> 

Os animo a utilizarlo si queréis hacer alguna prueba, trastear en un lugar donde no tenéis Docker instalado o para enseñar Docker en contextos donde no se pueda instalar de forma nativa. 

## 6. CONCLUSIÓN

En esta unidad hemos visto los pasos básicos para instalar Docker en distintos sistemas operativos. No  obstante,  continuamos  con  nuestra  recomendación  de  que  si  es  posible,  para  minimizar problemas utilicemos Docker en sistemas Linux. 

## 7. BIBLIOGRAFÍA

[1] Docker Docs <https://docs.docker.com/>  

![imagen licencia](/imagenes/licencia.png)

Sergi García Barea es el autor de la primera versión de estos apuntes.
Reconocimiento – NoComercial - CompartirIgual (BY-NC-SA): No se permite un uso comercial de la obra original ni de las posibles obras derivadas, la distribución de las cuales se debe hacer con una licencia igual a la que regula la obra original.

