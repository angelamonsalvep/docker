
## 💡 PARTE 4: Soluciones Técnicas (Reference)

*Úsenlo solo si se atascan o para validar su trabajo.*

### 📄 Dockerfile (Optimizado)
```dockerfile
# Usamos alpine-slim para que la imagen pese menos de 10MB
FROM nginx:alpine-slim

# Definimos el directorio de trabajo (Buena práctica)
WORKDIR /usr/share/nginx/html

# Copiamos nuestro archivo al contenedor
COPY index.html .

# Exponemos el puerto como documentación
EXPOSE 80
```

### 🐙 compose.yaml (Full Stack Moderno)
```yaml
services:
  # --- Servicio 1: Base de Datos ---
  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: riwi_user
      POSTGRES_PASSWORD: secret_password
      POSTGRES_DB: riwi_db
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data # Persistencia
    healthcheck: # El estándar moderno para evitar Race Conditions
      test: ["CMD-SHELL", "pg_isready -U riwi_user"]
      interval: 5s
      timeout: 5s
      retries: 5

  # --- Servicio 2: Admin Visual ---
  adminer:
    image: adminer
    restart: always
    ports:
      - "8080:8080"
    depends_on:
      db:
        condition: service_healthy # Espera a que el healthcheck pase

  # --- Servicio 3: Web (Reto Watch) ---
  web:
    build: .
    ports:
      - "3000:80"
    develop: # <--- La joya de Docker Compose V2
      watch:
        - action: sync
          path: ./index.html
          target: /usr/share/nginx/html/index.html

# Definición del volumen global
volumes:
  pgdata:
```
