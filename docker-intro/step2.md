# 2) Imágenes: pull, ls, filter, history

## Objetivo
Entender “imagen = plantilla de solo lectura con capas” y practicar cómo inspeccionarla.

## ¿Qué son Alpine y Nginx (en 20 segundos)?
- **Alpine**: una distribución Linux *muy* pequeña. Se usa mucho en contenedores para tener imágenes ligeras.
- **Nginx**: un servidor web/proxy muy común. Aquí lo usamos porque permite ver rápido el mapeo de puertos y logs.

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

## ¿Qué deberías ver?
- `docker image ls` muestra `alpine` y `nginx`.
- `docker image history nginx:latest` lista varias capas (no es un solo blob).

## ¿Qué debes poder explicar?
- Qué es un tag (`:latest` no es “la última versión garantizada”)
- Qué representa `history` (capas)
