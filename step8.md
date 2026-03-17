# 8) Dockerfile: build de una web estática (nginx + index)

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

Build:
```bash
docker image build -t uni/web:1.0 .
```{{exec}}

Run:
```bash
docker container run -d --name web2 -p 8090:80 uni/web:1.0
```{{exec}}

Logs y prueba:
```bash
docker container logs --tail 20 web2
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
