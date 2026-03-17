# 1) Verificar entorno + reglas del juego

```bash
docker version
```{{exec}}

```bash
docker info --format '{{json .ServerVersion}}'
```{{exec}}

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
