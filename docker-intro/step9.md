# 9) Docker Compose (2 servicios) + Swarm (BONUS)

## 9.1 Compose: Flask + Redis (contador)
## Objetivo
Levantar una app multi-servicio y comprobar comunicación por nombre de servicio (`redis`) y persistencia lógica (contador en Redis mientras el stack está arriba).

## ¿Qué es cada servicio?
- **Flask (web)**: micro-framework de Python para exponer un endpoint HTTP. Aquí sirve para ver una app “real” mínima.
- **Redis**: base de datos en memoria tipo key-value. Aquí la usamos para guardar un contador (`hits`) compartido entre requests.

```bash
mkdir -p compose-lab && cd compose-lab
```{{exec}}

```bash
cat > compose.yml << 'EOF'
services:
  web:
    image: python:3.11-alpine
    working_dir: /app
    volumes:
      - .:/app
    ports:
      - "5000:5000"
    command: sh -c "pip install -q flask redis && python app.py"
    depends_on:
      - redis
  redis:
    image: redis:alpine
EOF
```{{exec}}

```bash
cat > app.py << 'EOF'
from flask import Flask
from redis import Redis

app = Flask(__name__)
r = Redis(host="redis", port=6379)

@app.get("/")
def home():
    n = r.incr("hits")
    return f"Hits = {n}\n"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
EOF
```{{exec}}

Levantar:
```bash
docker compose -f compose.yml up -d
```{{exec}}

Estado:
```bash
docker compose -f compose.yml ps
```{{exec}}

Logs:
```bash
docker compose -f compose.yml logs --tail 50 web
```{{exec}}

## ¿Qué deberías ver?
- `docker compose ps` muestra `web` y `redis` en **running**.
- Al abrir la web, el contador **sube** en cada refresh.

Abrir en el navegador:
- `{{TRAFFIC_HOST1_5000}}`

## Troubleshooting (rápido)
Si `web` no responde o el contador no sube:

```bash
docker compose -f compose.yml ps
docker compose -f compose.yml logs --tail 80 web
docker compose -f compose.yml logs --tail 80 redis
```{{exec}}

Pista: si `web` no logra conectar, casi siempre verás errores de conexión a `redis` en los logs de `web` (nombre de servicio, orden de arranque, etc.).

Bajar:
```bash
docker compose -f compose.yml down -v
```{{exec}}

## 9.2 BONUS: Swarm en 1 nodo (services/stack/scale)
> Si el tiempo alcanza: esto es demo. En 1 nodo igual sirve para ver el modelo.

Init:
```bash
docker swarm init
```{{exec}}

Crear un servicio web replicado:
```bash
docker service create --name websvc -p 8088:80 --replicas 3 nginx:alpine
```{{exec}}

Ver servicios y tareas:
```bash
docker service ls
```{{exec}}

```bash
docker service ps websvc
```{{exec}}

Escalar:
```bash
docker service scale websvc=5
```{{exec}}

Actualizar imagen (rolling update):
```bash
docker service update --image nginx:latest websvc
```{{exec}}

Borrar servicio:
```bash
docker service rm websvc
```{{exec}}

Salir de swarm:
```bash
docker swarm leave --force
```{{exec}}

Volver:
```bash
cd ..
```{{exec}}

## ¿Qué debes poder explicar?
- Compose: describe una app multi-servicio local (y más)
- Swarm: modelo declarativo de servicios (replicas, update, rollback)
