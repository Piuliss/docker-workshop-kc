# 8b) Multi-stage + .dockerignore + USER

## Objetivo
Aplicar tres prácticas que separan "funciona" de "está listo para prod":
**multi-stage build** (imágenes mucho más chicas), **`.dockerignore`**
(contexto de build reducido) y **`USER`** (no correr como root).

## 8.1 Multi-stage con Go

Crea el proyecto:
```bash
mkdir -p goapp && cd goapp
```{{exec}}

`main.go`:
```bash
cat > main.go << 'EOF'
package main

import "fmt"

func main() {
    fmt.Println("Hola desde un binario Go de 10MB en un container de 10MB")
}
EOF
```{{exec}}

Dockerfile multi-stage:
```bash
cat > Dockerfile << 'EOF'
FROM golang:1.22-alpine AS builder
WORKDIR /src
COPY main.go .
RUN go build -o app main.go

FROM alpine:latest
COPY --from=builder /src/app /app
USER 1000
CMD ["/app"]
EOF
```{{exec}}

Build:
```bash
docker image build -t goapp:multi .
```{{exec}}

Compara tamaños:
```bash
docker image ls goapp:multi
```{{exec}}

Ejecutá:
```bash
docker container run --rm goapp:multi
```{{exec}}

## 8.2 `.dockerignore`

```bash
cat > .dockerignore << 'EOF'
node_modules
.git
*.log
.env
__pycache__
EOF
```{{exec}}

## 8.3 USER no-root

```bash
docker container run --rm alpine:latest sh -c "id"
```{{exec}}

```bash
docker container run --rm --user 1000:1000 alpine:latest sh -c "id"
```{{exec}}

## ¿Qué deberías ver?
- `docker image ls goapp:multi` muestra una imagen final **muy pequeña**
  (~15MB) comparada con `golang:1.22-alpine` (~800MB).
- `id` sin `--user` muestra `uid=0` (root). Con `--user 1000:1000` muestra `uid=1000`.

Limpieza:
```bash
docker container run --rm goapp:multi
cd ..
```{{exec}}

## Ten en cuenta que…
- **Multi-stage** es la forma ordenada de separar "lo que necesito para
  compilar" de "lo que necesito para correr". Lo segundo es muchísimo más
  chico.
- **`.dockerignore`** es la optimización más barata que podés hacer en
  Docker. Sin él, el contexto de build incluye `node_modules/`, `.git/`,
  etc.
- **`USER` no es suficiente** por sí solo para seguridad, pero combinado
  con `--read-only` y `--cap-drop=ALL` te da una base razonable.
