# NGINX como Web Server

![](img/nginx_request.png)

## Comandos NGINX

```bash


nginx -v                  #displays NGINX version details

nginx -s quit             #graceful shutdown (Note: This will exit the container)

nginx -s stop             #terminates all NGINX processes (Note: This will exit the container)

nginx -t                  #test configuration syntax and files

nginx -T                  #dumps the current running configurations

nginx -s reload           #reloads NGINX with new configuration


```

## RESUMEN CONFIGURACIÓN NGINX

![](img/nginx-contexts.png)

En general, los contextos y bloques se encuentran en una jerarquía lógica que sigue la construcción de una URL HTTP, como sigue:

- main and events : parámetros de inicio de nginx
- http : parámetros de alto nivel - logging, data types, timers, include files
- server : parámetros de servidores virtuales - listen port, hostname, access log, include files
- location : URL path, object type

## NGINX Host based Routing

```bash

server {
    
    listen 80 default_server;        # Listening on port 80 on all IP addresses on this machine

    server_name www2.example.com;   # Set hostname to match in request

    access_log  /var/log/nginx/www2.example.com.log main; 
    error_log   /var/log/nginx/www2.example.com_error.log info; 

    location / {
        
        default_type text/html;      
        return 200 "Congrats, you have reached www2.example.com, the base path /\n";

    }

}


```

## NGINX Path based Routing

Se configura el `/etc/hosts`:
```bash
 vi /etc/hosts

 # NGINX Basics hostnames for labs
 127.0.0.1 localhost www.example.com www2.example.com cafe.example.com

```

Configuración de un servidor virtual: `/etc/nginx/sites-available/cafe.example.com`:

```bash


server {
    
    listen 80;      # Listening on port 80 on all IP addresses on this machine

    server_name cafe.example.com;   # Set hostname to match in request

    access_log  /var/log/nginx/cafe.example.com.log main; 
    error_log   /var/log/nginx/cafe.example.com_error.log info; 

    location / {
        
        default_type text/html;
        return 200 "Congrats, you have reached cafe.example.com, path $uri\n";
    }
    
    location /coffee {
        
        default_type text/html;
        return 200 "Caffiene relief from cafe.example.com, path $uri\n";
    }
    
    location /tea {
        
        default_type text/html;
        return 200 "Green Tea from cafe.example.com, path $uri\n";
    }
    
    location /hours {
        
        default_type text/html;
        return 200 "We are open:\nSun 6am-3pm\nMon Closed\nTue 6am-3pm\nWed 6am-3pm\nThurs 6am-3pm\nFri 6am-3pm\nSat 6am-3pm\nSun 6am-3pm\nat cafe.example.com, path $uri\n";
    }
    
    location /hours/closed {
        
        default_type text/html;
        return 200 "Sorry - We are Closed on Tuesdays\nat cafe.example.com, path $uri\n";
    }

}


```

Recordar que siempre se recarga la configuración: 

`nginx -s reload`

```bash

curl 127.0.0.1 -H "Host: cafe.example.com"
curl 127.0.0.1/coffee -H "Host: cafe.example.com"
curl 127.0.0.1/tea -H "Host: cafe.example.com"
curl 127.0.0.1/hours -H "Host: cafe.example.com"
curl 127.0.0.1/hours/closed -H "Host: cafe.example.com"

```

Y en las salidas debe salir:

```bash

 ##Sample outputs##
 Congrats, you have reached cafe.example.com, path /

 Caffeine relief from cafe.example.com, path /coffee

 Green Tea from cafe.example.com, path /tea

 We are open:
 Sun 6am-3pm
 Mon Closed
 Tue 6am-3pm
 Wed 6am-3pm
 Thurs 6am-3pm
 Fri 6am-3pm
 Sat 6am-3pm
 Sun 6am-3pm
 at cafe.example.com, path /hours

 Sorry - We are Closed on Tuesdays
 at cafe.example.com, path /hours/closed


```

