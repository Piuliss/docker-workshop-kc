# Taller Intro Docker (Containers → Compose → Swarm bonus)

Este repositorio contiene un **laboratorio tipo KillerCoda** (paso a paso) para una clase de **2h30**.

## Contenido
- Imágenes (pull/ls/history)
- Contenedores (run/ps/logs/stop/rm)
- `exec`/procesos/restart policy
- Inspección (`inspect`, `diff`, `cp`, `stats`)
- Redes (custom network + DNS)
- Persistencia (volúmenes vs bind mounts)
- Dockerfile (web estática)
- Docker Compose (Flask+Redis)
- **Bonus**: Docker Swarm (service/scale/update)

## Estructura (KillerCoda)
- `structure.json`
- `docker-intro/`
  - `index.json`
  - `intro.md`
  - `step1.md` … `step9.md`
  - `finish.md`

## Uso
1. Conecta este repo desde [KillerCoda](https://killercoda.com) (modo Creator / GitHub import).
2. Asegúrate de que el escenario detectado sea `docker-intro`.
3. Publica y comparte el link con los alumnos.

## Nota
Si no usas KillerCoda, los alumnos pueden seguir las instrucciones localmente con Docker Desktop o Docker Engine.
