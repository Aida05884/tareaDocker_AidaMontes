# Actividad Evaluable 3 - Docker - EJERCICIO 1 - Contenedores en red y Docker Desktop

> Tarea creada por Aida Montes Cabello para el módulo Despliegue de Aplicaciones Web.

[TOC]

## **Introducción**

En este documento se detallarán los pasos a seguir para la resolución del Ejercicio 1 de la Actividad Evaluable 3 de Despliegue de Aplicaciones Web sobre Docker. Para la realización de dicho ejercicio, utilizaremos la extensión PortNavigator para poder gestionar las redes desde Docker Desktop.



## **Instalación extensión PortNavigator**

Para el desarrollo de este ejercicio, es necesario instalar la extensión PortNavigator en nuestro Docker Desktop, para ello abrimos la aplicación y vamos a la pestaña `Extension` y buscamos dicha extensión y la instalamos.

![image-20250415194825069](C:/Users/Usuario/AppData/Roaming/Typora/typora-user-images/image-20250415194825069.png)

Acto seguido nos aparecerá en el panel izquierdo. Con esto ya podemos crear nuestra red a través de esta extención.

<img src="C:/Users/Usuario/AppData/Roaming/Typora/typora-user-images/image-20250415195006199.png" alt="image-20250415195006199" style="zoom:50%;" />

### **1.** Creación red bridge

Para la creación de nuestra red bridge `redej1`, abrimos PortNavigator y pulsamos en el botón `Add Network`

![image-20250415195434316](C:/Users/Usuario/AppData/Roaming/Typora/typora-user-images/image-20250415195434316.png)

Como podemos ver a continuación, nos saldrá una pestaña donde añadiremos el nombre de nuestra red y pulsamos `Create Network`.

![image-20250415195813369](C:/Users/Usuario/AppData/Roaming/Typora/typora-user-images/image-20250415195813369.png)

Después de crearla, confirmamos que se haya generado correctamente y comprobamos que es una red birdge.

![image-20250415195900221](C:/Users/Usuario/AppData/Roaming/Typora/typora-user-images/image-20250415195900221.png)

### **2.** Creación contenedor MariaDB

En este apartado vamos a crear un contenedor que se ejecutará en segundo plano utilizando una imagen de `mariaDB` y que estará conectado a la red configurada en el punto anterior. Este contenedor también tendrá que ser accesible a través del puerto 3306. En este punto también realizaremos las siguientes tareas:

- Definición de una contraseña para el usuario `root`, y un usuario que será el nombre de pila y una contraseña para éste (BD: DAW).
- Creación de un script SQL que defina una tabla llamada `módulos` e inserción de algunos registros con nombres de módulos que se esté cursando.

Lo primero que debemos hacer antes de crear el contenedor, es descargar la imagen de `mariadb`, para ello utilizaremos el buscador para buscarla. Una vez encontrada, pulsaremos el botón `Pull`.

![image-20250416170606005](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416170606005.png)

Una vez descargada, accederemos a la pestaña `Images`, que se encuentra en el panel izquierdo de la aplicación.

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416170138366.png" alt="image-20250416170138366" style="zoom:67%;" />

A continuación podemos observar que se ha descargado correctamente, por lo que pulsamos sobre `Run` para la creación del contenedor.

![image-20250416171435971](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416171435971.png)

#### Configuración contenedor

Ahora comenzaremos con la configuración del contenedor cuyo nombre va a ser `db-DAW` y la accesibilidad del puerto 3306. También agregaremos las variables de entorno: la contraseña para el usuario `root`(`MYSQL_ROOT_PASSWORD`), la base de datos DAW (`MYSQL_DATABASE`), un usuario (`MYSQL_USER`) y su correspondiente contraseña (`MYSQL_PASSWORD`). Una vez que se hayan añadido, presionaremos el botón `Run`.

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416173829693.png" alt="image-20250416173829693" style="zoom:67%;" />

Con esto, el contenedor se iniciará, pero todavía no estaría en la red `redej1`, para ello accederemos a la extensión `PortNavigator` y buscamos el contenedor creado, el cual vemos que no tiene asignada ninguna red, por lo tanto hacemos clic en `Connect to Networks`.

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416181437023.png" alt="image-20250416181437023" style="zoom: 67%;" />

Aquí seleccionamos la red redej1 y conectamos.

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416181559810.png" alt="image-20250416181559810" style="zoom:67%;" />

Ahora vamos a comprobar que el contenedor está conectado a nuestra red. En `PortNavigator` buscamos la red y observamos que hay un contenedor conectado, por lo que pulsamos `Show Containers` para verificarlo. En efecto, el contenedor `db-DAW` está conectado a `redej1` usando `PortNavigator`.

![image-20250416182554938](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416182554938.png)

#### Script SQL

Por último vamos a crear un script SQL que defina una tabla cuyo nombre será `módulos` e insertaremos algunos registros con los nombres de los módulos que se esté cursando.

