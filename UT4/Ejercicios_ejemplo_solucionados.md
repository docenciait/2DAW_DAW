# 1.  Despliegue de aplicación .NET

- [Despliegue de aplicación .NET](#despliegue-de-aplicación-net)
  - [Despliegue de aplicación .NET](#despliegue-de-aplicación-net-1)

## Despliegue de aplicación .NET
Para desplegar una aplicación .Net en un contenedor Docker, necesitarás un Dockerfile que defina cómo se va a construir tu contenedor y tu ejecutable

```Dockerfile
# Etapa de compilación
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build-env
WORKDIR /app

# Copia csproj y restaura dependencias
COPY *.csproj ./
RUN dotnet restore

# Copia todo lo demás y compila
COPY . ./
RUN dotnet publish -c Release -o out

# Etapa de prueba
FROM build-env AS test-env
WORKDIR /app/tests
COPY ./tests ./
RUN dotnet test --logger:trx

# Etapa de ejecución
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "your-app-name.dll"]

```

Para construir la imagen de Docker a partir de este Dockerfile, puedes usar el siguiente comando:

```bash
docker build -t myapp .
```

Y para ejecutar el contenedor, puedes usar el siguiente comando, si por ejemplo usa un puerto:

```bash
docker run -p 8000:8080 -d myapp
```

Este comando ejecutará tu aplicación y mapeará el puerto 8080 del contenedor al puerto 8080 de tu máquina local.

A continuación, puedes utilizar Docker Compose para gestionar tu aplicación. Aquí tienes un ejemplo básico de un archivo `docker-compose.yml` con una aplicación web y una base de datos MySQL:

```yaml
version: '3.8'
services:
  web:
    build: 
      context: .
      dockerfile: Dockerfile
    ports:
      - "8000:8080"
    depends_on:
      - db
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: testdb
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    volumes:
      - db_data:/var/lib/mysql
    ports:
      - "3306:3306"

volumes:
  db_data:
```

Para construir y ejecutar tu aplicación con Docker Compose, puedes usar el siguiente comando:

```bash
docker-compose up --build
```

Este comando construirá la imagen de tu aplicación (si no se ha construido ya) y luego ejecutará el contenedor.

# 2. 
# Proxy Inverso con Nginx

- [Proxy Inverso con Nginx](#proxy-inverso-con-nginx)
  - [¿Qué es un proxy inverso?](#qué-es-un-proxy-inverso)
  - [Configuración de un proxy inverso con Nginx](#configuración-de-un-proxy-inverso-con-nginx)
  - [Práctica Proxy Inverso](#práctica-proxy-inverso)

## ¿Qué es un proxy inverso?
Un proxy inverso es un servidor que recibe solicitudes de clientes y las reenvía a uno o más servidores de origen. A diferencia de un proxy normal, que se coloca entre el cliente y el servidor de origen, un proxy inverso se coloca entre el cliente y muchos servidores de origen. En este caso, el cliente no sabe a qué servidor de origen está conectado, y el servidor de origen no sabe que el cliente está conectado a él a través de un proxy inverso. Un proxy inverso se utiliza para equilibrar la carga entre varios servidores de origen, para proporcionar alta disponibilidad y escalabilidad, y para proteger los servidores de origen de los ataques de denegación de servicio.

Un proxy inverso se puede utilizar para:
- Equilibrar la carga entre varios servidores de origen.
- Proporcionar alta disponibilidad y escalabilidad.
- Proteger los servidores de origen de los ataques de denegación de servicio.
- Cachear contenido estático para mejorar el rendimiento.
- Enmascarar la dirección IP del servidor de origen.
- Enmascarar la dirección IP del cliente.


## 2. Configuración de un proxy inverso con Nginx

Para configurar un proxy inverso con Nginx, primero debe instalar Nginx en su servidor. Puede instalar Nginx en Ubuntu con el siguiente comando:

```bash
sudo apt-get update
sudo apt-get install nginx
```

Una vez que haya instalado Nginx, puede configurar un proxy inverso en el archivo de configuración de Nginx. El archivo de configuración de Nginx se encuentra en `/etc/nginx/nginx.conf`. La idea con este fichero es que se indique los servidores de origen a los que se va a redirigir el tráfico.

Antes de nada, debes tener en cuenta que cada uno de los servidores por separado deben ser capaces de responder a las peticiones en el path o ruta indicada, por lo que te recomiendo que cada parte responda tanto en la ruta por defecto como en la ruta que se va a redirigir (porque esa será la ruta a donde le llegue la petición).

Recuerda que como interiormente son contenedores, se le pasa el nombre del contenedor (si no sería la ip del servidor) y el puerto en el que escucha el servidor.

Una vez hecho esto, puedes configurar el proxy inverso en Nginx de la siguiente manera:

```nginx
events {}

http {
    server {
        listen 80
        server_name dominio.com;


        # Configuración de proxy para la raíz
        location / {
            proxy_pass http://nginx_server; # Servidor de origen
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }

        # Configuración de proxy para /one
        location /one {
            proxy_pass http://nginx_server; # Servidor de origen
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }

        # Configuración de proxy para /two
        location /two {
            proxy_pass http://apache_server; # Servidor de origen
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}

```

Un Docker Compose con esta infraestructura sería algo así:

```yaml
services:

  nginx:
    image: ubuntu/nginx # imagen de Nginx
    container_name: nginx_server # nombre del contenedor
    # ports:
    #  - "8081:80" # mapeo de puertos  HTTP
    volumes:
      - ./nginx/sites-available:/etc/nginx/sites-available # archivos de configuración de hosts virtuales
      - ./nginx/website:/var/www/html/ # directorio de los sitios web
    restart: always # reinicio automático
    networks:
      - webnet # red de contenedores

  apache:
    image: ubuntu/apache2 # imagen de Apache
    container_name: apache_server # nombre del contenedor
    # ports:
    #  - "8082:80" # mapeo de puertos  HTTP
    volumes:
      - ./apache/sites-available:/etc/apache2/sites-available # archivos de configuración de hosts virtuales
      - ./apache/website:/var/www/html/ # directorio de los sitios web
      - ./apache/htpasswd/.htpasswd:/etc/apache2/.htpasswd # archivo de contraseñas (en este caso lo uso)
    restart: always # reinicio automático
    networks:
      - webnet # red de contenedores

  proxy:
    image: ubuntu/nginx # imagen de Nginx
    container_name: proxy_server # nombre del contenedor
    ports:
      - "80:80" # mapeo de puertos  HTTP
      - "443:443" # mapeo de puertos  HTTPS
    volumes:
      - ./proxy/conf/nginx.conf:/etc/nginx/nginx.conf # archivo de configuración principal
      - ./proxy/certs:/etc/nginx/certs # directorio de certificados (hechos con openssl)
    restart: always # reinicio automático
    depends_on:
      - apache # dependencia de Apache
      - nginx # dependencia de Nginx
    networks:
      - webnet # red de contenedores

networks:
  webnet:
  
```

En este ejemplo, se configura un proxy inverso en Nginx que redirige el tráfico a dos servidores de origen: un servidor Nginx y un servidor Apache. El proxy inverso escucha en el puerto 80 y el puerto 443 y redirige el tráfico a los servidores de origen en función de la ruta de la solicitud. Por ejemplo, si la solicitud es para la raíz `/`, el tráfico se redirige al servidor Nginx. Si la solicitud es para `/one`, el tráfico se redirige al servidor Nginx. Si la solicitud es para `/two`, el tráfico se redirige al servidor Apache. Además, el proxy inverso establece las cabeceras `Host`, `X-Real-IP`, `X-Forwarded-For` y `X-Forwarded-Proto` para que los servidores de origen puedan saber de dónde proviene la solicitud.

Luego cada uno de ellos puede tener sus particulares configuraciones de los sitios web. De esta manera, en vez de usar un servidor web con múltiples virtual hosts, se pueden usar varios servidores web independientes y un proxy inverso para equilibrar la carga entre ellos o re-direccionar a cada uno y tener el mejor servidor según el caso


```bash
sudo systemctl restart nginx
```

# 3. Despliegue de Web con Netlify

- [Despliegue de Web con Netlify](#despliegue-de-web-con-netlify)
  - [Netlify](#netlify)
  - [Desplegando una web con Netlify](#desplegando-una-web-con-netlify)
    - [Deploy manually](#deploy-manually)
    - [Start from template](#start-from-template)
    - [Import an existing project](#import-an-existing-project)
  - [Práctica Netlify](#práctica-netlify)


## Netlify
Netlify es un servicio de alojamiento web gratuito y fácil de usar que ofrece Netlify para alojar sitios web estáticos directamente desde un repositorio de GitHub. Puedes crear un sitio web personal, un proyecto, una documentación o cualquier otro tipo de sitio web estático directamente desde tu repositorio de GitHub.

Para más información, visita la [documentación de Netlify](https://www.netlify.com/).

## Desplegando una web con Netlify
Lo primero que debemos hacer es crear una cuenta de usuario en Netlify. Para ello, vamos a la página de [Netlify](https://www.netlify.com/) y nos registramos.

El siguiente paso en sites, seleccionamos `And new Site` y tenemos varias opciones:
- Import an existing project: Importar un proyecto existente.
- Start from template: Empezar desde una plantilla.
- Deploy manually: Desplegar manualmente.

![](https://docs.netlify.com/images/sites-list-add-new-site.png)

### Deploy manually
Simplemente arrastramos o seleccionamos el directorio de nuestro proyecto y Netlify lo desplegará automáticamente. Podemos configurar el dominio y otras opciones.

### Start from template
Tendremos diferentes plantillas para elegir. Podemos seleccionar una y personalizarla a nuestro gusto, por ejemplo un portfolio, un blog, etc. con Astro, web plana o Next.js.

### Import an existing project
Indicamos el repositorio de GitHub que queremos desplegar y Netlify lo hará automáticamente. Seleccionamos el proyecto, el nombre del sitio y la rama a desplegar y el directorio base si no es la raíz, así como algún comando si lo necesitamos. También podemos configurar el dominio y otras opciones como variables de entorno, etc.


