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

-  Borraremos instalaciones previas del directorio *_/tmp_* y es muy importante de aqui en adelante tras usar este comando: 



    ``` 
    #Antes de mover el contenido eliminamos instalaciones previas de WordPress en /var/www/html

    rm -rf /var/www/html/*
    ```

- Es *SUPER IMPORTANTE* que este escrito de la siguiente manera, por que si no, borraremos el directorio /html de completamente.

- Por ultimo paso hacemos un *_mv_* para mover el repositorio al directorio que en este caso queremos nosotros.

    ```
   #Movemos el contenido de /tmp/wordpress a /var/html

    mv -f /tmp/wordpress/* /var/www/html

    ```

## Archivo *_.env_* y MySql.

- Tenemos que antes de modificar nuestro script para lanzarlo y generar la base de datos. Tenemos que tener configurado nuestro archivo *_.env_* para poder lanzar el script con las variables necesarias.

- El archivo .env en nuestro caso se vería de la siguiente manera:

    ![](images/cap3.png)

- Una vez configurado todo esto, podemos lanzar el script que queremos ejecutar, recordamos que el `<<<` nos sirve para escribir lineas dentro de MySql.

    ``` 
    # Creamos la base de datos y el usuario de base de datos.

    mysql -u root <<< "DROP DATABASE IF EXISTS $WORDPRESS_DB_NAME"
    mysql -u root <<< "CREATE DATABASE $WORDPRESS_DB_NAME"
    mysql -u root <<< "DROP USER IF EXISTS $WORDPRESS_DB_USER@$IP_CLIENTE_MYSQL"
    mysql -u root <<< "CREATE USER $WORDPRESS_DB_USER@$IP_CLIENTE_MYSQL IDENTIFIED BY '$WORDPRESS_DB_PASSWORD'"
    mysql -u root <<< "GRANT ALL PRIVILEGES ON $WORDPRESS_DB_NAME.* TO $WORDPRESS_DB_USER@$IP_CLIENTE_MYSQL"

    ```
- Si accedemos a MySql por consola, podremos ver que se ha creado de manera correcta:

    ![](images/cap1.png)



## Hacemos uso del comando *_sed_* para el archivo wp_config.