```
CREATE TABLE modulos (
	id INT AUTO_INCREMENT PRIMARY KEY,
	nombre VARCHAR(50) NOT NULL
);

INSERT INTO modulos (nombre) VALUES
('Despliegue de Aplicaciones Web'),
('Desarrollo Web en Entorno Cliente'),
('Desarrollo Web en Entorno Servidor'),
('Diseño de Interfaces Web'),
('Empresa e Iniciativa Emprendedora');
```



### **3.** Creación contenedor con Adminer

El punto 3 de este ejercicio, nos pide crear un contenedor con `Adminer` o con `phpMyAdmin` que podamos conectar al contenedor de la BD, pues bien, vamos a escoger crear el contenedor `Adminer`, que es una herramienta que gestiona bases de datos desde el navegador y que es similar a `phpMyAdmin`.

Para ello, vamos a `Images` y en el buscador escribimos `adminer`, seguiremos los mismos pasos realizados con la imagen de mariadb en el punto número 2. 

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416184351378.png" alt="image-20250416184351378" style="zoom:67%;" />

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416184805405.png" alt="image-20250416184805405" style="zoom:67%;" />

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416185610466.png" alt="image-20250416185610466" style="zoom:67%;" />

Como nos pasó con anterioridad con mariaDB, el contenedor `adminer-DAW` se iniciará, pero aún no estará conectado a `redej1`. Por lo tanto seguiremos también los pasos seguidos con db-DAW. Buscaremos el contenedor y pulsaremos Connect to Networks donde seleccionaremos redej1.

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416190502773.png" alt="image-20250416190502773" style="zoom:67%;" />

Ahora, tanto Maria DB (`db-DAW`) como Adminer (`adminer-DAW`) están conectadas en la misma red bridge, lo que les permite comunicarse entre sí usando sus nombres como si fueran direcciones IP.

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416190701909.png" alt="image-20250416190701909" style="zoom:67%;" />



### **4.** Conexión a la BD.

En este punto, desde Microsoft Edge nos conectaremos a la BD con el usuario creado en el punto 2, ejecutaremos el script con los datos de los módulos mostrando la BD y la tabla creados.

Para conectarnos a la BD escribiremos en nuestra interfaz http://localhost:8080 y nos aparecerá lo siguiente, que rellenaremos con nuestros datos.

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416193506244.png" alt="image-20250416193506244" style="zoom:67%;" />

Una vez dentro, hacemos clic en Comando SQL y pegamos el script creado en el apartado 2 y lo ejecutamos.

![image-20250416195607945](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416195607945.png)

![image-20250416200157352](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416200157352.png)

Al ejecutar el script nos sale lo siguiente:

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416200827207.png" alt="image-20250416200827207" style="zoom:67%;" />

Esto quiere decir que se han ejecutado las consultas correctamente, y que en el caso de la inserción de los módulos, se han hecho 5 registros.

Finalmente, vamos a comprobar que la base de datos y la tabla se han creado, mostrando el contenido. Para ello, accederemos a la tabla `módulos` que aparecerá en la parte izquierda del menú. Inicialmente se mostrará su estructura, pero lo que queremos es ver son los datos de la tabla, por lo que pulsamos en `Visualizar contenido`. De este modo, podemos observar los registros insertados en la tabla.

![image-20250416201824749](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416201824749.png)

### **5.** Instalación Disk Usage

En este punto instalaremos la extensión `Disk Usage` para comprobar el espacio ocupado y posteriormente eliminar los contenedores, las redes y los volúmenes que no se estén usando.

Lo primero que haremos será ir a `Extensions`, buscar `Disk Usage` y pulsamos `Install`.

![image-20250416203957497](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416203957497.png)

Una vez instalada, nos aparecerá en el lateral izquierdo, junto a PortNavigator, pulsamos sobre ella y podremos ver el espacio ocupado por imágenes, contenedores y volúmenes en forma de gráfico y tabla.

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416204558351.png" alt="image-20250416204558351" style="zoom:67%;" />

<img src="Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416205148040.png" alt="image-20250416205148040" style="zoom:67%;" />

#### Borrado de elementos. Comprobación espacio.

Ahora vamos a eliminar los contenedores (`db-DAW` y `adminer-DAW`), la red (`redej1`) y los volúmenes. 

 Primero vamos a acceder a `Containers` y eliminar los contenedores creados con anterioridad pulsando sobre la papelera que aparece a sus derechas.

![image-20250416210522619](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416210522619.png)

Tanto en `db-DAW` como en `adminer-DAW` nos saldrá la siguiente advertencia en la cual pondremos `Delete forever`.

![image-20250416210711891](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416210711891.png)

Lo mismo haremos con las imágenes tanto de `mariadb` como `adminer`.

![image-20250416210902114](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416210902114.png)

En volúmenes ya no saldría nada ya que no hay nada que lo vincule.

Para eliminar la red, iremos a PortNavigator y buscaremos la red redej1 y pulsaremos sobre la cruz.

![image-20250416211459253](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416211459253.png)

Por último, vamos a comprobar en Disk usage que el espacio ha disminuido con el borrado de estos elementos.

![](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416211702646.png)

![image-20250416211809278](Actividad%20Evaluable%203%20-%20Docker%20-%20EJERCICIO%201%20-%20Contenedores%20en%20red%20y%20Docker%20Desktop.assets/image-20250416211809278.png)

