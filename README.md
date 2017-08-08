# NodeJS Proyecto01

By: Daniela Serna Escobar - dsernae@eafit.edu.co

# Descripción de aplicación

Aplicación web que permite gestionar Musica, un CRUD básico de metadatos MP3 (titulo, artista, genero, año).
# 1. Análisis

## 1.1 Requisitos funcionales:

1. Crear canción.
2. Buscar canción por parte del titulo
3. Borrar canción por Id de la canción
4. Listar todas las canciones de la base de datos que sean publicas y sean de año 2017
5. Listar las canciones privadas de un usuario
6. Listar las canciones compartidas con un usuario
7. Crear usuario
8. Buscar usuario por nombre de usuario para compartir canciones
9. Borrar usuario
10. Actualizar contraseña

## 1.2 Definición de tecnología de desarrollo y despliegue para la aplicación:

* Lenguaje de Programación: Javascript
* Framework web backend: NodeJS - Express
* Framework web frontend: Angular 2
* Base de datos: MongoDB
* Web App Server: NodeJS
* Web Server: NGINX

# 2. Desarrollo

Se generó la base del servidor, con Yeoman:

	$ yo express

Luego se generó la base del cliente con Angular 2:

	$ng new TanzenMusic 

# 3. Diseño:

## 3.1 Modelo de datos:

	song:
	{
	  	title: String,
	  	artist: String,
	  	date: String,
	  	genre: String,
	  	owner_username: String,
	  	visibility: String,
	  	sharedWith: [String]
	}

	person: 
	{
	 	username: String,
		password: String
	}

## 3.2 Servicios Web
### Canciones

	/* Servicio Web: Crear Canción
	  Método: POST
	  Autenticado: SI
	  URI: /createSong
	  Body:
	  {
		"title": val,
		"artist": val,
		"date": val,
		"genre": val,
		"visibility" : ["Publico"| "Privado"],
		"sharedWith": val[] 
	 }
	*/
	/* Servicio Web: Listar todas las canciones publicas
	  Método: GET
	  Autenticado: NO
	  URI: /getAllSongs
	*/
	
	/* Servicio Web: Buscar cancion publica por titulo
	  Método: POST
	  Autenticado: NO
	  URI: /searchSongs
	  Body:
	  {
		  "searchTerm": val
	  }
	*/

	/* Servicio Web: Buscar cancion publica por genero
	  Método: POST
	  Autenticado: NO
	  URI: /songGenre
	  Body:
	  {
		  "searchTerm": val
	  }
	*/
	
	/* Servicio Web: Listar todas las canciones privadas que contengan parte del titulo
	  Método: POST
	  Autenticado: SI
	  URI: /searchSongs
	  Body:
	  {
		  "searchTerm": val,
		  "username": val
	  }
	*/
	
	/* Servicio Web: Listar las canciones compartidas con el usuario
	  Método: POST
	  Autenticado: SI
	  URI: /sharedWith
	  Body:
	  {
		  "username": val
	  }
	*/
	
	/* Servicio Web: Listar canciones compartidas a un usuario
	  Método: POST
	  Autenticado: Si
	  URI: /sharedWith
	  Body:
	  {
		  "username": val
	  }
	*/
	
	/* Servicio Web: Actualizar Canción
	  Método: POST
	  Autenticado: SI
	  URI: /updateSong
	  Body:
	  {
		"title": val,
		"artist": val,
		"date": val,
		"genre": val,
		"visibility" : ["Publico"| "Privado"],
		"sharedWith": val[] 
	  }
	*/
	
	/* Servicio Web: Borrar cancion por Id
	  Método: POST
	  Autenticado: SI
	  URI: /deleteSong
	  Body:
	  {
		  "id": "val"
	  }
	*/	
### Usuarios
	/* Servicio Web: Crear usuario
	  Método: POST
	  Autenticado: NO
	  URI: /signup
	  Body: 
	  {
	  	"username": "val",
	  	"password": "val"
	  }
	*/
	/* Servicio Web: Ingresar a la plataforma con un usuario
	  Método: POST
	  Autenticado: NO
	  URI: /login
	  Body: 
	  {
	  	"username": "val",
	  	"password": "val"
	  }
	*/
	/* Servicio Web: Actualizar contraseña de usuario
	  Método: POST
	  Autenticado: SI
	  URI: /updatePassword
	  Body: 
	  {
	  	"_id": val,
	  	"password": val,
	  }
	*/
	
	/* Servicio Web: Borrar usuario
	  Método: POST
	  Autenticado: SI
	  URI: /deleteUser
	  Body: 
	  {
	  	"_id": val,
	  	"username": val,
	  }
	*/

		/* Servicio Web: Logout usuario
	  Método: GET
	  Autenticado: SI
	  URI: /logout
	*/

		/* Servicio Web: traer informacion del usuario
	  Método: POST
	  Autenticado: SI
	  URI: /userInfo
	*/

