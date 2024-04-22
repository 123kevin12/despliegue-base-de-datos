# despliegue-base-de-datos
tarea de sistemas operativos grupo 01
Nombres: 
Kevin Andres Prado - 2242595 - kevin.prado@correounivalle.edu.co
Jhoan Sebastian Rivera - 2241872 - rivera.jhoan@correounivalle.edu.co

Descripcion tarea:
El siguiente trabajo tiene como objetivo la creación y modificación de una base de datos de postgres utilizando 2 containers de docker que esten conectados en la misma network

link video de youtube:
https://youtu.be/zQS00RKRYg8

scripts usados:

docker volume create pg_db 	#creacion del volumen

docker network create pg_network 	#creacion de la network

docker run --name pg_server --network pg_network -v pg_db:/var/lib/postgresql/data -e POSTGRES_PASSWORD=hola123 -d postgres:15-bookworm #creacion del container de servidor que se conecta a la network y tiene acceso a el volumen

docker exec -it pg_server bash		 #entramos al container

psql -U postgres			 #entramos a postgres

CREATE DATABASE tarea_db; 		 #creacion de la base de datos

\c tarea_db; 				#accediendo a la base de datos

CREATE TABLE pg_tabla(
    id SERIAL PRIMARY KEY,
    mensaje TEXT
);    					#creado la tabla y asignandole atributos

SELECT * FROM pg_tabla; 		#verificando que se creo la tabla

\q 					#saliendo de la base de datos

/#Exit 					#saliendo del container

docker run --name pg_client --network pg_network -it postgres:15-bookworm psql -h pg_server -U postgres #creacion del container de cliente que esta conectado a la network creada previamente

\c tarea_db;  				 #accediendo a la base de datos

SELECT*FROM pg_tabla  			 #verificando que existe la tabla   

INSERT INTO pg_tabla(mensaje) VALUES(‘hola mundo’); 	#insertando datos a la tabla (mensaje de hola mundo)

SELECT*FROM pg_tabla  			 #verificando que el mensaje se introdujo de forma correcta

\q  					#saliendo de la base de datos 

docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)   #deteniendo los containers

docker  rm $(docker ps -a -q)   	# removiendo los containers

docker volume rm pg_db 			 #removiendo el volumen

docker network rm pg_network 		#removiendo la network
