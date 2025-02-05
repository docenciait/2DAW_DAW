##Laboratorio 1
**Objetivo:** 
En este laboratorio, configurarás una red en Docker que interconecte tres contenedores: un servidor Nginx que actúe como proxy inverso, una aplicación web Django y una base de datos PostgreSQL. Además, implementarás volúmenes para la persistencia de datos.**Requisitos previos:**  
- Tener Docker instalado en el sistema.
 
- Conocimientos básicos de Docker, redes y volúmenes.
 
- Familiaridad con Nginx, Django y PostgreSQL.
**Actividades:**  
1. **Creación de la red y volúmenes**  
  - Crea una red en Docker para permitir la comunicación entre los contenedores.
 
  - Define volúmenes para la persistencia de datos en PostgreSQL y almacenamiento estático en Django.
 
2. **Despliegue de PostgreSQL**  
  - Ejecuta un contenedor con la imagen oficial de PostgreSQL.
 
  - Configura el usuario, base de datos y contraseña.
 
  - Monta un volumen para garantizar la persistencia de los datos.
 
3. **Despliegue de la aplicación Django**  
  - Crea una imagen de Docker para una aplicación Django.
 
  - Configura la aplicación para conectarse a la base de datos PostgreSQL.
 
  - Utiliza un volumen para almacenar archivos estáticos.
 
  - Ejecuta la aplicación en un contenedor de Docker.
 
4. **Configuración de Nginx como proxy inverso**  
  - Define un archivo de configuración para Nginx.
 
  - Implementa un contenedor Nginx que sirva de proxy para la aplicación Django.
 
  - Expone el puerto 80 para acceder a la aplicación desde el navegador.
 
5. **Pruebas y verificación**  
  - Accede a la aplicación desde un navegador mediante la dirección del servidor.
 
  - Verifica la correcta conexión entre los contenedores.
 
  - Asegura la persistencia de los datos tras reiniciar los contenedores.

**Resultados esperados:**  
- Los contenedores deben poder comunicarse dentro de la misma red.
 
- Nginx debe actuar correctamente como proxy inverso hacia Django.
 
- La base de datos debe almacenar los datos de manera persistente.
 
- La aplicación debe estar disponible a través del navegador.

## Laboratorio 2

- Realizar lo mismo que lo anterior pero con Docker-Compose