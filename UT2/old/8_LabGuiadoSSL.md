# Práctica 2
## Configurar SSL

## Método 1

Para agregar HTTPS a un sitio en NGINX, necesitas obtener un certificado SSL/TLS y configurar tu servidor NGINX para usarlo. A continuación te explico cómo hacerlo paso a paso usando **Let's Encrypt**  para obtener un certificado SSL gratuito, junto con la herramienta `Certbot` para facilitar el proceso.
### Paso 1: Instalar Certbot 

Certbot es una herramienta que automatiza la obtención y renovación de certificados SSL de Let's Encrypt.
 
1. Actualiza los paquetes de tu sistema:


```bash
sudo apt update
```
 
2. Instala Certbot y el plugin de NGINX:


```bash
sudo apt install certbot python3-certbot-nginx
```

### Paso 2: Obtener un certificado SSL con Let's Encrypt 

Ahora, puedes usar Certbot para solicitar un certificado SSL para tu dominio.
 
1. Ejecuta Certbot con el siguiente comando:


```bash
sudo certbot --nginx
```
 
2. Certbot te pedirá algunas opciones, como:

  - El dominio para el cual deseas obtener el certificado (asegúrate de que el dominio esté correctamente configurado en tu archivo de configuración de NGINX).

  - Si deseas redirigir todo el tráfico HTTP a HTTPS (opción recomendada). Selecciona "Sí" si deseas forzar HTTPS en todo el sitio.

Certbot configurará automáticamente NGINX para que utilice HTTPS y también agregará las redirecciones necesarias.

### Paso 3: Verificar la configuración de NGINX 

Después de que Certbot termine, el archivo de configuración de tu sitio en NGINX debería verse similar a este:


```nginx
server {
    listen 80;
    server_name midominio.com www.midominio.com;

    # Redirección de HTTP a HTTPS
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name midominio.com www.midominio.com;

    root /var/www/misitio;
    index index.html;

    # Configuración de SSL
    ssl_certificate /etc/letsencrypt/live/midominio.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/midominio.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

### Explicación de las secciones: 
 
1. **Redirección HTTP a HTTPS:** 
El primer bloque redirige todo el tráfico HTTP (puerto 80) a HTTPS (puerto 443).
 
2. **Configuración HTTPS:** 
El segundo bloque configura el servidor para escuchar en el puerto 443 con SSL habilitado: 
  - `ssl_certificate` y `ssl_certificate_key`: Especifican los archivos del certificado y la clave privada generados por Let's Encrypt.
 
  - `include /etc/letsencrypt/options-ssl-nginx.conf`: Este archivo incluye configuraciones de seguridad recomendadas.
 
  - `ssl_dhparam`: Archivo de parámetros Diffie-Hellman para mayor seguridad.

### Paso 4: Recargar la configuración de NGINX 

Verifica que no haya errores en la configuración con:


```bash
sudo nginx -t
```

Si todo está correcto, recarga la configuración de NGINX:


```bash
sudo systemctl reload nginx
```

### Paso 5: Probar HTTPS 
Abre un navegador y navega a `https://midominio.com`. Si todo está correctamente configurado, deberías ver que el sitio ahora usa HTTPS y tu navegador mostrará un candado indicando que la conexión es segura.
### Paso 6: Renovación automática del certificado 

Los certificados de Let's Encrypt tienen una validez de 90 días, pero Certbot puede configurarse para renovarlos automáticamente. Esto ya debería estar habilitado al instalar Certbot. Para verificar que la renovación automática está configurada, puedes ejecutar un comando de prueba:


```bash
sudo certbot renew --dry-run
```

Este comando simula el proceso de renovación y te informará si hubo algún problema.


---


Con estos pasos, tu sitio estará protegido con HTTPS y redirigirá automáticamente las conexiones HTTP a HTTPS para asegurar que todo el tráfico esté cifrado.

## Método 2


Además de usar **Let's Encrypt**  y Certbot, hay otras formas de agregar SSL a un sitio NGINX. A continuación te explico dos métodos alternativos:1. **Usar un certificado SSL comprado a una autoridad certificadora (CA)** Si prefieres no usar Let's Encrypt o necesitas un certificado más personalizado (como un certificado de validación extendida o de una marca específica), puedes comprar un certificado SSL de una autoridad certificadora (CA) como **DigiCert** , **Comodo** , **GoDaddy** , entre otras. Los pasos generales son los siguientes:
#### Paso 1: Generar una Solicitud de Firma de Certificado (CSR) 
 
1. Genera una clave privada:


```bash
sudo openssl genrsa -out /etc/nginx/ssl/midominio.key 2048
```
 
