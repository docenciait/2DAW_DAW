### Lab propuesto

Configuración Completa de un Sitio en NGINX para `cars.mydomain.com`
#### Objetivo 
Configurar un servidor NGINX para el dominio `cars.mydomain.com`, aplicando múltiples configuraciones como múltiples servernames, redirecciones a locations que retornen mensajes HTTP, configuraciones SSL, autenticación y logs, y realizar pruebas de cada configuración usando un navegador web y `curl`.
#### Requerimientos 
 
1. **Múltiples Servernames y Configuración del Puerto 80** : 
  - Configurar NGINX para que el dominio `cars.mydomain.com` y variantes como `www.cars.mydomain.com`, `app.cars.mydomain.com` respondan en el puerto 80.
 
  - Añadir una redirección desde `www.cars.mydomain.com` a `cars.mydomain.com`.
 
2. **Locations con Mensaje Personalizado y Código 200** : 
  - Configurar las siguientes `locations` para devolver un mensaje personalizado en texto plano (`Content-Type: text/plain`) con código de respuesta HTTP `200`: 
    - `/hello`: Retorna el mensaje "Welcome to Cars!"
 
    - `/status`: Retorna el mensaje "Service is running!"
 
3. **Locations con Acceso a Archivos HTML Internos** : 
  - Crear dos subdirectorios dentro de la carpeta raíz del sitio: 
    - `/info`: que apunte a `/var/www/cars/info` y contenga un archivo `index.html`.
 
    - `/docs`: que apunte a `/var/www/cars/docs` y contenga un archivo `index.html`.
 
  - Configurar `location` para cada uno de estos directorios, de forma que, al acceder desde el navegador o `curl`, se sirvan los archivos HTML almacenados en cada carpeta.
 
4. **Habilitar SSL en el Sitio** : 
  - Configurar HTTPS para el dominio `cars.mydomain.com`.

  - Crear o simular un certificado SSL (por ejemplo, con un certificado autofirmado para pruebas).
 
  - Asegurarse de que, al acceder a `https://cars.mydomain.com`, se redirija automáticamente a HTTPS si el acceso inicial fue por HTTP.
 
5. **Implementar Autenticación en el Sitio** : 
  - Habilitar autenticación básica para las rutas `/info` y `/docs`.
 
  - Crear un usuario de prueba con contraseña en un archivo `.htpasswd` para proteger estos directorios.
 
6. **Configurar Log para el Sitio** : 
  - Configurar un archivo de log específico para `cars.mydomain.com` para registrar accesos y errores.
 
  - Los registros deben incluir al menos:
    - IP de origen

    - Fecha y hora del acceso

    - URI solicitado

    - Código de respuesta
 
7. **Pruebas con Navegador y `curl`** : 
  - Realizar pruebas de todas las configuraciones usando un navegador web y el comando `curl`.
 
  - Pruebas específicas: 
    - Acceso a cada `location` (`/hello`, `/status`, `/info`, `/docs`) tanto desde navegador como desde `curl`.
 
    - Verificar mensajes y códigos de respuesta correctos para cada `location`.

    - Comprobar la redirección HTTP a HTTPS.
 
    - Verificar el funcionamiento de la autenticación en `/info` y `/docs`.

    - Asegurarse de que los logs reflejen correctamente cada prueba realizada.

    - Mostrar los mensajes de logs para verificar que cada acceso queda registrado en el archivo de log.

#### Criterios de Evaluación 

- Configuración correcta de cada servername y redirección en el puerto 80.
 
- Funcionalidad correcta de cada `location` y respuesta HTTP `200` con los mensajes personalizados.
 
- Acceso exitoso a archivos HTML en `info` y `docs`.

- Implementación de SSL con redirección automática y funcionamiento de autenticación básica.

- Configuración de logs con entradas correspondientes a cada prueba realizada.
 
- Validación correcta mediante navegador y `curl`.
