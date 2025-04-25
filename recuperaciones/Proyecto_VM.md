

## 📋 Enunciado del Proyecto 


Tu tarea es desplegar una arquitectura como la siguiente:



```lua
+---------------------+
                       |      Cliente        |
                       +---------------------+
                                |
                                v
                         +-------------+
                         |   NGINX     | <--- Servidor web y balanceador de carga
                         +-------------+
                        /               \
                       v                 v
              +-------------+     +-------------+
              | API BACKEND |     | API BACKEND |
              | Instancia 1 |     | Instancia 2 |
              +-------------+     +-------------+
```



---



## 🔧 Requisitos Técnicos 


### 🔹 1. Sitio Estático 

 
- Crea un frontend con react, (frontend de un crud)

 
- Debe estar accesible desde `http://localhost`.


### 🔹 2. Microservicios Backend (APIs) 

Debes crear **dos APIs independientes** , ambas simulan una pequeña aplicación de gestión de tareas. Cada una:
 
- Guarda las tareas en memoria local.
 
- Expone estas rutas:



```bash
GET    /api/tareas/         # Lista todas las tareas
GET    /api/tareas/<id>     # Obtiene una tarea por ID
POST   /api/tareas/         # Crea una nueva tarea
DELETE /api/tareas/<id>     # Elimina una tarea por ID
UPDATE /api/tareas/<id>     # Actualiza una tarea por ID
```

Cada backend debe identificarse claramente con una propiedad `"backend": "backend1"` o `"backend2"` en las respuestas, para poder comprobar el funcionamiento del balanceador.

Ambos servicios deben estar escritos en Flask (o Node.js si prefieres), y ejecutarse en diferentes puertos (por ejemplo: 5001 y 5002).


### 🔹 3. NGINX como balanceador de carga 

 
- Debe:

 
  - Servir la web estática desde `/`.
 
  - Redirigir las peticiones a `/api/` hacia los backends.
 
  - Utilizar un bloque `upstream` con al menos 2 servidores (los dos backends).
 
  - Balancear mediante round-robin.


### 🔹 4. HTTPS 

 
- Puedes usar un certificado autofirmado Let's Encrypt.
 
- Redirigir todo HTTP a HTTPS correctamente.


### 🔹 5. Docker y Docker Compose 

 
- Automatiza la ejecución con `docker-compose.yml`.
 
- Se deben levantar:

 
  - NGINX
 
  - backend1
 
  - backend2



---



## 🧠 Criterios de Evaluación 

| Criterio | Puntos | 
| --- | --- | 
| Sitio estático funcional y accesible | 2 pts | 
| API backend 1 funcional con respuestas válidas | 2 pts | 
| API backend 2 funcional con diferencias claras respecto a 1 | 2 pts | 
| Configuración correcta de NGINX como balanceador (proxy + upstream) | 3 pts | 
| Estructura limpia del proyecto, documentación, comentarios | 1 pt | 
| HTTPS funcionando | +0.5 pt | 
| Docker Compose funcional | +0.5 pt | 

**Total: 10 puntos (máximo 11)** 


---



## 📁 Estructura esperada del proyecto 



```csharp
proyecto_nginx_balanceador/
│
├── frontend/
│   └── ...
│
├── backend1/
│   ├── app.py
│   └── requirements.txt
│
├── backend2/
│   ├── app.py
│   └── requirements.txt
│
├── nginx/
│   └── default.conf
│
├── docker-compose.yml        # (opcional)
└── README.md                 # Instrucciones claras de ejecución
```



---



## 🧪 Ejemplo de test esperado 

Al realizar múltiples peticiones a `http://localhost/api/tareas/`, la respuesta debería alternarse entre `backend1` y `backend2`, mostrando tareas distintas y valores del campo `"backend"` para verificar el balanceo de carga.


---

