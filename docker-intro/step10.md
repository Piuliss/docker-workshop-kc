# 10) Seguridad básica en contenedores (mini-lab)

## Objetivo
Aplicar 4 prácticas seguras y *ver* el efecto: **no-root**, **filesystem read-only**, **capabilities mínimas** y **límites de recursos**.

> Nota: Esto no reemplaza políticas enterprise ni hardening completo; es un “mínimo viable” alineado a lo visto en clase.

## 10.1 No correr como root
Compara root vs usuario sin privilegios:

```bash
docker container run --rm alpine:latest sh -c "id"
```{{exec}}

```bash
docker container run --rm --user 1000:1000 alpine:latest sh -c "id"
```{{exec}}

## 10.2 Filesystem read-only (reduce impacto si comprometen el contenedor)
Provoca que un write falle:

```bash
docker container run --rm --read-only alpine:latest sh -c "echo test > /tmp/x || echo 'write blocked (expected)'"
```{{exec}}

Arreglo típico: permitir escritura solo donde hace falta (ej. `/tmp`) con `tmpfs`:

```bash
docker container run --rm --read-only --tmpfs /tmp alpine:latest sh -c "echo ok > /tmp/x && cat /tmp/x"
```{{exec}}

## 10.3 Reducir capabilities (principio de mínimo privilegio)
Ejemplo: quitar todas las caps y agregar solo lo necesario (aquí, ninguna):

```bash
docker container run --rm --cap-drop=ALL alpine:latest sh -c "id && echo 'caps dropped'"
```{{exec}}

## 10.4 Limitar recursos (mitiga DoS accidental)
Lanza un contenedor con límites y observa su configuración:

```bash
docker container run -d --name limited --memory=128m --cpus=0.5 alpine:latest sh -c "sleep 300"
```{{exec}}

```bash
docker container inspect limited --format 'Memory={{.HostConfig.Memory}}  NanoCpus={{.HostConfig.NanoCpus}}'
```{{exec}}

Limpieza:

```bash
docker container rm -f limited
```{{exec}}

## ¿Qué debes poder explicar?
- Por qué **no-root** reduce impacto (aunque no es bala de plata).
- Qué cambia con `--read-only` y por qué `tmpfs` es un patrón común.
- Qué son capabilities (alto nivel) y por qué `--cap-drop=ALL` es una base sana.
- Cómo los límites de CPU/Mem ayudan a estabilidad y seguridad.

