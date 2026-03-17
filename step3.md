# 3) Contenedores: run/ps/logs/stop/rm

Hello world:
```bash
docker container run --rm hello-world
```{{exec}}

Servidor web en background:
```bash
docker container run -d --name web -p 80:80 nginx:latest
```{{exec}}

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
