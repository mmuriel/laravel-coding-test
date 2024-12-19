# LARAVEL CODING TEST

Este proyecto incluye un reto de programación para los nuevos candidatos a Desarrollador Fullstack, para las empresas: SIBA S.A., SIENTIFICA S.A.S., o cameyando.com


## Requisitos para correr el proyecto

Se necesita que la máquina donde se va a instalar el proyecto incluya:

- Git
- Docker. [Docker Desktop](https://docs.docker.com/desktop/setup/install/windows-install/) para plataformas Windows, [Docker Engine](https://docs.docker.com/engine/install/) para las demás plataformas.

## Instrucciones para correr el proyecto (Instalación)

Las instrucciones para correr el proyecto son las siguientes:

0. Asegurarse que no esté corriendo ningún servicio en los puertos 8000 y 8090 de su máquina local.

1. Tener instalados: docker (engine o desktop según la plataforma) y git en su máquina.

2. Crear una carpeta para el proyecto:

```
$ mkdir /path/to/project
```

3. Vincular este proyecto en la carpeta recién creada:

```
$ cd /path/to/project
$ git init
$ git branch -m "main"
$ git remote add origin git@github.com:mmuriel/laravel-coding-test.git
$ git pull origin main
```

4. Generar el archivo docker-compose.yml a partir del archivo /path/to/project/docker-compose.yml.template

```
$ cp docker-compose.yml.template docker-compose.yml
```

5. Ajustar los valores a las rutas correctas en el sistema local de archivos, de los volúmenes en el archivo recién copiado docker-compose.yml, particularmente los campos:

	- services.app.volumes
	- services.db.volumes


6. Arrancar los contenedores:

```
$ docker compose up -d &
```

7. Instalar las dependencias de laravel, para esto se debe ingresar al contenedor "app"

```
$ docker exec -it app /bin/bash
```
Una vez dentro del contenedor, ejecutar la instalación de las dependencias:

```
$ cd /var/www/app/
$ composer install
```

8.  Crear el archivo .env a paritir del archivo .env.example, este archivo es necesario para la operación de Laravel. Estando dentro del contenedor "app", ejecutar el siguiente comando:

```
$ cp /var/www/app/.env.example /var/www/app/.env
``` 

Una vez copiado el archivo de variables de entorno del framework, ajustar los valores de acuerdo a la instalación local.

9. Asegurarse que la carpeta de almacenamiento de Laravel (/path/to/project/storage) tenga los permisos adecuados de escritura.

```
$ chmod -Rf 777 /var/www/app/storage/
``` 

10. Generar un API KEY del proyecto, con el comando: php artisan key:generate

```
$ cd /var/www/app/
$ php artisan key:generate
```

11. Comprobar que el proyecto se ejecuta correctamente apuntando el navegador web a la URL: http://localhost:8000 (ajustar el valor del puerto, de acuerdo a la configuración del contenedor "app" en el archivo docker-compose.yml)


FIN. 


## ¿En qué consiste el reto?:

El reto consiste en desarrollar una serie de archivos php, en el contexto del framework Laravel (es decir: modelos, vistas, controladores, comandos, rutas... etc.) que realicen lo siguiente:


1. Lea los datos que se encuentran en el archivo: docs/TV-GLOBO.txt, y almacenarlos en una base de datos MariaDB donde se pueda posteriormente consultar los campos: 

	- Fecha y hora
	- Título del programa
	- Descripción del programa (sinopsis)

Es decir, hay que desarrollar una series de archivos php (modelos, controladores y/o comandos Laravel) que lean desde el archivo de texto mencionado (docs/TV-GLOBO.txt), extraiga los campos mencionados (fecha y hora, Título de programa y Sinopsis) y los almacene en la base de datos.

2. Luego de tener los datos almacenados en MariaDB, se debe construir otra serie de scripts php (de nuevo, bajo el contexto de Laravel, modelos, vistas, controladores, rutas, comandos... etc.) que al ser ejecutado reciba dos (2) parámetros, una fecha de inicio y una fecha de cierre, que defina un rango de tiempo para recuperar los programas almacenados, con los datos recuperados desde MariaDB, se debe escribir un documento XML que tenga la siguiente estructura de nodos:

```xml
<programacion inicio="YYYY-MM-DD HH:MM:SS" fin="YYYY-MM-DD HH:MM:SS">
	<evento>
		<fecha-hora>YYYY-MM-DD HH:MM:SS</fecha-hora>
		<titulo>Titulo del primer evento del rango de tiempo</titulo>
		<sinopsis>Sinopsis del evento</sinopsis>
	</evento>
	.
	.
	.
	<evento>
		<fecha-hora>YYYY-MM-DD HH:MM:SS</fecha-hora>
		<titulo>Titulo del último evento del rango de tiempo</titulo>
		<sinopsis>Sinopsis del evento</sinopsis>
	</evento>
</programacion>
```

El documento XML resultante, debe poder ser consultado vía web en la ruta /programacion, apuntando el navegador a la dirección: http://localhost:8000/programacion.

- *Nota:* El segmento XML anterior es solo un ejemplo de la estructura del cuerpo del documento que se solicita, se deben incluir todos los tags básicos para la construcción de un documento XML válido.


## Otras consideraciones del reto:

1. Queda a su criterio la forma de programar los scripts y la forma de ser ejecutados, pueden ser comandos Laravel accesibles vía CLI, o todo un grupo de archivos que respondan al patrón MVC del framework, que se ejecuten vía un navegador web.

2. Para acceder al contenedor vía terminal donde se ejecutan los script puede usar el siguiente comando de terminal:

> docker exec -it app /bin/bash

El proyecto se encuentra en la ruta: /var/www/app, dentro del contenedor.

3. Queda a su criterio la estructura de la entidad (tabla) o entidades (tablas) en MariaDB para persistir los datos. Lo que si debe usar es la base base de datos ya creada que se llama: programacion

4. MUY Importante, si usted no alcanza a terminar todo el proyecto o no está seguro de alguna solución implementada, no se preocupe, reporte el trabajo realizado.

... Buena programación ;-)


