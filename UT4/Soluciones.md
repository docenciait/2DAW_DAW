# Laboratorio 1: Despliegue con Script Shell

# Crear un archivo `deploy.sh` con el siguiente contenido:
Dockefile para django:
``` 
# Usar la imagen base de Python
FROM python:3.10

# Establecer el directorio de trabajo dentro del contenedor
WORKDIR /app


# Copiar el archivo de requerimientos e instalar dependencias
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copiar el código de la aplicación
COPY . .

# Exponer el puerto en el que Django correrá
EXPOSE 8000

# Definir el comando de arranque con el servidor de desarrollo de Django
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

```


#!/bin/bash

# Crear red y volúmenes
docker network create my_network
docker volume create pg_data
docker volume create static_data

# Desplegar PostgreSQL
docker run -d \
  --name postgres_container \
  --network my_network \
  -e POSTGRES_USER=django_user \
  -e POSTGRES_PASSWORD=django_password \
  -e POSTGRES_DB=django_db \
  -v pg_data:/var/lib/postgresql/data \
  postgres:latest

# Construir y ejecutar el contenedor Django
docker build -t django_app .
docker run -d \
  --name django_container \
  --network my_network \
  -e DATABASE_URL=postgres://django_user:django_password@postgres_container/django_db \
  -v static_data:/app/static \
  django_app

nginx.conf
server {
    listen 80;

    location / {
        proxy_pass http://django_container:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

# Ejecutar Nginx
docker run -d \
  --name nginx_container \
  --network my_network \
  -p 80:80 \
  -v "$(pwd)/nginx.conf:/etc/nginx/conf.d/default.conf" \
  nginx:latest

# Dar permisos de ejecución y ejecutar el script
```
chmod +x deploy.sh
./deploy.sh
```



---

# Laboratorio 2: Despliegue con Docker Compose

version: '3.8'

services:
  postgres:
    image: postgres:latest
    container_name: postgres_container
    environment:
      POSTGRES_USER: django_user
      POSTGRES_PASSWORD: django_password
      POSTGRES_DB: django_db
    volumes:
      - pg_data:/var/lib/postgresql/data
    networks:
      - my_network

  django:
    build: .
    container_name: django_container
    environment:
      DATABASE_URL: postgres://django_user:django_password@postgres:5432/django_db
    volumes:
      - static_data:/app/static
    depends_on:
      - postgres
    networks:
      - my_network

  nginx:
    image: nginx:latest
    container_name: nginx_container
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - django
    networks:
      - my_network

networks:
  my_network:
    driver: bridge

volumes:
  pg_data:
  static_data:

# 1. Construir y levantar los servicios con Docker Compose
```
docker-compose up --build -d
```

# 2. Verificar los logs de los contenedores
```
docker-compose logs -f
```

# 3. Acceder a `http://localhost` en el navegador para verificar el despliegue.

Este archivo `docker-compose.yml` simplifica la gestión de los contenedores, asegurando que los servicios se ejecuten en la misma red y con volúmenes persistentes.
