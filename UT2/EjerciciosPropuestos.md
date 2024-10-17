

### Propuesta de Práctica: Configuración de NGINX con HTTPS, Autenticación Básica y HTTP/2 
**Objetivo de la Práctica:** 
El objetivo de esta práctica es que los estudiantes configuren un servidor web NGINX para habilitar tres funcionalidades clave: autenticación básica para proteger partes del sitio web, implementación de HTTPS para garantizar la seguridad de las conexiones, y soporte para HTTP/2 para mejorar el rendimiento de la página.

---


### Requisitos: 

1. Conocimientos previos de configuración básica de servidores web con NGINX.

2. Un dominio que apunte al servidor.

3. Acceso administrativo (root o sudo) al servidor.

4. Instalación de NGINX en el servidor.
 
5. Instalación de herramientas auxiliares como `apache2-utils` para la autenticación y `certbot` para el manejo de certificados SSL.


---


### Tareas: 

#### Tarea 1: Configurar Autenticación Básica 
 
1. **Objetivo:**  Proteger una sección del sitio web (por ejemplo, `/admin`) utilizando autenticación básica.
 
2. **Requisitos:**  
  - El estudiante debe crear un archivo `.htpasswd` utilizando `apache2-utils` y configurar NGINX para proteger una ruta específica con autenticación básica.
 
3. **Puntos a cubrir:**  
  - Uso de `htpasswd` para crear un usuario y su contraseña.

  - Modificar la configuración de NGINX para aplicar la autenticación básica a una ruta específica.
 
4. **Preguntas de reflexión:** 
  - ¿Cuándo es apropiado usar autenticación básica y cuándo no lo es?
 
  - ¿Cómo podrías gestionar la seguridad de las contraseñas almacenadas en `.htpasswd`?


---


#### Tarea 2: Habilitar HTTPS con Let’s Encrypt 
 
1. **Objetivo:**  Implementar HTTPS en el servidor utilizando un certificado SSL de Let’s Encrypt.
 
2. **Requisitos:**  
  - El estudiante debe usar `certbot` para obtener y configurar un certificado SSL para el dominio asociado al servidor.

  - Configurar redireccionamiento de todo el tráfico HTTP a HTTPS para garantizar que todas las conexiones sean seguras.
 
3. **Puntos a cubrir:**  
  - Instalación de `certbot` y solicitud de un certificado SSL.

  - Configuración de NGINX para manejar conexiones HTTPS.

  - Redireccionamiento de tráfico HTTP a HTTPS en NGINX.
 
4. **Preguntas de reflexión:** 
  - ¿Por qué es importante cifrar las conexiones con HTTPS en sitios web modernos?

  - ¿Qué limitaciones tiene Let’s Encrypt en comparación con otros proveedores de certificados SSL?


---


#### Tarea 3: Habilitar HTTP/2 
 
1. **Objetivo:**  Mejorar el rendimiento del servidor web habilitando el protocolo HTTP/2, que permite la multiplexación y reduce la latencia.
 
2. **Requisitos:** 
  - El estudiante debe modificar la configuración de NGINX para habilitar HTTP/2 en las conexiones seguras (HTTPS).
 
3. **Puntos a cubrir:** 
  - Verificar la compatibilidad de HTTP/2 con NGINX.

  - Configurar el protocolo HTTP/2 en el bloque del servidor HTTPS.
 
4. **Preguntas de reflexión:** 
  - ¿Qué ventajas ofrece HTTP/2 frente a HTTP/1.1 en términos de rendimiento?

  - ¿Cómo influye HTTP/2 en la experiencia del usuario y el SEO de un sitio web?


---


### Actividades Adicionales: 
 
1. **Configurar Acceso Condicional por IP:** 
Investigar cómo restringir el acceso a la ruta protegida por autenticación básica a direcciones IP específicas.
 
2. **Habilitar Políticas de Seguridad para SSL:** 
Investigar cómo fortalecer la configuración de SSL en NGINX para mitigar ataques como BEAST y POODLE mediante la configuración de ciphers seguros y prácticas recomendadas.
 
3. **Monitoreo del Rendimiento con HTTP/2:** 
Evaluar el impacto de HTTP/2 en el rendimiento de la página utilizando herramientas de monitoreo como Chrome DevTools o Lighthouse.


---


### Entregables: 

- Archivo de configuración de NGINX con la autenticación básica, HTTPS y HTTP/2 habilitado.
 
- Capturas de pantalla del sitio web mostrando el candado de seguridad (HTTPS) y la ventana emergente solicitando autenticación al acceder a `/admin`.

- Respuesta a las preguntas de reflexión al final de cada tarea.


---

**Evaluación:** 
La práctica será evaluada en base a:
1. Correcta configuración de cada funcionalidad solicitada.

2. Reflexión adecuada sobre las preguntas planteadas.

3. Capacidad de integrar múltiples tecnologías de forma coherente para mejorar la seguridad y el rendimiento del servidor.


---


Esta propuesta plantea la configuración y el uso de funcionalidades clave en un servidor NGINX, que son fundamentales tanto en entornos de producción como en desarrollo. La práctica no solo asegura que los estudiantes aprendan a configurar un entorno seguro, sino que también los desafía a pensar en cómo optimizar la seguridad y el rendimiento de un sitio web.
