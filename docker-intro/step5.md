# 5) Inspección: inspect, diff, cp, stats

## Objetivo
Usar `inspect` como “fuente de verdad” y ver cambios/recursos de un contenedor.

Lanza nginx:
```bash
docker container run -d --name web -p 8080:80 nginx:latest
```{{exec}}

Abrir en el navegador:
- `{{TRAFFIC_HOST1_8080}}`

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

## ¿Qué deberías ver?
- `inspect` imprime una IP interna (típicamente 172.x/192.x) y un mapeo de `8080/tcp`.
- `stats` muestra CPU/Mem (aunque sea muy bajo).

Limpieza:
```bash
docker container rm -f web
```{{exec}}

## ¿Qué debes poder explicar?
- Qué es `inspect` (fuente de verdad)
- Qué muestra `diff` (RW layer)
- Para qué sirve `cp` en IR/forense básica
