
### Práctica: Configuración de Autenticación Básica en NGINX 
**Objetivo:** 
En esta práctica, configurarás la autenticación básica en NGINX para proteger el acceso a un sitio web con un nombre de usuario y una contraseña. Aprenderás a usar archivos `.htpasswd` y configurar las directivas correspondientes en NGINX.

---


#### Requisitos: 

- Tener NGINX instalado y configurado en el servidor.
 
- Tener configurado al menos un sitio virtual en el archivo de configuración NGINX (por ejemplo, en `/etc/nginx/sites-available/`).
 
- Acceso al servidor con privilegios de administrador o `sudo`.


---


### Paso 1: Instalar herramientas necesarias 
Para crear el archivo `.htpasswd` que contendrá los usuarios y contraseñas cifradas, es necesario instalar la herramienta `apache2-utils` (si no está instalada ya):

```bash
sudo apt update
sudo apt install apache2-utils
```

### Paso 2: Crear el archivo de contraseñas 
Usa el comando `htpasswd` para crear un archivo de contraseñas cifradas. En este ejemplo, vamos a crear un usuario llamado `usuario1`.

```bash
sudo htpasswd -c /etc/nginx/.htpasswd usuario1
```
Te pedirá ingresar una contraseña para `usuario1`. Si deseas añadir más usuarios, puedes repetir el comando sin el parámetro `-c`:

```bash
sudo htpasswd /etc/nginx/.htpasswd usuario2
```

### Paso 3: Configurar NGINX 
Ahora, modifica el archivo de configuración del sitio que deseas proteger. Este archivo normalmente se encuentra en `/etc/nginx/sites-available/` o `/etc/nginx/nginx.conf` si es un único sitio. Asegúrate de que la configuración del bloque `server` se vea de la siguiente manera:

```nginx
server {
    listen 80;
    server_name midominio.com;

    location / {
        root /var/www/misitio;
        index index.html;

        # Configurar autenticación básica
        auth_basic "Área Protegida";
        auth_basic_user_file /etc/nginx/.htpasswd;
    }
}
```
 
- `auth_basic "Área Protegida";` define el mensaje que aparecerá en el cuadro de autenticación.
 
- `auth_basic_user_file /etc/nginx/.htpasswd;` apunta al archivo de contraseñas creado anteriormente.

### Paso 4: Verificar la configuración 

Antes de reiniciar NGINX, verifica que la configuración sea correcta usando el siguiente comando:


```bash
sudo nginx -t
```

Si no hay errores, recarga la configuración de NGINX:


```bash
sudo systemctl reload nginx
```

### Paso 5: Probar la autenticación 
Abre un navegador e ingresa la URL de tu sitio (por ejemplo, `http://midominio.com`). Deberías ver un cuadro de diálogo pidiéndote un nombre de usuario y contraseña. Introduce las credenciales que creaste en el archivo `.htpasswd` para acceder al sitio.

---


### Actividades adicionales: 
 
1. **Agregar HTTPS** : Configura NGINX para usar HTTPS y asegúrate de que la autenticación básica esté disponible solo a través de conexiones seguras.
 
2. **Limitar el acceso a una ubicación específica** : Modifica la configuración para que solo una carpeta (por ejemplo, `/admin`) requiera autenticación, dejando el resto del sitio accesible sin ella.
 
3. **Agregar restricciones por IP** : Investiga cómo limitar el acceso a determinadas direcciones IP además de la autenticación básica.


---


Con esta práctica habrás aprendido a proteger sitios web con autenticación básica en NGINX, asegurando que solo usuarios autorizados puedan acceder a los recursos.
