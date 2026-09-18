# Taller Intro Docker (Containers → Compose → Swarm bonus)

Este repositorio contiene un **laboratorio tipo KillerCoda** (paso a paso) para
una clase de **2 sesiones × 1.5h = 3h**.

## Contenido (12 steps)

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

## Estructura (KillerCoda)

- `structure.json` — declara el escenario
- `docker-intro/`
  - `index.json` — metadata + lista de steps
  - `intro.md` — pantalla inicial
  - `step1.md` … `step11.md` + `step8b.md`
  - `finish.md` — pantalla final

## Cambios vs versión upstream

Esta versión extiende el repo original (`Piuliss/docker-workshop-kc`) con:

- **step8b** — multi-stage builds, `.dockerignore`, USER no-root.
- **step9** — Compose con WordPress + MariaDB (incluye backup/restore con
  `mysqldump` y `down -v`). Reemplaza al ejemplo Flask+Redis anterior.
- **step10** — Swarm dedicado (init, service, scale, rolling update). Incluye
  disclaimer explícito sobre estado de mantenimiento.
- **step11** — mini-lab de seguridad (sin cambios, renumerado).

## Publicación en KillerCoda

Ver [PUBLISH.md](./PUBLISH.md) para los pasos exactos de publicación.
