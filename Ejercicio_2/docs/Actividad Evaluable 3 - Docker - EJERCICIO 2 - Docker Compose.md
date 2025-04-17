# Actividad Evaluable 3 - Docker - EJERCICIO 2 - Docker Compose

> Tarea creada por Aida Montes Cabello para el módulo Despliegue de Aplicaciones Web.

[TOC]

## Introducción

En este ejercicio utilizaremos `Docker Compose` a través de `Docker Desktop` para desplegar la aplicación`FileBrowser` cuya imagen está disponible en el repositorio oficial de Docker Hub: https://hub.docker.com/r/hurlenko/filebrowser. El despliegue se hará con la creación de un archivo `compose.yaml`, en el cual se definirán los servicios, puertos y volúmenes necesarios para el correcto funcionamiento de la aplicación.



## **1.** Archivo `compose.yaml`

En este apartado, mostramos y explicamos el contenido del archivo `compose.yaml` que vamos a utilizar en este ejercicio.

```
services:
	filebrowser:
		image: hurlenko/filebrowser
		container_name: filebrowser
		ports:
			- "8080:8080"
		volumes:
			- ./data:/data
			- ./config:/config
		restart: unless-stopped
```

En este archivo se define un servicio que se llama `filebrowser` utilizando la imagen oficial `hurlenko/filebrowser`. A continuación, se explican los elementos de este archivo:

- `sercives:`: servicio que se desplegará: `filebrowser.`

- `image:` : imagen de Docker que se va a utilizar.

- `container_name:` : nombre del contenedor.

- `ports:` : abre el puerto 8080 del contenedor para poder acceder a la aplicación desde el navegador (`localhost:8080`).

- `volumes:` :

  - `./data:/data`: carpeta local (data) donde se almacenarán los archivos que se suban desde `FileBrowser`.
  - `./config:/config`: guarda la configuración de la aplicación.

- `restart: unless-stopped`: el contenedor se reinicia automáticamente.

  

## **2.** Cómo cargar el archivo YAML en Docker Desktop

Para cargar el archivo que hemos creado en el punto anterior, abriremos la terminal integrada en `Docker Desktop` pulsando en `>_ Terminal` que se encuentra abajo en la parte derecha.

![image-20250417002203420](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417002203420.png)

Una vez abierta, nos tenemos que situar en la carpeta donde está el archivo:

![image-20250417004741469](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417004741469.png)

Tras acceder a la carpeta, ejecutamos el siguiente comando:

```
docker-compose up -d
```

![image-20250417111629334](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417111629334.png)

Comprobamos que se ha añadido como contenedor en la pestaña `Containers`.

![image-20250417111753370](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417111753370.png)

También vamos a verificar que se han creado las carpetas locales (`./data`, `./config`) ya que hemos utilizado bind mounts. Para ello accedemos a la carpeta donde tenemos el `.yaml`.

![image-20250417112448605](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417112448605.png)



## Funcionamiento FileBrowser

FileBrowser es una aplicación web ligera la cual ofrece una interfaz gráfica para gestionar archivos y carpetas dentro del volumen `/data`. Permite subir archivos desde nuestro equipo local, crear o editarlos directamente desde la interfaz, descargar contenido, organizar carpetas y, en versiones avanzadas, configurar usuarios, roles y permisos. En resumen, facilita la gestión de archivos almacenados en un directorio del sistema de archivos del servidor, en nuestro caso, dentro del contenedor Docker.

En el punto anterior, hemos desplegado el contenedor, lo siguiente que tendremos que hacer es acceder a la aplicación desde el navegador mediante la dirección`http://localhost:8080`. Para el inicio de sesión, se utilizarán las credenciales predeterminadas: usuario `admin`y contraseña `admin`.

### **a)** Acceso a FileBrowser

Como bien hemos comentado, para entrar en la interfaz gráfica de la aplicación, vamos a nuestro navegador e ingresamos la dirección `http://localhost:8080` e introducimos el usuario y la contraseña.

![image-20250417115429424](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417115429424.png)

### **b)** Navegación por FileBrowser

Una vez dentro de la aplicación lo primero que vamos a hacer es cambiar el idioma a español, para ello iremos al icono de `Settings` que se encuentra en el menú izquierdo. 

![image-20250417120739567](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417120739567.png)



Después de cambiar el idioma vamos a realizar alguna tarea, como por ejemplo la creación de una carpeta y la subida de un archivo. Para la creación de una carpeta lo que tendremos es que acceder a la pestaña `Nueva carpeta` situada en el menú izquierdo.

![image-20250417121049566](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417121049566.png)

![image-20250417121210167](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417121210167.png)

A continuación vamos a subir el archivo PDF de la tarea dentro de la carpeta que hemos creado previamente, para ello, accedemos a dicha carpeta haciendo doble clic sobre ella.

Una vez dentro, pulsamos sobre el icono de flecha hacia arriba, que se encuentra en la parte superior derecha. Seleccionamos `Archivo` y buscamos, seleccionamos el PDF de la tarea (`Actividad Evaluable 3 - Docker`) y lo subimos.

![image-20250417122158104](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417122158104.png)

Ahora nos aparecerá el archivo dentro de la carpeta que hemos creado.

![image-20250417122308733](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417122308733.png)

## Carpetas y archivos creados

Estamos usando bind-mounts, estamos mapeando carpetas locales, ya que en nuestro `compose.yaml` hemos puesto:

```
volumes: 
 - ./data:/data
 - ./config:/config
```

Por lo tanto, las carpetas `data` y `config` están dentro de la carpeta donde se encuentra el archivo `yaml`. A su vez, el archivo que hemos subido desde FileBrowser (`Actividad Evaluable - Docker`) aparecerá en la carpeta `data`.

Para verlo más gráficamente, lo que vamos a hacer es acceder al contenedor de `filebrowser` para ver las rutas de estas carpetas. Para ello iremos a `Containers` y apretaremos sobre la flecha que aparece a la izquierda de `ejercicio_2` para desplegar el contenedor. Una vez desplegado, pulsamos sobre él.

![image-20250417124437218](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417124437218.png)

Dentro, seleccionamos la pestaña `Bind mounts`, y aquí podemos observar los paths en las que se montan los datos.

![image-20250417124746398](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417124746398.png)

Por último,vamos a acceder en nuestro caso a la ruta de la carpeta `data` para poder ver la carpeta creada desde FileBrowser y el archivo PDF que hemos subido.

![image-20250417125410614](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%202%20-%20Docker%20Compose.assets/image-20250417125410614.png)

