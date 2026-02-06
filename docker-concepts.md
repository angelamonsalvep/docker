# 📘 Guía Maestra: Anatomía de Docker & Compose

**Documento de Referencia - Riwi**
Esta guía desglosa la arquitectura interna de Docker para entender no solo cómo usarlo, sino cómo funciona "bajo el capó".

---

## 1. Conceptos Clave (El Vocabulario) 🗣️

Antes de operar, definamos las piezas del rompecabezas:

* **Docker Daemon (`dockerd`):** Es el cerebro. Un proceso en segundo plano que escucha las peticiones de la API de Docker y gestiona los objetos.
* **Docker Client (CLI):** Es tu terminal. Cuando escribes `docker build`, el cliente le envía la orden al Daemon.
* **Docker Registry:** Es "la tienda" (ej. Docker Hub) donde viven las imágenes base.
* **Imagen:** Una plantilla de solo lectura (Read-Only). Es el plano de la casa.
* **Contenedor:** Una instancia ejecutable de una imagen. Es la casa construida.

---

## 2. Anatomía de un "Docker" (Ciclo de Vida) 🧬

El proceso de creación sigue una jerarquía estricta: **Dockerfile ➔ Imagen ➔ Contenedor**.

### A. El Dockerfile (La Receta) 📄
Instrucciones secuenciales. Cada una crea una capa nueva.

| Instrucción | Función | Analogía |
| :--- | :--- | :--- |
| `FROM node:18` | Define la base (SO + Runtime). | Cimientos. |
| `WORKDIR /app` | Crea y se mueve a una carpeta. | Entrar a la cocina. |
| `COPY . .` | Mueve tus archivos al contenedor. | Ingredientes en mesa. |
| `RUN npm install` | Ejecuta comandos *durante* el build. | Cocinar. |
| `CMD ["npm", "start"]` | Comando al *iniciar* el contenedor. | Servir plato. |

### B. La Imagen (Layers) 🍰
* **Inmutabilidad:** Una vez construida, no cambia.
* **Caché:** Docker reutiliza capas inferiores para acelerar los builds.

### C. El Contenedor (Read-Write Layer) 📦
Al hacer `run`, Docker añade una **capa de escritura** temporal sobre la imagen. Si el contenedor se borra, esta capa desaparece (por eso usamos volúmenes).

---

## 3. Anatomía de Docker Compose (`compose.yaml`) 🐙

Define la "Infraestructura como Código". Se divide en 3 bloques:

```yaml
# BLOQUE 1: Servicios (Los Actores)
services:
  backend:                # Nombre DNS interno
    image: node:18-alpine 
    ports:                # Puerta al exterior "Host:Container"
      - "3000:3000"       
    networks:             # Red interna
      - mi-red

  database:
    image: postgres:15
    volumes:              # Persistencia
      - db-data:/var/lib/postgresql/data
    networks:
      - mi-red

# BLOQUE 2: Redes (El cableado)
networks:
  mi-red:

# BLOQUE 3: Volúmenes (El disco duro)
volumes:
  db-data:
```

---

## 4. Comparativa Visual: VM vs. Contenedor 🆚

**💻 Máquina Virtual (VM)**
* Estructura: `Hardware ➔ Host OS ➔ Hypervisor ➔ Guest OS (Completo) ➔ App`
* **Peso:** GBs. **Arranque:** Minutos.
* *Ineficiente:* Duplica el sistema operativo.

**🐳 Contenedor Docker**
* Estructura: `Hardware ➔ Host OS ➔ Docker Engine ➔ App`
* **Peso:** MBs. **Arranque:** Milisegundos.
* *Eficiente:* Comparte el Kernel del Host.