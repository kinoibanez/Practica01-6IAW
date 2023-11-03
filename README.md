# Practica01-6IAW
Este repositorio es para la Práctica 1 apartado 6 de IAW

# Introducción.

- En esta práctica vamos a realizar la instalación de WordPress en una máquina de AWS con Sistema Operativo *_Ubuntu 22.04_*

## Antes de empezar.

- Tenemos que crear un repositorio en nuestra página de GitHub y una vez hecho eso clonarlo en nuestra máquina.

- Hacemos uso de `sudo apt install git`

- Después `git clone [URL del Repositorio]`

- Y comenzamos creando las carpetas *_scripts_* y *_conf_*.


## Primeros pasos.

- Tendremos que coger el *_script_* de la instalación de la *_pila LAMP_* que hemos utilizado anteriormente y lo lanzamos con *_sudo_*

- `sudo ./install_lamp`


## Creamos el script *_DEPLOY_*

- Vamos a crear un script que nos permite la instalación de WordPress de manera automatica. 

- Primero tenemos que instalar unzip `sudo apt install unzip -y`

- El primer paso que tenemos que seguir es instalar el código fuente que sacamos de la URL de la pagina.

- `wget http://wordpress.org/latest.zip -P /tmp`

- Pero antes de eso, tenemos que eliminar instalaciones previas para así no tener que estar creando archivos cada vez que lancemos el *_script_*

- `rm -rf /tmp/latest.zip`

- Una vez lo tengamos instalado podemos hacer uso de `ls -l /tmp/` para ver que los archivos que tenemos dentro y observar que se ha descargado correctamente el zip.


## Descomprimimos el archivo haciendo uso de ZIP.

- Haciendo uso del comando : `unzip -u /tmp/latest.zip -d /tmp` podremos descomprimirlo. 

### SI USASEMOS TAR  Descomprimimos el archivo.

- Haciendo uso del comando `tar` tendremos que descomprimir el archivo que hemos descargado anteriormente. Esta linea tendremos que añadirla a nuestro script de *_deploy_wordpress_root_directory.sh_* 

- La linea que tenemos que añadir es:

    `tar -xzvf /tmp/latest.tar.gz -C /tmp`

## Parametros que utilizamos ( Si usasemos el comando tar)

    Utilizamos los siguientes parámetros:

    -x: Indica que queremos extraer el contenido del archivo.
    -z: Indica que queremos descomprimir el archivo.
    -v: Habilita el modo verboso para mostrar por pantalla el proceso de descompresión.
    -f: Se utiliza para indicar cuál es el nombre del archivo de entrada.
    -C: Se utiliza para indicar cuál es el diretorio destino.

- Información sacada de la página ![JoseJuan](https://josejuansanchez.org/iaw/practica-01-06/index.html)

- Una vez hecho ya lo tenemos guardado en nuestro directorio *_/tmp/_*

## Es importante eliminar instalaciones previas para no saturarlo.

- 