> NOTA: Ha añadido una variable NGINX, $uri a la directiva return, para devolver la ruta URI de la petición recibida. Hay muchas más variables NGINX que pueden ser usadas como esta. Vamos a utilizar más de estas $variables NGINX para construir una página de depuración simple, que se hará eco de algunos de la información importante que pueda necesitar cuando se trabaja / prueba NGINX:

```bash


    location /debug {      # Used for testing, returns IP, HTTP, NGINX info 
        
        return 200 "NGINX Debug/Testing URL from cafe.example.com\n\nIP Parameters: ClientIP=$remote_addr, NginxIP=$server_addr, UpstreamIP=$upstream_addr, Connection=$connection\n\nHTTP Parameters: Scheme=$scheme, Host=$host, URI=$request, Args=$args, Method=$request_method, UserAgent=$http_user_agent, RequestID=$request_id\n\nSystem Parameters: Time=$time_local, NGINX version=$nginx_version, NGINX PID=$pid\n\n";
    }
    

```

Recuerda recargar `nginx -s reload` y probar: `curl 127.0.0.1/debug -H "Host: cafe.example.com"`

Deberías ver una respuesta del bloque de ubicación /debug, con las $variables de NGINX rellenadas con datos para cada petición a http://cafe.example.com/debug :

```bash

#Sample output
NGINX Debug/Testing URL from cafe.example.com

IP Parameters: ClientIP=172.18.0.2, NginxIP=172.18.0.2, UpstreamIP=, Connection=8

HTTP Parameters: Scheme=http, Host=cafe.example.com, URI=GET /debug HTTP/1.1, Args=, Method=GET, UserAgent=curl/8.5.0, RequestID=7a29149e6a687bb52c6901dfc19079f8

System Parameters: Time=31/Jan/2024:18:53:11 +0000, NGINX version=1.25.3, NGINX PID=89


```

## NGINX Static HTML pages

Vamos a probar con algunos archivos HTML e imágenes. Vas a crear otro sitio web nuevo, `coches.ejemplo.com`, que tendrá 3 archivos HTML nuevos y algunas imágenes de los coches, que representarán 3 automóviles de altas prestaciones: el Lexus RCF, el Nissan GTR y el Acura NSX. 

Cada coche tendrá su propia URL y bloque de ubicación, y los correspondientes archivos .html y .jpg en el disco. El directorio por defecto para servir contenido HTML con NGINX es `/usr/share/nginx/html`, por lo que lo utilizarás como raíz de tu nuevo sitio web. 

En primer lugar, utilizando vi o un editor de texto, actualiza tu archivo local DNS resolver hosts, normalmente etc/hosts, para añadir estos nombres de host FQDN utilizados para estos ejercicios de laboratorio. `cars.example.com`:

```bash
 vi /etc/hosts

 # NGINX Basics hostnames for labs
 127.0.0.1 localhost www.example.com www2.example.com cafe.example.com cars.example.com

```

Luego configuramos el site: `cars.example.com`:

```bash

server {
    
    listen 80;      # Listening on port 80 on all IP addresses on this machine

    server_name cars.example.com;   # Set hostname to match in request

    access_log  /var/log/nginx/cars.example.com.log main; 
    error_log   /var/log/nginx/cars.example.com_error.log info;

    root /usr/share/nginx/html;      # Set the root folder for the HTML and JPG files

    location / {           
        default_type text/html;
        return 200 "Let's go fast, you have reached cars.example.com, path $uri\n";
    }

    location /gtr {
        try_files $uri $uri.html;     # Look for a filename that matches the URI requested
    }
    
    location /nsx {
        try_files $uri $uri.html;
    }
    
    location /rcf {
        try_files $uri $uri.html;
    }

} 

```

Probando:

```bash
curl cars.example.com/gtr
curl cars.example.com/nsx
curl cars.example.com/rcf

```