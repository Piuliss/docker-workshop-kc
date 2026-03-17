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

## ¿Qué debes poder explicar?
- Por qué una red custom habilita DNS por nombre de contenedor
- Diferencia conceptual con `bridge` default
