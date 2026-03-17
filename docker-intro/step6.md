# 6) Networking: red custom + DNS por nombre

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
