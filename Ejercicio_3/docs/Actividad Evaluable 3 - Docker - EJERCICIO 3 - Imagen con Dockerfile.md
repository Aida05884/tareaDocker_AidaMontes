# Actividad Evaluable 3 - Docker - EJERCICIO 3 - Imagen con Dockerfile

> Tarea creada por Aida Montes Cabello para el módulo Despliegue de Aplicaciones Web.

[TOC]

## Introducción

En este ejercicio lo que haremos será crear una imagen personalizada llamada `ejercicio3` con Dockerfile basada en `php:7.4-apache` y que será accesible desde el navegador en el puerto 8000. Esta imagen contendrá lo siguiente:

- Un sitio web en el que figurará nuestro nombre y tendrá dos archivos, un archivo `index.html` sencillo y un archivo `estilos.css`.
- Un script llamado `fecha.php`.

Posteriormente se subirá la imagen a nuestra cuenta de Docker Hub, la borraremos de nuestro Docker local, la volveremos a bajar y ejecutaremos un contenedor usando la imagen.

## **1.** Creación estructura de archivos

Creamos un directorio llamado `Ejercicio_3` y, dentro de este, otro con el nombre `miSitioWeb`. En su interior, creamos los archivos `index.html` (con nuestro nombre y un enlace), `estilos.css` (para el diseño visual) y `fecha.php` (script).

### Archivos `index.html`, `estilos.css` y `fecha.php`



![image-20250417143656707](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417143656707.png)

![image-20250417135858590](C:/Users/Usuario/AppData/Roaming/Typora/typora-user-images/image-20250417135858590.png)

### Creación del archivo `Dockerfile`

Este archivo le indicará a Docker cómo debe construir la imagen personalizada, es importante que el archivo no tenga extensión y se llame `Dockerfile`. El archivo debe contener lo siguiente:

```
FROM php:7.4-apache 			
COPY .miSitioWeb /var/www/html			
EXPOSE 80
```

- Imagen base usando PHP con  Apache (`php:7.4-apache`)
- Se copian al directorio raíz del servidor todos los archivos del proyecto del directorio `miSitioWeb`.
- Por defecto en Apache, se expone el puerto 80.

![image-20250417142827583](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417142827583.png)

## **2.** Construcción de la imagen

Para la creación de la imagen personalizada, abrimos la terminal integrada en Docker Desktop y nos situamos en la ruta donde está la carpeta `miSitioWeb`.

![image-20250417143802783](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417143802783.png)

Una vez en la ruta, ejecutaremos el siguiente código para la construcción de la imagen:

```
docker build -t aidamontes/ejercicio3:v1 .
```

-  `docker build`: este comando inicia la construcción de la imagen.
- `-t`: da un nombre y etiqueta a la imagen.
- `aidamontes/ejercicio3:v1`: nombre completo de la imagen. `aidamontes` es el nombre de usuario y lo utilizaremos para después poder subirla, `ejercicio3` es el nombre descriptivo de la imagen y `v1` sería la versión de la imagen.

![image-20250417154808928](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417154808928.png)

![image-20250417155210803](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417155210803.png)

![image-20250417155055885](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417155055885.png)

![image-20250417155411837](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417155411837.png)

Comprobamos que la imagen se ha creado correctamente utilizando el código expuesto a continuación, el cual muestra las todas las imágenes de Docker que se encuentran almacenadas de manera local en nuestra máquina.

```
docker images
```

![image-20250417155831307](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417155831307.png)

## **3.** Ejecución del contenedor

Lo primero que tenemos que hacer es crear el contenedor, para ello ejecutamos:

```
docker run -d -p 8080:80 --name ejercicio3 aidamontes/ejercicio3:v1
```

- `-d`: ejecuta el contenedor en segundo plano.
- `-p 8080:80`: redirige el puerto 80 del contenedor al 8080 de nuestra máquina, más adelante, accederemos desde el navegador con `localhost:8080`.
- `--name ejercicio3`: nombre que se le asigna al contenedor.
- `aidamontes/ejercicio3:v1`: imagen anteriormente creada.

## **4.** Verificación. Ver el sitio web en el navegador

Para verificar que los pasos anteriores se han hecho de manera satisfactoria, vamos a acceder a `http://localhost:8080` desde nuestro navegador.

![image-20250417162304084](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417162304084.png)

![image-20250417162414783](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417162414783.png)

## **5.** Subida de la imagen a Docker Hub

Lo primero que tenemos que hacer para subir la imagen es iniciar sesión en Docker Hub desde la terminal.

```
docker login
```

![image-20250417163435375](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417163435375.png)

Vemos que el inicio de sesión ha sido exitoso, que se ha conectado correctamente a Docker Hub.

Ahora nos tocaría subir la imagen a Docker Hub haciendo un `push`.

```
docker push aidamontes/ejercicio3:v1
```

![image-20250417165207020](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417165207020.png)

En este caso en la imagen adjuntada, podemos ver que nos dice que ya existe porque ejecutamos el código, pero se nos cerró la terminal y no pudimos hacer captura de pantalla. Al ejecutarlo de nuevo, nos pone `Layer already exists`/`Already exists`, en un primer momento, cuando lo ejecutamos la primera vez nos traía `Pushed` .

Veamos si se ha subido correctamente, para ello iremos a `Repositories` de nuestro Docker Hub y pulsaremos sobre `aidamontes/ejercicio3`. Una vez dentro, iremos a la pestaña `Tags` y ahí podremos observar la etiqueta que hemos subido (`v1`), información sobre el tamaño y fecha en la que se subió entre otras cosas.

![image-20250417170137509](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417170137509.png)

## **6.** Borrado de la imagen del Docker local

Para eliminar la imagen local, iremos a la terminal y ejecutaremos:

`docker rmi aidamontes/ejercicio3:v1`

![image-20250417173006383](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417173006383.png)

Comprobamos que se ha eliminado correctamente:

```
docker images
```

![image-20250417173110876](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417173110876.png)

## **7.** Operación `Pull` de la imagen de Docker Hub

El comando Pull traerá la imagen a nuestro entorno local desde Docker Hub, para ello ejecutamos:

```
docker pull aidamontes/ejercicio3:v1
```

![image-20250417173538429](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417173538429.png)

## **8.** Creación y ejecución nuevo contenedor

En este apartado generaremos un contenedor a partir de la imagen creada y lo lanzaremos especificando un puerto diferente. Por lo tanto mapearemos el puerto 80 interno del contenedor al puerto 1234 del host, para poder acceder más adelante desde el navegador con `http://localhost:1234`. Ejecutamos para ello el siguiente comando:

```
docker run -d --name ejercicio3_v2 -p 1234:80 aidamontes/ejercicio3:v1
```

![image-20250417174331528](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417174331528.png)

Por último vamos a comprobar que accediendo a `http://localhost:1234` vemos nuestra página correctamente.

![image-20250417174534683](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417174534683.png)

![image-20250417174623906](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%203%20-%20Imagen%20con%20Dockerfile.assets/image-20250417174623906.png)

