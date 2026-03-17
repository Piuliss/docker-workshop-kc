# 1) Verificar entorno + reglas del juego

## Objetivo
Confirmar que el entorno tiene Docker funcionando y que sabes “leer” el estado del engine.

```bash
docker version
```{{exec}}

```bash
docker info --format '{{json .ServerVersion}}'
```{{exec}}

## ¿Qué deberías ver?
- `docker version` muestra **Client** y **Server** sin errores.
- `docker info` no debe decir “Cannot connect to the Docker daemon”.

## Limpieza (por si el entorno viene con residuos)
> No lo ejecutes en tu máquina real sin pensar; aquí es entorno de laboratorio.

```bash
docker container ls -a
```{{exec}}

```bash
docker image ls
```{{exec}}

## ¿Qué debes poder explicar?
- ¿Qué es Docker Engine?
- Diferencia entre cliente y daemon
- Qué significa “imagen” y “contenedor” (definición operacional)
