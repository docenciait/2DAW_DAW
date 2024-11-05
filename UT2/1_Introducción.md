# UNIDAD 2: SERVIDORES WEB

## Tabla de Contenidos

[Introducción](#introducción)
[NGINX Configuración General](#nginx-configuración-general)


# Introducción

Con la evolución y el acceso libre a Internet, uno de los principales alicientes que han surgido es la publicación de páginas web donde se pueden almacenar unos contenidos bastante atractivos para nosotros y que, al mismo tiempo, pueden ser consultados desde cualquier del mundo para todos.

Cabe decir que, con la popularización de Internet, tanto empresas como usuarios han visto la necesidad de establecer un punto desde donde anunciar sus productos, o bien, a título particular, dar publicidad a las aficiones o capacidades personales mediante la publicación de páginas web.

Las páginas web, en su mayoría en formato HTML, requieren ser alojadas en máquinas que dispongan de espacio en disco para almacenar archivos HTML, imágenes, bloques de código o archivos de vídeo en directorios específicos y, al mismo tiempo, deben ser capaces de entender todo tipo de extensión de los archivos que son enviados en ambos sentidos de la comunicación.

![](img/servidor1.jpg)

Paralelamente, no podemos dejar de lado la importancia de las medidas de seguridad ante los peligros existentes en Internet. Para ello, las páginas deberán estar diseñadas considerando la incorporación de protocolos de comunicación seguros como, por ejemplo, los desarrollados con el protocolo seguro de transferencia de hipertexto (HTTPS, Hyper Text Transfer Protocol secure) que utilizan claves y estrategias de cifrado propias de las herramientas del protocolo de capa de conexión segura (SSL, secure sockets layer).

Las máquinas que alojan las páginas web reciben la categoría de servidores web. Desde el punto de vista de los servidores, los requerimientos más relevantes son el espacio de disco necesario para poder almacenar la estructura de la página web y una buena conexión de red para que el consumo de la unidad de procesamiento central (CPU, central processing unit ) sea bastante bajo.

El funcionamiento de los servidores web es especial ya que, como si se tratara de un diente de sierra, tienen consumos de recursos puntuales porque podemos estar un tiempo sin peticiones y, de repente, tener una avalancha de peticiones. Esto hace que los servidores web suelan tener un número bajo de procesos en espera. A medida que resultan necesarios, se van arrancando nuevos.

Cabe decir que no todas las peticiones consumen el mismo, y, por ejemplo, aquellas páginas web que ejecuten programas de interacción con el usuario o requieran cifrado (HTTPS) consumen más recursos que otras páginas web con menos interacción.

**¿Qué es un servidor web?**

Los servidores web sirven para almacenar contenidos de Internet y facilitar su disponibilidad de forma constante y segura. Cuando visitas una página web desde tu navegador, es en realidad un servidor web el que envía los componentes individuales de dicha página directamente a tu ordenador. Esto quiere decir que para que una página web sea accesible en cualquier momento, el servidor web debe estar permanentemente online.

Toda página accesible en Internet necesita un servidor especial para sus contenidos web. A menudo, las grandes empresas y organizaciones cuentan con un servidor web propio para disponer sus contenidos en Intranet e Internet. Sin embargo, la mayoría de administradores recurren a los centros de datos de proveedores de alojamiento web para sus proyectos. Independientemente de si tienes un servidor web propio o de si alquilas uno externo, siempre necesitarás un software para gestionar los datos de tu página y mantenerla actualizada. En este sentido, tienes la posibilidad de elegir entre varias soluciones de software para servidores web diseñadas para diferentes aplicaciones y sistemas operativos.

**Tecnología de servidores web**

Principalmente, el software de un servidor HTTP es el encargado de proporcionar los datos para la visualización del contenido web.

Para abrir una página web, el usuario solo tiene que escribir el URL correspondiente en la barra de direcciones de su navegador web. El navegador envía una solicitud al servidor web, quien responde, por ejemplo, entregando una página HTML. Esta puede estar alojada como un documento estático en el host o ser generada de forma dinámica, lo que significa que el servidor web tiene que ejecutar un código de programa (p. ej., Java o PHP) antes de tramitar su respuesta.

El navegador interpreta la respuesta, lo que suele generar automáticamente más solicitudes al servidor a propósito de, por ejemplo, imágenes integradas o archivos CSS (hojas de estilos).

![](img/tecnologias.png)

El protocolo utilizado para la transmisión es HTTP (o su variante cifrada HTTPS), que se basa, a su vez, en los protocolos de red IP y TCP (y muy rara vez en UDP). 

***Un servidor web puede entregar los contenidos simultáneamente a varios ordenadores o navegadores web. La cantidad de solicitudes (requests) y la velocidad con la que pueden ser procesadas depende, entre otras cosas, del hardware y la carga (número de solicitudes) del host.***

Sin embargo, la complejidad del contenido también juega un papel importante: 

***los contenidos web dinámicos necesitan más recursos que los contenidos estáticos.***


La selección del equipo adecuado para el servidor y la decisión de si este debe ser dedicado, virtual o en la nube, se debe hacer pensando siempre en evitar sobrecargas en el servidor. 

Aunque se haya encontrado un servidor web que se adapta perfectamente a las necesidades del proyecto, siempre se corre el riesgo de que se presenten fallos en él como consecuencia de imprecisiones técnicas o cortes de energía en el centro de datos del host. 

Aunque no es muy frecuente, durante un período de inactividad de este tipo (downtime), la web no estará disponible.

**Otras funciones de los servidores web**

Aunque su principal función es la transferencia de contenido web, muchos programas de servidor web ofrecen características adicionales:

| **Seguridad**  |  **Cifrado de la comunicación entre el servidor web y el cliente vía HTTPS** |
|---|---|
|  Autenticación del usuario |  Autenticación HTTP para áreas específicas de una aplicación web |
|  Redirección |  Redirección de una solicitud de documento por medio de Rewrite Engine |
 | Redirección |  Almacenamiento en caché de documentos dinámicos para la respuesta eficiente de solicitudes y para evitar una sobrecarga del servidor web |
 | Asignación de cookies |  Envío y procesamiento de cookies HTTP |

Además del software del servidor, un host puede contener otro tipo de programas, como por ejemplo un servidor FTP para la carga de archivos o un servidor de base de datos para contenidos dinámicos. 

En general, existen diferentes tipos de servidores web que pueden ser utilizados para numerosos propósitos, por ejemplo, los servidores de correo, los servidores de juegos o los servidores proxy.

### Servidores web: Apache vs Nginx

Cuando vamos a poner en marcha un servidor web, lo primero que necesitamos es utilizar un sistema operativo sobre el cual vamos a ejecutar los diferentes servicios, sistema operativo que en más del 95% de las ocasiones suele ser un sistema Linux, así como un software que se encargue de la gestión de las bases de datos, MySQL habitualmente, y un software para gestionar el contenido dinámicos de las webs, que suele ser PHP. Además de este software esencial, otra de las partes más importantes del servidor suele ser la elección del servidor web, y aquí es donde entran las dudas.

Cuando buscamos montar una web podemos elegir una gran cantidad de servidores web diferentes, desde Apache y Nginx, los más conocidos y utilizados con más de un 85% de uso entre ambos, hasta otros servidores menos conocidos como Microsoft IIS (si usamos un servidor Windows), LiteSpeed, Node.js, etc.

Los dos servidores más utilizados para montar páginas web hoy en día son Apache y Nginx, sin embargo, es imposible decir que uno es mejor que otro ya que cada uno de ellos tiene sus propias fortalezas y debilidades y puede mejorar mejor bajo ciertas circunstancias o simplemente ser más sencillo de utilizar.

**Nginx está orientado a mejorar el rendimiento, soportando mayores cargas de tráfico y usuarios que Apache** ([Problema C10K](https://es.wikipedia.org/wiki/Problema_C10k)), además de ofrecer otras funcionalidades como hacer de proxy. En sus orígenes era especialmente eficiente ofreciendo contenido estático.

![](img/webservers.jpeg)

Después de ser lanzado, Nginx fue usado principalmente para servir archivos estáticos y como un balanceador de carga o proxy inverso en frente de instalaciones Apache.

Ejemplos de servicios de despliegue de páginas estáticas:

- Netlify
- Surge
- GitHub Pages
- GitLab Pages
- Firebase
- Vercel
- Neocities

Mientras evolucionaba la red, y la necesidad de exprimir hasta la última gota de la velocidad y eficiencia de uso de hardware con este, más sitios empezaron a reemplazar Apache con Nginx por completo, gracias a un software mucho más maduro.

![](img/wsusage.jpeg)

**Razones para usar Nginx**

- Es ligero: Nginx reduce el consumo de RAM.

- Es multiplataforma y fácil de instalar: La mayoría de las grandes distribuciones de GNU/Linux, tienen Nginx en sus repositorios.

- ¡Se puede usar junto a Apache!: Sí, como lo lees, algunas empresas solo usan Nginx para servir contenido estático y Apache para el contenido dinámico.

- Caché : Puedes usar Nginx como caché, con algo de configuración, permitiendo mejorar la eficiencia de tu aplicación sin tocar la programación de la misma.

- Balanceador de carga: Este servidor web puede funcionar como balanceador de carga, distribuyendo el tráfico entre varios servidores, permitiendo mayor escalabilidad.

- Soporte comunitario y profesional: Nginx, Inc está detrás del desarrollo de Nginx, además de la comunidad en general, permitiendo tener un soporte tanto profesional como comunitario.

- Compatibilidad con las aplicaciones web más populares: Nginx es compatible con una gran cantidad de CMS existentes en el mercado,  Go, Java, Node.js, PHP, Python, Ruby. Además como Proxy Inverso a .NET.

## PRÁCTICA PARA HACER CON AUTOEVALUACIÓN. INSTALACIÓN Y CONFIGURACIÓN DE UN SITIO WEB CON NGINX


DESPUÉS AUTOEVALUACIÓN:

- Configuración correcta del servidor web	=> 1 puntos
- Comprobación del correcto funcionamento del primer sitio web	=> 3 puntos
- Configuración correcta y comprobación del funcionamento de una segunda web =>	2 puntos
- Cuestiones finales => 2 puntos
- Se ha prestado especial atención al formato del documento, haciendo un correcto uso del lenguaje técnico	=> 2 puntos

---

# NGINX Configuración General


# Servidor Web NGINX

Nginx es un servidor web y proxy inverso altamente configurable y eficiente. Su configuración es flexible y modular, lo que permite adaptarlo a diferentes necesidades, desde servir sitios estáticos hasta manejar aplicaciones complejas a gran escala. La configuración de Nginx se organiza principalmente en bloques y directivas que se especifican en archivos de configuración, por defecto ubicados en `/etc/nginx/nginx.conf`.
Aquí te detallo cómo se estructura y qué significan las principales directivas y bloques de configuración.

1. **Estructura básica del archivo de configuración** 
El archivo de configuración de Nginx se compone de bloques de configuración (o contextos) y directivas. Las directivas definen acciones específicas, como establecer un valor o habilitar una característica. Los bloques permiten agrupar directivas que se aplican a contextos específicos.

#### Ejemplo de estructura básica: 


```nginx
user www-data;
worker_processes auto;
pid /run/nginx.pid;
events {
    worker_connections 1024;
}
http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    
    server {
        listen 80;
        server_name example.com;
        
        location / {
            root /var/www/html;
            index index.html index.htm;
        }
    }
}
```
- La directiva **http** es la configuración general y dentro podemos poner **server** con las configuraciones de los servidores que deseemos. No obstante, tener la directiva server en sites-available es mucho más cómodo.

2. **Contextos principales de Nginx** 
Nginx utiliza varios contextos o bloques que agrupan configuraciones específicas:
 
- **Main context** : Directivas aplicadas globalmente a todo Nginx, como la configuración de usuarios y procesos.
 
- **Events context** : Configura la forma en que Nginx maneja las conexiones, como el número máximo de conexiones concurrentes.
 
- **HTTP context** : Configura el servidor HTTP y las directivas relacionadas con la comunicación web.
 
- **Server context** : Define un servidor virtual que gestiona peticiones en un puerto específico.
 
- **Location context** : Define cómo se manejan las peticiones a una URL específica.

3. **Directivas comunes y bloques** **Main Context** 
Las directivas dentro del bloque principal configuran aspectos generales de Nginx y afectan a todas las instancias del servidor.
 
- `user`: Define el usuario bajo el cual Nginx ejecuta los procesos.

```nginx
user www-data;
```
 
- `worker_processes`: Establece cuántos procesos worker debe ejecutar Nginx. Puedes usar `auto` para que ajuste automáticamente según el número de núcleos de CPU.

```nginx
worker_processes auto;
```
 
- `pid`: Ubicación del archivo PID del proceso principal de Nginx.

```nginx
pid /run/nginx.pid;
```

**Events Context** 

Este bloque define cómo Nginx maneja las conexiones.
 
- `worker_connections`: Especifica el número máximo de conexiones simultáneas que cada proceso worker puede manejar.

```nginx
worker_connections 1024;
```

**HTTP Context** 

Este bloque engloba la configuración del protocolo HTTP, donde se pueden definir directivas relacionadas con el manejo de las peticiones HTTP.
 
- `include`: Permite incluir otros archivos de configuración.

```nginx
include /etc/nginx/mime.types;
```
 
- `default_type`: Define el tipo MIME predeterminado que se enviará en la respuesta si no se especifica otro.

```nginx
default_type application/octet-stream;
```
**Server Context** Dentro del bloque HTTP, el contexto `server` define un servidor virtual. Nginx puede tener múltiples bloques `server` para manejar diferentes dominios o puertos. 
- `listen`: Define en qué puerto escucha el servidor virtual. Por defecto, HTTP usa el puerto 80 y HTTPS el 443.

```nginx
listen 80;
```
 
- `server_name`: Especifica el nombre del servidor (o dominio) que Nginx debe manejar.

```nginx
server_name example.com;
```

**Location Context** El bloque `location` se utiliza para definir cómo Nginx responde a las solicitudes de determinadas rutas o URL. 
- `root`: Establece la raíz del documento, es decir, la carpeta en el sistema de archivos desde donde se servirán los archivos.

```nginx
root /var/www/html;
```
 
- `index`: Especifica el archivo predeterminado que Nginx buscará y servirá cuando se acceda a un directorio.

```nginx
index index.html index.htm;
```
4. **Directivas avanzadas** **Proxying y caching** 
Nginx se puede configurar como un proxy inverso para reenviar solicitudes a servidores de backend como servidores de aplicaciones (Node.js, PHP-FPM) u otros servicios.
 
- **proxy_pass** : Redirige las solicitudes a otro servidor.

```nginx
location /api/ {
    proxy_pass http://backend-server:8080;
}
```
 
- **proxy_cache_path** : Define la ruta y el tamaño máximo del caché.

```nginx
proxy_cache_path /data/nginx/cache levels=1:2 keys_zone=my_cache:10m max_size=10g;
```
**SSL/TLS** Nginx soporta SSL/TLS para servir contenido seguro. Las directivas relevantes están dentro del bloque `server` que escucha en el puerto 443. 
- `ssl_certificate` y `ssl_certificate_key`: Especifican los archivos de certificado y clave privada, respectivamente.

```nginx
ssl_certificate /etc/nginx/ssl/cert.pem;
ssl_certificate_key /etc/nginx/ssl/cert.key;
```
 
- `ssl_protocols`: Define qué versiones del protocolo SSL/TLS se admiten.

```nginx
ssl_protocols TLSv1.2 TLSv1.3;
```
**Gzip Compression** 
Nginx soporta la compresión de respuesta mediante Gzip, lo que mejora la velocidad de carga.
 
- `gzip`: Habilita o deshabilita la compresión Gzip.

```nginx
gzip on;
```
 
- `gzip_types`: Especifica los tipos de contenido que deben comprimirse.

```nginx
gzip_types text/plain text/css application/json;
```
**Rate Limiting y Control de tráfico** 
Nginx permite limitar la velocidad de las solicitudes para evitar la sobrecarga del servidor.
 
- `limit_req_zone`: Define una zona de memoria compartida para realizar el seguimiento de las solicitudes.

```nginx
limit_req_zone $binary_remote_addr zone=one:10m rate=1r/s;
```
 
- `limit_req`: Aplica la limitación de solicitudes en una ubicación o servidor específico.

```nginx
limit_req zone=one burst=5 nodelay;
```
5. **Bloques y Directivas útiles adicionales**  
- **Error Pages** : Personaliza las páginas de error.

```nginx
error_page 404 /404.html;
location = /404.html {
    root /var/www/html;
}
```
 
- **Access Log** : Configura la ruta y formato del archivo de registro de acceso.

```nginx
access_log /var/log/nginx/access.log;
```
6. **Conclusión** 

La configuración de Nginx es muy versátil, permitiendo personalizar desde simples servidores estáticos hasta configuraciones complejas de balanceo de carga y proxy inverso. Las directivas y bloques explicados aquí son solo una introducción; Nginx soporta muchos otros módulos y configuraciones específicas que pueden adaptarse a necesidades más complejas.

Si tienes alguna configuración específica que te gustaría profundizar, no dudes en preguntarme.


Para configurar varios sitios o ubicaciones dentro de un mismo dominio como mencionas, podemos usar directivas y bloques de `server` y `location` de Nginx. A continuación, te proporciono un ejemplo de cómo configurar Nginx para manejar las siguientes rutas: 
1. `www.mysite.com`
 
2. `www.mysite.com/site1`
 
3. `www.mysite.com/site2`
 
4. `www.mysite.com/site1/site12`

Cada ruta servirá diferentes directorios o archivos según tu estructura. La configuración a continuación está diseñada para manejar este tipo de rutas.

### Ejemplo de configuración de Nginx para varios sitios 


```nginx
# Configuración general
user www-data;
worker_processes auto;
pid /run/nginx.pid;
events {
    worker_connections 1024;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    
    # Habilitar compresión Gzip para mejorar rendimiento
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

    # Servidor principal para www.mysite.com
    server {
        listen 80;
        server_name www.mysite.com;

        # Root para el sitio principal www.mysite.com
        location / {
            root /var/www/mysite;  # Directorio raíz de www.mysite.com
            index index.html;
        }

        # Subsitio para www.mysite.com/site1
        location /site1 {
            alias /var/www/mysite/site1;  # Directorio para /site1
            index index.html;
        }

        # Subsitio para www.mysite.com/site1/site12
        location /site1/site12 {
            alias /var/www/mysite/site1/site12;  # Directorio para /site1/site12
            index index.html;
        }

        # Subsitio para www.mysite.com/site2
        location /site2 {
            root /var/www/mysite/site2;  # Directorio para /site2
            index index.html;
        }

        # Páginas de error personalizadas
        error_page 404 /404.html;
        location = /404.html {
            root /var/www/mysite;
        }
    }

    # Servidor adicional (puedes agregar más bloques server para otros dominios)
    # server {
    #     listen 80;
    #     server_name anotherdomain.com;
    #     # Configuración de otro dominio aquí...
    # }
}
```

### Explicación del ejemplo: 
 
1. **Directiva `listen 80`:**  El servidor escucha en el puerto 80 para tráfico HTTP.
 
2. **Directiva `server_name www.mysite.com`:**  Define el nombre de dominio que este bloque `server` manejará (en este caso, `www.mysite.com`).
 
3. **Directiva `root`:**  Define el directorio raíz desde donde Nginx servirá los archivos. Cada `location` tiene su propia raíz.
 
4. **Subsitios usando `location`:**  
  - `location /` es la ruta principal para `www.mysite.com`, sirviendo archivos desde `/var/www/mysite`.
 
  - `location /site1` sirve archivos desde `/var/www/mysite/site1` para las URLs que coincidan con `www.mysite.com/site1`.
 
  - `location /site1/site12` sirve archivos desde `/var/www/mysite/site1/site12` para la URL `www.mysite.com/site1/site12`.
 
  - `location /site2` sirve archivos desde `/var/www/mysite/site2` para la URL `www.mysite.com/site2`.
 
5. **Directiva `index`:**  Define el archivo que Nginx debe buscar cuando accede a un directorio, como `index.html`.

### Estructura de archivos esperada: 

Debes asegurarte de que la estructura de directorios en tu servidor coincida con lo que define la configuración de Nginx. Por ejemplo, deberías tener lo siguiente:


```bash
/var/www/mysite
    ├── index.html                 # Página principal para www.mysite.com
    ├── site1/
    │   ├── index.html             # Página para www.mysite.com/site1
    │   └── site12/
    │       └── index.html         # Página para www.mysite.com/site1/site12
    └── site2/
        └── index.html             # Página para www.mysite.com/site2
```

### ¿Cómo funciona Nginx para cada URL? 
 
- Cuando un usuario accede a `www.mysite.com`, Nginx sirve el archivo `index.html` desde `/var/www/mysite/`.
 
- Para `www.mysite.com/site1`, Nginx busca y sirve archivos desde `/var/www/mysite/site1/`.
 
- Para `www.mysite.com/site1/site12`, Nginx busca los archivos en `/var/www/mysite/site1/site12/`.
 
- Para `www.mysite.com/site2`, Nginx usa `/var/www/mysite/site2/`.
Esta configuración proporciona un enfoque modular y organizado para manejar varias rutas dentro del mismo dominio con facilidad. Si tienes más rutas o sitios para añadir, simplemente puedes replicar el uso de `location` con las rutas y directorios correspondientes.

Cuando trabajas con Nginx, una práctica común es separar las configuraciones de cada sitio en archivos individuales dentro del directorio `sites-available` y luego habilitar estos sitios mediante enlaces simbólicos en el directorio `sites-enabled`. A continuación te mostraré cómo configurar esto en la estructura de `sites-available` y cómo habilitar los sitios en `sites-enabled`.

### 1. Estructura de archivos 

Nginx usa dos directorios principales para gestionar los sitios web:
 
- **`/etc/nginx/sites-available/`** : Aquí se guardan los archivos de configuración de cada sitio, pero no están habilitados aún.
 
- **`/etc/nginx/sites-enabled/`** : Aquí se colocan enlaces simbólicos (`symlink`) a los archivos de configuración en `sites-available`, lo que habilita los sitios.

2. Configuración del archivo para `mysite.com`Vamos a crear un archivo de configuración en `/etc/nginx/sites-available/mysite.com` para el dominio `www.mysite.com` con las rutas que mencionaste.Archivo de configuración para `mysite.com`Crea un archivo en `/etc/nginx/sites-available/mysite.com` con el siguiente contenido:

```nginx
server {
    listen 80;
    server_name www.mysite.com;

    # Root para el sitio principal www.mysite.com
    location / {
        root /var/www/mysite;  # Directorio raíz de www.mysite.com
        index index.html;
    }

    # Subsitio para www.mysite.com/site1
    location /site1 {
        alias /var/www/mysite/site1;  # Directorio para /site1
        index index.html;
    }

    # Subsitio para www.mysite.com/site1/site12
    location /site1/site12/ {
        alias /var/www/mysite/site1/site12;  # Directorio para /site1/site12
        index index.html;
    }

    # Subsitio para www.mysite.com/site2
    location /site2/ {
        root /var/www/mysite/site2;  # Directorio para /site2
        index index.html;
    }

    # Páginas de error personalizadas
    error_page 404 /404.html;
    location = /404.html {
        root /var/www/mysite;
    }

    # Logs de acceso y error
    access_log /var/log/nginx/mysite_access.log;
    error_log /var/log/nginx/mysite_error.log;
}
```
Este archivo es similar al ejemplo anterior, pero está listo para ser habilitado mediante el mecanismo de `sites-available` y `sites-enabled`.

### 3. Habilitar el sitio con un enlace simbólico 

Una vez que hayas creado el archivo de configuración en `sites-available`, el siguiente paso es habilitar el sitio creando un enlace simbólico en `sites-enabled`.
Para hacerlo, sigue estos pasos:
 
1. **Crear el enlace simbólico:** Ejecuta este comando para crear un enlace simbólico que apunte a la configuración del sitio en `sites-enabled`:

```bash
sudo ln -s /etc/nginx/sites-available/mysite.com /etc/nginx/sites-enabled/
```
 
2. **Verificar la configuración de Nginx:** 
Es importante verificar que la configuración no tenga errores antes de recargar Nginx:


```bash
sudo nginx -t
```

Si todo está bien, verás un mensaje indicando que la configuración es exitosa.
 
3. **Recargar Nginx para aplicar la configuración:** 
Si la verificación es correcta, recarga Nginx para aplicar los cambios:


```bash
sudo systemctl reload nginx
```

### 4. Estructura de directorios esperada 

Tu servidor debe tener una estructura de directorios similar a esta para que Nginx pueda servir correctamente los diferentes sitios:


```bash
/var/www/mysite
    ├── index.html                 # Página principal para www.mysite.com
    ├── site1/
    │   ├── index.html             # Página para www.mysite.com/site1
    │   └── site12/
    │       └── index.html         # Página para www.mysite.com/site1/site12
    └── site2/
        └── index.html             # Página para www.mysite.com/site2
```

### 5. Comprobación final 
 
- **Acceso a `www.mysite.com`:**  Debe servir el contenido desde `/var/www/mysite/index.html`.
 
- **Acceso a `www.mysite.com/site1`:**  Debe servir el contenido desde `/var/www/mysite/site1/index.html`.
 
- **Acceso a `www.mysite.com/site1/site12`:**  Debe servir el contenido desde `/var/www/mysite/site1/site12/index.html`.
 
- **Acceso a `www.mysite.com/site2`:**  Debe servir el contenido desde `/var/www/mysite/site2/index.html`.

### 6. Deshabilitar un sitio 
Si en algún momento quieres deshabilitar este sitio, simplemente elimina el enlace simbólico en `sites-enabled`:

```bash
sudo rm /etc/nginx/sites-enabled/mysite.com
```

Luego recarga Nginx para que los cambios surtan efecto:


```bash
sudo systemctl reload nginx
```
Con esta configuración, puedes manejar diferentes subsitios dentro de un mismo dominio, y habilitar o deshabilitar sitios de manera sencilla usando el mecanismo de `sites-available` y `sites-enabled`.

---