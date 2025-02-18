# Estas soluciones realizan un entorno mysql, django, nginx/ssl 

## Solución sin volúmenes

1. Imagen de Django

- `Dockerfile` de **Django**:

```bash

# Usamos Python como base
FROM python:3.11

# Set the working directory inside the container
WORKDIR /app

# Set environment variables
# Prevents Python from writing pyc files to disk
ENV PYTHONDONTWRITEBYTECODE=1
#Prevents Python from buffering stdout and stderr
ENV PYTHONUNBUFFERED=1

# Upgrade pip
RUN pip install --upgrade pip

# Copy the Django project  and install dependencies
COPY requirements.txt  /app/

# run this command to install all dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the Django project to the container
COPY . /app/

# Expose the Django port
EXPOSE 8000

# Run Django development server
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

- `settings.py` de Django:

Hay que configurar la base de datos usando pymysql agregrándolo al settings.py y luego 
configurar la base de datos, usamos como HOST el nombre del contenedor, que es mysql:

```python

...

import pymysql
pymysql.install_as_MySQLdb()

...

DATABASES = {
   'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'djangodb',
        'USER': 'user',
        'PASSWORD': '1234',
        'HOST': 'mysql',  # Nombre del contenedor de MySQL en la red de Docker
        'PORT': '3306',
    }
}
```

2. Imagen de mysql

- `Dokerfile` de mysql:

```bash
FROM mysql:8

# Definir variables de entorno
ENV MYSQL_ROOT_PASSWORD=root
ENV MYSQL_DATABASE=djangodb
ENV MYSQL_USER=user
ENV MYSQL_PASSWORD=1234

# Exponemos el puerto 3306
EXPOSE 3306
```
- Aparte el `requirements.txt` debe llevar lo siguiente:

```bash
asgiref==3.8.1
Django==5.1.6
sqlparse==0.5.3
tzdata==2025.1
PyMySQL
cryptography
```

3. Imagen de nginx:

- Antes debemos generar los certificados con: 

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout key.pem -out cert.pem -subj "/CN=localhost"
```

- Fichero `default.conf`:

```bash
server {
    listen 443 ssl;
    server_name localhost;

    ssl_certificate /etc/ssl/certs/cert.pem;
    ssl_certificate_key /etc/ssl/private/key.pem;

    location / {
        proxy_pass http://mi-django-app:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

- **Fijarse que con** `http://mi-django-app:8000` **podemos acceder por nombre del contenedor:**

- `Dockerfile` de nginx:

```bash
FROM nginx:latest

COPY default.conf /etc/nginx/conf.d/default.conf
COPY cert.pem /etc/ssl/certs/cert.pem
COPY key.pem /etc/ssl/private/key.pem
EXPOSE 443 80
```



4. Generación de imágenes

Para cada Docker file se crean las siguientes imágenes:
El . es en la ruta actual pero podemos escoger una carpeta.

```bash
docker build -t mi-django-app . 
docker build -t mi-mysql .
docker build -t mi-nginx .
```

- Si hacemos un `docker image ls`:

```bash
S C:\Users\madrid\django-projects\> docker image ls
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
mi-django-app   latest    5ad29717865c   26 minutes ago   1.57GB
mi-nginx        latest    a3149071e557   42 minutes ago   279MB
mi-mysql        latest    7023335dd2d7   4 weeks ago      1.05GB
```
Veremos las imágenes creadas:

5. Creación de la red

```bash
docker network create mi-red

```

6. Crear los contenedores:

```bash
docker run -d --name mysql --network mi-red mi-mysql
docker run -d --name mi-django-app --network mi-red mi-django-app
docker run -d -p 443:443 --name mi-nginx --network mi-red mi-nginx

````

- Contenedores creados:

```bash
PS C:\Users\madrid\django-projects\nginx> docker container ls
CONTAINER ID   IMAGE           COMMAND                  CREATED             STATUS             PORTS                                      NAMES
f5b72b183118   mi-django-app   "python manage.py ru…"   32 minutes ago      Up 32 minutes      8000/tcp                                   mi-django-app
6805cc72bb0c   mi-nginx        "/docker-entrypoint.…"   47 minutes ago      Up 47 minutes      0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   mi-nginx
a92d31d30a21   mi-mysql        "docker-entrypoint.s…"   About an hour ago   Up About an hour   3306/tcp, 33060/tcp 
```

7. Migraciones django

Realizar las migraciones de django:

```bash
docker exec -it mi-django-app python manage.py migrate
docker exec -it mi-django-app python manage.py createsuperuser
```

8. Volúmenes (no añadidos a este ejemplo):

- Creamos un volumen para mysql:

```bash
docker run -d \
  -v mi-mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=mi-clave \
  --name mi-mysql \
  --network mi-red \
  mi-mysql

```

- Creamos volumen para django: media y static:

```bash
docker run -d \
  -v /ruta/local/a/archivos/static:/app/static \
  -v /ruta/local/a/archivos/media:/app/media \
  --name mi-django-app \
  --network mi-red \
  mi-django-app
```


9. Logs

- Con el comando: 
```docker logs <nombre-contenedor>```
Obtendrás los logs del contenedor y verás los posibles fallos.

- De red podrás conocer el direccionamiento de red y además qué contenedores están conectados con:

```bash
docker network inspect mi-red
```

--- 

# DOCKER COMPOSE

```bash
version: '3.8'

services:
  mi-mysql:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: djangodb
      MYSQL_USER: user
      MYSQL_PASSWORD: 1234
    volumes:
      - mi-mysql-data:/var/lib/mysql
    networks:
      - mi-red
    ports:
      - "3306:3306"
    restart: always

  mi-django-app:
    build:
      context: .
      dockerfile: Dockerfile.django  # Ajusta el Dockerfile si es necesario
    environment:
      - DATABASE_URL=mysql://usuario:clave@mi-mysql:3306/mi_basededatos
    volumes:
      - ./static:/app/static
      - ./media:/app/media
    depends_on:
      - mi-mysql
    networks:
      - mi-red
    ports:
      - "8000:8000"
    restart: always

  mi-nginx:
    image: nginx:alpine
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./static:/app/static
    depends_on:
      - mi-django-app
    networks:
      - mi-red
    ports:
      - "80:80"
      - "443:443"
    restart: always

networks:
  mi-red:

volumes:
  mi-mysql-data:

```