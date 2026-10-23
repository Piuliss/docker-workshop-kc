# Taller Intro Docker (Containers → Compose → Swarm bonus)

Este repositorio contiene un **laboratorio tipo KillerCoda** (paso a paso) para
una clase de **2 sesiones × 1.5h = 3h**.

## Contenido (13 steps)

0. **Requisito: instalar Docker Compose** (step0.md)
1. Verificar entorno + reglas del juego
2. Imágenes: pull, ls, history
3. Contenedores: run/ps/logs/stop/rm
4. Entrar al contenedor: exec/attach + procesos
5. Inspección: inspect, diff, cp, stats
6. Networking: red custom + DNS por nombre
7. Persistencia: volumes vs bind mounts
8. Dockerfile: build de una web estática
8b. Multi-stage + .dockerignore + USER
9. Compose: WordPress + MariaDB (backup/restore)
10. Swarm: demo de orquestación (BONUS)
11. Seguridad básica en contenedores (mini-lab)

## Requisito de Docker Compose (por qué el step 0)

El escenario corre sobre la imagen base **`ubuntu`** (`docker-intro/index.json` →
`backend.imageid`). Esa imagen trae el **Docker Engine** pero **no** el plugin de
Compose, así que `docker compose ...` falla con:

```
unknown shorthand flag: 'f' in -f
Usage:  docker [OPTIONS] COMMAND [ARG...]
```

El **step 0** instala el binario standalone de **Compose V2**
(`docker-compose`, con guion) en `/usr/local/bin/docker-compose`, verificando el
checksum SHA-256 contra el `checksums.txt` oficial de la release, y lo deja
listo en `~/.docker/cli-plugins/` para que `docker compose` también funcione en
CLIs modernos (≥ 19.03).

Por eso, en los steps 7b y 9 los comandos de Compose van como **`docker-compose`**
(con guion). Es **el mismo motor** (Compose V2), la misma sintaxis y los mismos
`compose.yml` — lo que cambia es cómo se invoca el binario.

## Estructura (KillerCoda)

- `structure.json` — declara el escenario
- `docker-intro/`
  - `index.json` — metadata + lista de steps
  - `intro.md` — pantalla inicial
  - `step0.md` (requisito de Compose) + `step1.md` … `step11.md` + `step8b.md`
  - `finish.md` — pantalla final

## Cambios vs versión upstream

Esta versión extiende el repo original (`Piuliss/docker-workshop-kc`) con:

- **step0** — instala Docker Compose V2 (standalone) y explica la diferencia
  entre `docker compose` (plugin) y `docker-compose` (binario), que es el
  requisito que faltaba en la imagen base `ubuntu`.
- **step8b** — multi-stage builds, `.dockerignore`, USER no-root.
- **step9** — Compose con WordPress + MariaDB (incluye backup/restore con
  `mysqldump` y `down -v`). Reemplaza al ejemplo Flask+Redis anterior.
- **step10** — Swarm dedicado (init, service, scale, rolling update). Incluye
  disclaimer explícito sobre estado de mantenimiento.
- **step11** — mini-lab de seguridad (sin cambios, renumerado).

## Publicación en KillerCoda

Ver [PUBLISH.md](./PUBLISH.md) para los pasos exactos de publicación.
