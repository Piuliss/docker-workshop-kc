# 7) Persistencia: volumes vs bind mounts

## Objetivo
Comparar volumen (gestionado por Docker) vs bind mount (carpeta del host) y cuándo usar cada uno.

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

## ¿Qué deberías ver?
- El archivo `hola.txt` persiste entre ejecuciones del contenedor cuando usas el volumen `mi-vol`.
- En bind mount, lo que escribas en `bindtest/app.txt` se refleja al instante en el contenedor.

## Ten en cuenta que…
- Los **volúmenes** son ideales para datos de **runtime** (bases de datos, uploads, etc.) gestionados por Docker.
- Los **bind mounts** brillan en desarrollo: editas en tu máquina y ves el cambio inmediato dentro del contenedor.
