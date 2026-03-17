# 4) Entrar al contenedor: exec/attach + procesos

## Objetivo
Distinguir `run` vs `exec` y ver que un contenedor vive mientras su **PID 1** siga activo.

Crear Alpine “vivo”:
```bash
docker container run -dit --name a1 alpine:latest sh
```{{exec}}

Entrar con exec:
```bash
docker container exec -it a1 sh
```{{exec}}

Dentro:
```bash
ps
```{{exec}}

```bash
whoami && uname -a
```{{exec}}

Sal con `exit`.

## ¿Qué deberías ver?
- Dentro del contenedor, `ps` muestra al `sh` como PID 1 (o el primer proceso).
- `docker logs` de `a1` normalmente estará vacío (no estás corriendo un servicio que loguee).

## Restart policy (concepto operativo)
```bash
docker container rm -f a1
```{{exec}}

```bash
docker container run -d --restart unless-stopped --name a1 alpine:latest sh -c "date; sleep 2; exit 1"
```{{exec}}

```bash
docker container ls -a --filter name=a1
```{{exec}}

```bash
docker container logs --tail 50 a1
```{{exec}}

Limpieza:
```bash
docker container rm -f a1
```{{exec}}

## ¿Qué debes poder explicar?
- `exec` entra a un contenedor existente; `run` crea uno nuevo
- `--restart` y por qué importa en servicios
