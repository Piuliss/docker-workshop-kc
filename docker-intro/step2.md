# 2) Imágenes: pull, ls, filter, history

```bash
docker image pull alpine:latest
```{{exec}}

```bash
docker image pull nginx:latest
```{{exec}}

```bash
docker image ls --no-trunc
```{{exec}}

Filtra por referencia:
```bash
docker image ls --filter=reference='alpine*'
```{{exec}}

Capas / historial:
```bash
docker image history nginx:latest
```{{exec}}

## ¿Qué debes poder explicar?
- Qué es un tag (`:latest` no es “la última versión garantizada”)
- Qué representa `history` (capas)
