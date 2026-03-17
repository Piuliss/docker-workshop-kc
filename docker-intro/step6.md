# 6) Networking: red custom + DNS por nombre

## Objetivo
Crear una red bridge propia y validar que Docker provee DNS por nombre dentro de esa red.

## ¿Qué es `curl`?
`curl` es una herramienta de línea de comandos para hacer requests HTTP. Aquí la usamos para probar conectividad sin abrir navegador.

Crear red:
```bash
docker network create labnet
```{{exec}}

Crea un contenedor “servidor”:
```bash
docker container run -d --name srv --network labnet nginx:latest
```{{exec}}

Crea un contenedor cliente y resuelve por nombre:
```bash
docker container run --rm --network labnet alpine:latest sh -c "apk add --no-cache curl >/dev/null && curl -I http://srv | head -n 5"
```{{exec}}

Ver redes:
```bash
docker network ls
```{{exec}}

Inspeccionar red:
```bash
docker network inspect labnet --format '{{json .Containers}}'
```{{exec}}

## ¿Qué deberías ver?
- El `curl -I http://srv` responde con `HTTP/1.1 200 OK` (o similar).
- `network inspect` lista al contenedor `srv` dentro de `labnet`.

Limpieza:
```bash
docker container rm -f srv
```{{exec}}

```bash
docker network rm labnet
```{{exec}}

## Ten en cuenta que…
- Las redes **custom bridge** habilitan resolución por **nombre de contenedor**, la `bridge` default no.
- Pensar en DNS por nombre te prepara para Compose, Swarm y Kubernetes (donde casi nunca apuntas por IP).