2. Genera un CSR (Certificate Signing Request):


```bash
sudo openssl req -new -key /etc/nginx/ssl/midominio.key -out /etc/nginx/ssl/midominio.csr
```

Durante este proceso, se te pedirá que ingreses la información del dominio, como el nombre común (dominio), la organización y otros detalles.
 
3. Envía el archivo `.csr` a la autoridad certificadora de tu elección cuando realices la compra del certificado.

#### Paso 2: Recibir e instalar el certificado 

Después de la validación, la CA te enviará varios archivos:
 
- El certificado del dominio (generalmente con la extensión `.crt`).

- El certificado intermedio y raíz, que conecta tu certificado con la CA.
Coloca estos archivos en el servidor, por ejemplo, en `/etc/nginx/ssl/`.
#### Paso 3: Configurar NGINX con el certificado 

Modifica el archivo de configuración de tu sitio en NGINX para usar el certificado:


```nginx
server {
    listen 443 ssl;
    server_name midominio.com www.midominio.com;

    root /var/www/misitio;
    index index.html;

    # Configuración de SSL con el certificado comprado
    ssl_certificate /etc/nginx/ssl/midominio.crt;
    ssl_certificate_key /etc/nginx/ssl/midominio.key;
    ssl_trusted_certificate /etc/nginx/ssl/cadena-certificados.crt;

    location / {
        try_files $uri $uri/ =404;
    }
}

server {
    listen 80;
    server_name midominio.com www.midominio.com;

    # Redirigir HTTP a HTTPS
    return 301 https://$host$request_uri;
}
```
 
- `ssl_certificate`: Apunta al archivo `.crt` del certificado del dominio.
 
- `ssl_certificate_key`: Apunta a la clave privada generada en el Paso 1.
 
- `ssl_trusted_certificate`: Apunta al archivo que contiene los certificados intermedios y raíz que te proporciona la CA.

#### Paso 4: Verificar y recargar NGINX 

Verifica la configuración de NGINX:


```bash
sudo nginx -t
```

Si todo está correcto, recarga la configuración de NGINX:


```bash
sudo systemctl reload nginx
```

## Método 3

2. **Usar un certificado auto-firmado (self-signed)** 

Este método es útil para entornos de desarrollo o pruebas internas. Un certificado auto-firmado no está respaldado por una autoridad certificadora, por lo que los navegadores mostrarán advertencias de seguridad, pero aún proporcionará encriptación SSL.

#### Paso 1: Generar un certificado auto-firmado 
 
1. Genera una clave privada:


```bash
sudo openssl genpkey -algorithm RSA -out /etc/nginx/ssl/selfsigned.key
```
 
2. Genera el certificado auto-firmado (válido por 365 días):


```bash
sudo openssl req -new -x509 -key /etc/nginx/ssl/selfsigned.key -out /etc/nginx/ssl/selfsigned.crt -days 365
```

Durante el proceso, se te pedirá ingresar información para el certificado, incluyendo el nombre común (dominio), organización, etc.

#### Paso 2: Configurar NGINX con el certificado auto-firmado 

Modifica tu configuración de NGINX para utilizar el certificado auto-firmado:


```nginx
server {
    listen 443 ssl;
    server_name midominio.com www.midominio.com;

    root /var/www/misitio;
    index index.html;

    # Configuración de SSL con el certificado auto-firmado
    ssl_certificate /etc/nginx/ssl/selfsigned.crt;
    ssl_certificate_key /etc/nginx/ssl/selfsigned.key;

    location / {
        try_files $uri $uri/ =404;
    }
}

server {
    listen 80;
    server_name midominio.com www.midominio.com;

    # Redirigir HTTP a HTTPS
    return 301 https://$host$request_uri;
}
```

#### Paso 3: Verificar y recargar NGINX 

Verifica que la configuración de NGINX esté correcta:


```bash
sudo nginx -t
```

Recarga NGINX para aplicar los cambios:


```bash
sudo systemctl reload nginx
```

#### Advertencia: 

Cuando visites el sitio con el certificado auto-firmado, verás una advertencia en el navegador indicando que la conexión no es segura. Esto se debe a que el certificado no está validado por una autoridad certificadora. Puedes ignorar la advertencia para propósitos de desarrollo.


---


Ambos métodos son alternativas válidas dependiendo de tu escenario. Si buscas simplicidad y un certificado gratuito para producción, Let's Encrypt es la mejor opción. Si necesitas un certificado personalizado o específico de una CA, entonces el primer método con una autoridad certificadora es el adecuado. Para pruebas locales o desarrollo, un certificado auto-firmado es una solución rápida.
