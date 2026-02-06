# 🐳 Taller Integral: Docker & Docker Compose (Modern Stack 2026)

**Dirigido a:** Clanes Riwi  
**Rol:** Developer / DevOps Junior  
**Duración:** 2 Horas (30 min Teoría / 90 min Práctica)  
**Metodología:** Trabajo por Células  

---

## PARTE 1: Fundamentos (Introducción Teórica)
*Lectura obligatoria antes de iniciar el código.*

### 1. El Problema: "En mi máquina funciona" 🤷‍♂️
Cuando compartimos código, a menudo falla en otras máquinas por versiones diferentes de Node, Java o bases de datos mal configuradas.
* **La Solución:** No enviamos solo el código (muebles). Enviamos el entorno completo (la casa) dentro de un contenedor.



### 2. Imagen vs. Contenedor vs. Dockerfile
Para entender Docker, usaremos la analogía de la Programación Orientada a Objetos (POO):

| Concepto Docker | Analogía POO | Descripción |
| :--- | :--- | :--- |
| **Dockerfile** | El Código Fuente | Es la "receta". Un archivo de texto con instrucciones (`FROM`, `COPY`, `RUN`). |
| **Imagen** | La Clase | Es el resultado de la receta. Es inmutable (no se puede cambiar una vez creada). |
| **Contenedor** | La Instancia (Objeto) | Es la imagen en ejecución. Puedes tener 100 contenedores basados en una sola imagen. |



### 3. Docker Compose (El Orquestador) 🎻
En el mundo real, una App tiene Frontend, Backend y Base de Datos. Levantar 3 contenedores a mano es ineficiente.
* **Compose:** Un archivo (`compose.yaml`) que define toda la infraestructura.
* **Networking Interno:** ¡OJO AQUÍ! ⚠️ En Docker Compose, los servicios se hablan por su **nombre**, no por `localhost`.
    * Si tu base de datos se llama `db`, tu backend se conecta a `postgres://db:5432`.



[Image of docker compose architecture diagram]


### 4. Volúmenes (Persistencia) 💾
Los contenedores son efímeros. Si se apagan o reinician, los datos internos se borran (amnesia).
* **Volumen:** Es un "cable" que conecta una carpeta del contenedor a una carpeta de tu disco duro real. Así, los datos sobreviven aunque el contenedor explote.



---

## PARTE 2: El Taller (Docker Cell Challenge)

### 🚨 Reglas de Juego
1.  **Estándar Moderno:** Prohibido usar comandos viejos. Usamos `docker compose` (sin guion) y el estándar V2.
2.  **Logs son vida:** Si algo falla, ejecutar `docker compose logs -f [nombre_servicio]`.
3.  **Colaboración:** Si tu célula termina, ayuda a la siguiente (Cultura Riwi).

---

### 🟢 Reto 1: El Contenedor Web (Modern Init)
**Objetivo:** Crear una imagen optimizada para un sitio web estático usando herramientas de IA nativas de Docker.

1.  Creen una carpeta nueva para su célula: `mkdir reto-docker && cd reto-docker`
2.  Creen un archivo `index.html` que contenga un `<h1>` con el nombre de su célula.
3.  Ejecuten el comando de inicialización:
    ```bash
    docker init
    ```
    * Seleccionen: *Other* -> *Nginx*.
4.  **Misión:** El `docker init` crea un Dockerfile genérico. Deben editarlo para asegurarse de que copie su `index.html` específico dentro de la carpeta `/usr/share/nginx/html` del contenedor.

---

### 🟡 Reto 2: Orquestación y Healthchecks (Compose V2)
**Objetivo:** Levantar una arquitectura real (Base de Datos + Interfaz Gráfica) que sea resiliente a fallos de inicio.

1.  Creen un archivo llamado `compose.yaml` (Nota: Ya no usamos `version: '3.x'`, es obsoleto).
2.  Definan dos servicios:
    * `db`: Usando la imagen `postgres:16-alpine`.
    * `adminer`: Usando la imagen `adminer` (puerto 8080).
3.  **Misión Crítica:** Configurar un **Healthcheck** en la base de datos.
    * *¿Por qué?* Adminer suele fallar si intenta conectarse antes de que Postgres haya terminado de arrancar.
    * Deben usar `depends_on` con la condición `service_healthy` en el servicio de Adminer.

---

### 🔴 Reto 3: Desarrollo en Tiempo Real (Docker Watch)
**Objetivo:** La funcionalidad más nueva de Docker (2025). Lograr "Hot Reload" dentro del contenedor.

1.  Agreguen su servicio web (el del Reto 1) al `compose.yaml`.
2.  **Misión:** Configurar la sección `develop` y `watch` en el servicio web.
    * Acción: `sync`.
    * Debe sincronizar los cambios de su `index.html` local hacia el contenedor automáticamente.
3.  Ejecuten `docker compose watch`, cambien el HTML y vean la magia en el navegador sin reiniciar nada.

---

## PARTE 3: Cheatsheet (Comandos Modernos 2026)

| Acción | Comando Moderno ✅ |
| :--- | :--- |
| **Iniciar todo (Background)** | `docker compose up -d` |
| **Forzar reconstrucción** | `docker compose up -d --build` |
| **Ver estado de servicios** | `docker compose ps` |
| **Ver logs en tiempo real** | `docker compose logs -f [nombre_servicio]` |
| **Entrar al contenedor** | `docker compose exec -it [nombre_servicio] sh` |
| **Modo Desarrollo (Sync)** | `docker compose watch` |
| **Apagar todo** | `docker compose down` |
| **Apagar y borrar datos** | `docker compose down -v` (⚠️ Peligro) |

---
**¡Éxito Clanes! A romper el código.** 🚀