# 4. Despligue en un Servidor Centos 7.x en el DCA

## 4.1 Se instala nvm local para el usuario

source: https://www.liquidweb.com/kb/how-to-install-nvm-node-version-manager-for-node-js-on-centos-7/

      user1$ nvm install v7.7.4

## 4.2 Se instala el package del cliente con sus dependencias de Angular

      dsernae$ npm install --save @angular/cli

## 4.3 Se instala el servidor mongodb

como root:

      user1$ sudo yum install mongodb-server -y'

ponerlo a correr:

      user1$ sudo systemctl enable mongod
      user1$ sudo systemctl start mongod

## 4.4 Se instala NGINX

      user1$ sudo yum install nginx
      user1$ sudo systemctl enable nginx
      user1$ sudo systemctl start nginx

Abrir el puerto 80

      user1$ sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
      user1$ sudo firewall-cmd --reload

## 4.5 Abrir los puertos en el firewall que utilizara la app:

      user1$ firewall-cmd --zone=public --add-port=3001/tcp --permanent
      user1$ firewall-cmd --reload
      user1$ firewall-cmd --list-all

## 4.6 Se instala un manejador de procesos de nodejs, se instala: PM2 (http://pm2.keymetrics.io/)

      user1$ npm install -g pm2
      user1$ cd dev/proyecto1/
      user1$ pm2 start app.js
      user1$ pm2 list

ponerlo como un servicio, para cuando baje y suba el sistema:    

      user1$ sudo pm2 startup systemd

Deshabilitar SELINUX

          user1$ sudo vim /etc/sysconfig/selinux

                SELINUX=disabled

          user1$ sudo reboot      

### 4.7 Configuración del proxy inverso en NGINX para cada aplicación:

      // /etc/nginx/nginx.config
      .
      .
      server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  10.131.137.219;
        root         /usr/share/nginx/html;
      .
      .
      location / {
        root /usr/share/nginx/html;
        index index.html index.htm;
      }

      location /dsernae/{
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header HOST $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_pass http://127.0.0.1:3001/;
        proxy_redirect off;
      }
      .
      .
      location /dsernae/ {
        if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        #
        # Custom headers and headers various browsers should be OK with but aren't
        #
        add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Content-Range,Range';
        #
        # Tell client that this pre-flight info is valid for 20 days
        #
        add_header 'Access-Control-Max-Age' 1728000;
        add_header 'Content-Type' 'text/plain; charset=utf-8';
        add_header 'Content-Length' 0;
        return 204;
     }
     if ($request_method = 'POST') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Content-Range,Range';
        add_header 'Access-Control-Expose-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Content-Range,Range';
     }
     if ($request_method = 'GET') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Content-Range,Range';
        add_header 'Access-Control-Expose-Headers' 'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Content-Range,Range';
     }
         alias /home/dsernae/dev/proyecto1/TanzenMusic/dist/;
         try_files $uri$args $uri$args/ /dsernae/index.html;
        }



# 5. Despliege Produccion:

## 5.1 Proveedor de PaaS: Heroku:

Pasos para el despliegue luego de tener la cuenta:
Se descarga el CLI de Heroku: https://devcenter.heroku.com/articles/heroku-command-line
Luego en la terminal se ejecutan los siguentes pasos:
	
	$ heroku login
	>usernamae:dsernae@eafit.edu.co
	>password *********

Ir hacia la carpeta donde esta el proyecto

	$git init
	$ git add .
	$ git commit -am "first commit"
	$ git push heroku master

y se verifica en la pagina https://morning-retreat-11483.herokuapp.com/

## 5.2 Proveedor de BDaaS mLab:

Se crea una cuenta en MLab y se crea una base de datos nueva, despues se crea un usuario y se agrega la ruta de la base de datos en el archivo respectivo de la configuracion:

	TanzenServer/config/config.js:
	.
	.
	production: {
		root: rootPath,
		app: {
			name: 'tanzenserver'
		},
		port: process.env.PORT || 8080,
		db: 'mongodb://dsernae1:dsernae1@ds149201.mlab.com:49201/tanzenserver'
	}
	.
	.


