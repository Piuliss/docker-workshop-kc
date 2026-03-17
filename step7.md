# 7) Persistencia: volumes vs bind mounts

## 7.1 Volumen (gestionado por Docker)
```bash
docker volume create mi-vol
```{{exec}}

```bash
docker container run --rm -v mi-vol:/data alpine:latest sh -c "echo hola > /data/hola.txt && ls -l /data"
```{{exec}}

```bash
docker container run --rm -v mi-vol:/data alpine:latest sh -c "cat /data/hola.txt"
```{{exec}}

```bash
docker volume inspect mi-vol
```{{exec}}

## 7.2 Bind mount (carpeta del host → contenedor)
```bash
mkdir -p bindtest && echo "v1" > bindtest/app.txt
```{{exec}}

```bash
docker container run --rm -v "$(pwd)/bindtest:/app" alpine:latest sh -c "ls -l /app && cat /app/app.txt"
```{{exec}}

## ¿Qué debes poder explicar?
- Volumen: mejor para datos “de runtime”
- Bind: mejor para desarrollo (edito en host, se refleja en contenedor)
