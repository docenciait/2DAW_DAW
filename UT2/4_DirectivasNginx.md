# Directivas NGINX

[LISTADO ALFABÉTICO DE DIRECTIVAS](https://nginx.org/en/docs/dirindex.html)

NGINX es un servidor web ligero y altamente configurable, utilizado comúnmente como servidor web, proxy inverso, balanceador de carga y servidor de caché. Su configuración se basa en un archivo de texto plano donde se utilizan **directivas**  que configuran el comportamiento del servidor.
Las directivas de NGINX pueden clasificarse en dos tipos:
 
- **Directivas simples:**  constan de una sola línea y terminan con un punto y coma.
 
- **Directivas de bloque:**  tienen una estructura de bloque, anidan otras directivas en su interior y terminan con llaves `{}`.

A continuación, se presentan ejemplos de las directivas más comunes en NGINX, clasificadas en diferentes categorías.
1. **Directivas generales** Estas directivas se configuran en el bloque principal del archivo `nginx.conf` o en bloques más específicos (http, server o location). 
- **`worker_processes`** : Configura el número de procesos de trabajo que se ejecutarán. A menudo se usa el valor `auto` para que NGINX determine el número de núcleos de la CPU disponible.

```nginx
worker_processes auto;
```
 
- **`error_log`** : Establece la ubicación y el nivel de severidad del archivo de registro de errores.

```nginx
error_log /var/log/nginx/error.log warn;
```
2. **Directivas del bloque HTTP** El bloque `http {}` contiene configuraciones relacionadas con la gestión del tráfico HTTP. 
- **`client_max_body_size`** : Define el tamaño máximo permitido para el cuerpo de las solicitudes de los clientes.

```nginx
http {
    client_max_body_size 50M;
}
```
 
- **`keepalive_timeout`** : Establece el tiempo que una conexión HTTP se mantiene abierta antes de cerrarse.

```nginx
http {
    keepalive_timeout 65;
}
```
3. **Directivas del bloque Server** Dentro del bloque `http`, puedes tener múltiples bloques `server` que representan servidores virtuales. Las directivas del bloque `server` especifican las configuraciones para cada uno de estos servidores. 
- **`listen`** : Define en qué puerto y dirección IP escucha el servidor.

```nginx
server {
    listen 80;
}
```
 
- **`server_name`** : Define el nombre del servidor, utilizado para el enrutamiento de solicitudes a diferentes sitios virtuales.

```nginx
server {
    server_name www.ejemplo.com;
}
```
 
- **`root`** : Define el directorio raíz donde se encuentran los archivos estáticos del sitio.

```nginx
server {
    root /var/www/html;
}
```
 
- **`error_page`** : Establece las páginas de error personalizadas para diferentes códigos de estado HTTP.

```nginx
server {
    error_page 404 /404.html;
}
```
4. **Directivas del bloque Location** El bloque `location {}` define reglas de manejo de solicitudes basadas en patrones de URL. 
- **`location`** : Especifica cómo se procesan las solicitudes que coinciden con un patrón de URL específico. Los patrones pueden ser exactos o utilizar expresiones regulares.
Para un patrón exacto:


```nginx
location = /about {
    return 200 "Página sobre nosotros";
}
```
Para un patrón que coincide con cualquier subruta que comience con `/images`:

```nginx
location /images/ {
    root /var/www/media;
}
```

Para una expresión regular:


```nginx
location ~* \.(jpg|jpeg|png|gif)$ {
    root /var/www/media;
}
```
 
- **`proxy_pass`** : Utilizada dentro de un bloque `location`, esta directiva define un proxy inverso, donde las solicitudes se redirigen a un servidor diferente.

```nginx
location /api/ {
    proxy_pass http://backend_server;
}
```
5. **Directivas de proxy inverso y balanceo de carga** 
NGINX también se utiliza comúnmente como proxy inverso y balanceador de carga.
 
- **`proxy_set_header`** : Configura los encabezados HTTP que NGINX pasará al servidor backend.

```nginx
location /api/ {
    proxy_set_header Host $host;
    proxy_pass http://backend_server;
}
```
 
- **`upstream`** : Define un grupo de servidores backend que se utilizarán para balanceo de carga.

```nginx
upstream backend_servers {
    server backend1.example.com;
    server backend2.example.com;
}

server {
    location / {
        proxy_pass http://backend_servers;
    }
}
```
6. **Directivas de seguridad** 
NGINX tiene muchas directivas relacionadas con la seguridad.
 
- **`ssl_certificate`**  y ****`ssl_certificate`**  y `ssl_certificate_key`** : Se utilizan para habilitar SSL/TLS en un servidor y proporcionar el certificado y la clave privada.

```nginx
server {
    listen 443 ssl;
    ssl_certificate /etc/nginx/ssl/nginx.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx.key;
}
```
 
- **`deny`**  y ****`deny`**  y `allow`** : Estas directivas controlan el acceso a ciertos recursos basados en la dirección IP.

```nginx
location /admin/ {
    deny all;
    allow 192.168.1.0/24;
}
```
7. **Directivas de caché** 
NGINX es muy eficaz para servir contenido en caché.
 
- **`proxy_cache_path`** : Define la ruta y los parámetros de almacenamiento en caché.

```nginx
http {
    proxy_cache_path /data/nginx/cache levels=1:2 keys_zone=my_cache:10m max_size=10g;
}
```
 
- **`proxy_cache`** : Habilita el uso de caché para las solicitudes proxy.

```nginx
location /api/ {
    proxy_cache my_cache;
    proxy_pass http://backend_server;
}
```

### Resumen de las directivas más importantes: 
| Directiva | Descripción | 
| --- | --- | 
| worker_processes | Define el número de procesos de trabajo | 
| error_log | Ubicación y nivel de severidad de los logs de error | 
| client_max_body_size | Tamaño máximo del cuerpo de las solicitudes | 
| keepalive_timeout | Tiempo que una conexión HTTP permanece abierta | 
| listen | Puerto y dirección en la que escucha el servidor | 
| server_name | Nombre del servidor virtual | 
| root | Directorio raíz del servidor | 
| location | Define reglas de procesamiento de solicitudes en base a patrones de URL | 
| proxy_pass | Redirige solicitudes a un servidor backend | 
| upstream | Configura un grupo de servidores backend para balanceo de carga | 
| ssl_certificate | Habilita SSL y especifica el certificado | 
| proxy_cache_path | Configura la ruta y parámetros del caché | 

Estas directivas son solo una muestra de la vasta capacidad de configuración de NGINX, y pueden combinarse para manejar una variedad de casos de uso.
