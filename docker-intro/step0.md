# 0) Requisito: Docker Compose

## Objetivo
Dejar **Docker Compose** instalado y verificado en este entorno. Los steps 7b y 9
(la parte de Compose, en ambas clases) lo necesitan.

> **¿Por qué existe este step?**
> La imagen base de este laboratorio (`ubuntu`) trae Docker, pero **no** trae el
> plugin de Compose. Si corrés `docker compose ...` vas a ver esto:
>
> ```
> unknown shorthand flag: 'f' in -f
> Usage:  docker [OPTIONS] COMMAND [ARG...]
> ```
>
> No es un error de tu `compose.yml` ni del `docker` que tenés: es que **el
> comando `docker compose` no existe en esta máquina**. Docker ofrece dos formas
> de invocar Compose:
>
> | Forma | Qué es | ¿Está acá? |
> |---|---|---|
> | `docker compose` | plugin del CLI (Docker ≥ 19.03) | ❌ no viene en la imagen |
> | `docker-compose` | binario standalone (Compose V2) | ✅ lo instalamos abajo |
>
> **En este taller vamos a usar `docker-compose` (con guion).** Es el mismo motor
> —Compose V2—, con la misma sintaxis y los mismos archivos `compose.yml`. La
> diferencia es solo de invocación.

## 1) Diagnóstico: ¿qué hay instalado?

```bash
docker --version
docker compose version 2>&1 | head -2
docker-compose version 2>&1 | head -2
```{{exec}}

## 2) Instalación de Compose V2 (standalone, con verificación de checksum)

> Es idempotente: si ya está instalado, no hace nada. Tarda ~30 s la primera vez
> (descarga ~60 MB).

```bash
if docker-compose version 2>/dev/null | grep -q "v2\."; then
  echo "Compose V2 ya estaba instalado. No hace falta instalar nada."
  docker-compose version
else
  COMPOSE_VERSION=v2.29.7
  ARCH="$(uname -m)"
  case "$ARCH" in
    x86_64)        ASSET=docker-compose-linux-x86_64 ;;
    aarch64|arm64) ASSET=docker-compose-linux-aarch64 ;;
    armv7l)        ASSET=docker-compose-linux-armv7 ;;
    *) echo "Arquitectura no soportada: $ARCH — avisale al docente." ;;
  esac
  BASE="https://github.com/docker/compose/releases/download/${COMPOSE_VERSION}"
  mkdir -p /tmp/compose-install && cd /tmp/compose-install
  echo "Descargando $ASSET ($COMPOSE_VERSION)..."
  if ! command -v curl >/dev/null 2>&1 && ! command -v wget >/dev/null 2>&1; then
    echo "No hay curl ni wget — instalando curl con apt..."
    apt-get update -qq && apt-get install -y -qq curl
  fi
  if command -v curl >/dev/null 2>&1; then
    curl -fsSL -o "$ASSET" "$BASE/$ASSET" && curl -fsSL -o checksums.txt "$BASE/checksums.txt"
  else
    wget -q -O "$ASSET" "$BASE/$ASSET" && wget -q -O checksums.txt "$BASE/checksums.txt"
  fi
  grep -F "*$ASSET" checksums.txt | head -1 | sha256sum -c -
  install -m 0755 "$ASSET" /usr/local/bin/docker-compose
  cd ~
  docker-compose version
fi
```{{exec}}

## 3) Habilitar también `docker compose` (opcional)

Esto copia el mismo binario al directorio de plugins del CLI. **Solo sirve si tu
Docker CLI es 19.03 o newer** — en un CLI viejo el subcomando `docker compose`
simplemente no existe, y no hay forma de habilitarlo.

```bash
mkdir -p ~/.docker/cli-plugins
cp /usr/local/bin/docker-compose ~/.docker/cli-plugins/docker-compose
docker compose version 2>&1 | head -2
```{{exec}}

## 4) Verificación final

```bash
docker-compose version
docker-compose ls
```{{exec}}

## ¿Qué deberías ver?

- `docker-compose version` imprime una versión **v2.x** (no "1.x": la v1 es
  Python y no entiende el formato de `compose.yml` que vamos a usar).
- `docker-compose ls` **no se queja** de nada. Si lo hiciste bien, muestra una
  tabla vacía: todavía no hay stacks.
- `docker compose version` (paso 3) puede o no funcionar según la versión de tu
  CLI. **Si falla, no es un problema**: usá `docker-compose` con guion.

## Si algo falla

| Síntoma | Causa probable | Solución |
|---|---|---|
| `sha256sum: WARNING: 1 computed checksum did NOT match` | descarga corrupta o proxy que modifica el tráfico | Repetí el paso 2 |
| `curl: (6) Could not resolve host` | sin salida a internet | Repetir o avisar al docente (hay imagen alternativa) |
| `docker-compose: command not found` | la instalación no terminó | Volvé a correr el paso 2 y mirá el error anterior |
| `docker-compose ls` dice `client version is too old` | daemon Docker muy antiguo | Avisale al docente: hay que usar otra imagen |

## Ten en cuenta que…

- **Compose no viene con Docker**: es un proyecto aparte (docker/compose). Por
  eso hay que instalarlo explícitamente — y por eso existen dos nombres.
- **El checksum no es paranoia**: verificás que el binario que descargaste sea
  exactamente el que publicó Docker. Es el mismo control que hacés con una
  imagen: `docker pull` + `docker inspect` te dan el digest; acá el digest lo
  verificamos con `sha256sum`.
- **En producción no hacés esto a mano**: el Compose va en la imagen de la
  pipeline (o como plugin del runner de CI), versionado y auditado.
