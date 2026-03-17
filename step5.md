# 5) Inspección: inspect, diff, cp, stats

Lanza nginx:
```bash
docker container run -d --name web -p 8080:80 nginx:latest
```{{exec}}

Inspeccionar puertos/IP:
```bash
docker container inspect web --format 'IP={{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}  Ports={{json .NetworkSettings.Ports}}'
```{{exec}}

Cambios en filesystem del contenedor:
```bash
docker container diff web
```{{exec}}

Copia un archivo desde el contenedor al host:
```bash
docker container cp web:/etc/nginx/nginx.conf ./nginx.conf
```{{exec}}

Ver recursos:
```bash
docker container stats --no-stream
```{{exec}}

Limpieza:
```bash
docker container rm -f web
```{{exec}}

## ¿Qué debes poder explicar?
- Qué es `inspect` (fuente de verdad)
- Qué muestra `diff` (RW layer)
- Para qué sirve `cp` en IR/forense básica
