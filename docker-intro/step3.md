# 3) Contenedores: run/ps/logs/stop/rm

## Objetivo
Lanzar un contenedor en primer plano y otro en background, entender mapeo de puertos, y practicar logs/parar/borrar.

## ¿Qué es `hello-world`?
Es una imagen de prueba oficial que valida que tu Docker Engine puede **descargar** una imagen y **ejecutar** un contenedor.

Hello world:
```bash
docker container run --rm hello-world
```{{exec}}

## ¿Qué es Nginx y por qué lo usamos?
Nginx es un **servidor HTTP** muy usado. Es ideal para laboratorio porque “funciona” sin configurar nada y deja claro cómo se publican puertos con `-p`.

Servidor web en background:
```bash
docker container run -d --name web -p 80:80 nginx:latest
```{{exec}}

## ¿Qué deberías ver?
- `docker container ls` debe mostrar `web` en estado **Up**.
- Debe haber un servicio HTTP accesible en:
  - `{{TRAFFIC_HOST1_80}}`

Ver estado:
```bash
docker container ls
```{{exec}}

Logs:
```bash
docker container logs --tail 20 web
```{{exec}}

Prueba reinicio y limpieza:
```bash
docker container stop web
```{{exec}}

```bash
docker container rm web
```{{exec}}

## ¿Qué debes poder explicar?
- `-d`, `--rm`, `-p HOST:CONT`
- Por qué un contenedor “muere” si el proceso PID 1 termina
