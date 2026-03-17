# 8) Dockerfile: build de una web estática (nginx + index)

## Objetivo
Construir una imagen propia (`uni/web:1.0`) y exponer una web estática para validar el ciclo **Dockerfile → build → run → probar por puerto**.

Crea proyecto:
```bash
mkdir -p webimg && cd webimg
```{{exec}}

Crea `index.html`:
```bash
cat > index.html << 'EOF'
<!doctype html>
<html>
  <head><meta charset="utf-8"><title>Docker Workshop</title></head>
  <body>
    <h1>Hola desde una imagen custom</h1>
    <p>Build time: reemplaza esto en el futuro con tu app real.</p>
  </body>
</html>
EOF
```{{exec}}

Crea Dockerfile:
```bash
cat > Dockerfile << 'EOF'
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EOF
```{{exec}}

## Nota importante: CMD vs ENTRYPOINT (muy frecuente en entrevistas y en prod)
- En `nginx:alpine`, la imagen ya trae un **proceso por defecto** (CMD/ENTRYPOINT) que mantiene vivo el contenedor.
- Regla operativa: **si el proceso PID 1 termina, el contenedor muere**.
- Si en tus Dockerfiles defines `CMD`/`ENTRYPOINT`, estás decidiendo cuál será ese PID 1.

Build:
```bash
docker image build -t uni/web:1.0 .
```{{exec}}

Run:
```bash
docker container run -d --name web2 -p 8090:80 uni/web:1.0
```{{exec}}

## ¿Qué deberías ver?
- Un contenedor `web2` corriendo.
- Una página HTML con el título “Docker Workshop”.

Abrir en el navegador:
- `{{TRAFFIC_HOST1_8090}}`

Logs (opcional) y prueba:
```bash
docker container logs --tail 20 web2
```{{exec}}

## Troubleshooting (con ejemplo)
### Ejemplo A: “no abre la web” por puerto mal mapeado
1) Provoca el error a propósito:

```bash
docker container rm -f web2 2>/dev/null || true
docker container run -d --name web2 -p 8090:8080 uni/web:1.0
```{{exec}}

2) Síntoma: al abrir `{{TRAFFIC_HOST1_8090}}` no verás la página.

3) Diagnóstico: ¿qué puertos expone nginx dentro del contenedor?

```bash
docker container port web2
```{{exec}}

4) Arreglo: publica el **puerto interno correcto** (80):

```bash
docker container rm -f web2
docker container run -d --name web2 -p 8090:80 uni/web:1.0
```{{exec}}

### Ejemplo B: build falla por `COPY` incorrecto
1) Provoca el error (path mal escrito):

```bash
sed -i.bak 's|COPY index.html|COPY index.htm|' Dockerfile
docker image build -t uni/web:broken .
```{{exec}}

2) Deberías ver un error tipo “not found”/“failed to compute cache key”.

3) Arreglo: restaura el Dockerfile y rebuild:

```bash
mv Dockerfile.bak Dockerfile
docker image build -t uni/web:1.0 .
```{{exec}}

Limpieza:
```bash
docker container rm -f web2
```{{exec}}

Volver a raíz:
```bash
cd ..
```{{exec}}

## ¿Qué debes poder explicar?
- Qué hace `FROM` y `COPY`
- Qué es el contexto de build (el `.`)
