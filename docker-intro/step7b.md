# 7b) Compose: WordPress + MariaDB (primera mirada)

## Objetivo
Levantar una **app multi-servicio real** (WordPress + MariaDB) con Compose y ver
cómo dos containers cooperan en la misma red. Versión **light** — sin backup ni
restore. La versión completa (con backup, `down -v`, restore) se ve en la
**Parte 2, step 9**.

> **¿Por qué WordPress y no otra cosa?**
> Es la combinación canónica: una app web (PHP) + una base de datos (SQL).
> El 90% de las apps tienen esta forma (Ghost, Discourse, Moodle, Shopify…).
> Lo que hoy te parece "mucho stack" mañana es el piso de cualquier deploy.

## Crear el proyecto

```bash
mkdir -p wp-lab && cd wp-lab
```{{exec}}

## Escribir el `compose.yml`

```bash
cat > compose.yml << 'EOF'
services:
  db:
    image: mariadb:11
    environment:
      MARIADB_ROOT_PASSWORD: rootpw
      MARIADB_DATABASE: wordpress
      MARIADB_USER: wp
      MARIADB_PASSWORD: wppw
    volumes:
      - dbdata:/var/lib/mysql
    healthcheck:
      test: ["CMD", "mariadb-admin", "ping", "-h", "localhost"]
      interval: 5s
      timeout: 3s
      retries: 10

  wordpress:
    image: wordpress:6-apache
    depends_on:
      db:
        condition: service_healthy
    ports:
      - "8000:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wp
      WORDPRESS_DB_PASSWORD: wppw
      WORDPRESS_DB_NAME: wordpress

volumes:
  dbdata:
EOF
```{{exec}}

## Levantar el stack

```bash
docker compose -f compose.yml up -d
```{{exec}}

Estado:

```bash
docker compose -f compose.yml ps
```{{exec}}

## Acceder a WordPress

Abrí la URL del puerto 8000 en KC. Completá el wizard:
- Idioma: español
- Título: el que quieras
- Usuario: `admin` / pass: lo que quieras (anotalá)

## Crear un post de prueba

Posts → Añadir nuevo → "Post de prueba de la Parte 1" → Publicar.

## ¿Qué deberías ver?

- WordPress responde en `localhost:8000` con el wizard de instalación.
- Tras completar el wizard y publicar un post, lo ves en la home.
- `docker compose ps` muestra **2 servicios healthy**: `db` y `wordpress`.

## Inspección rápida (qué te llevás a casa)

```bash
docker compose -f compose.yml logs --tail 20 db
```{{exec}}

```bash
docker compose -f compose.yml logs --tail 20 wordpress
```{{exec}}

Mirá cómo WordPress loguea conexiones a la DB y la DB loguea queries. **Dos
procesos cooperando en una red custom**, administrados por un solo `compose.yml`.

## Limpieza (importante antes de la Parte 2)

```bash
docker compose -f compose.yml down -v
```{{exec}}

> `-v` borra el volumen `dbdata`. El próximo step de KC (7c) te lo recuerda.
> En la Parte 2 vamos a usar **el mismo ejemplo con backup/restore incluido** —
> hoy era solo para que veas "lo que se viene".

```bash
cd ..
```{{exec}}

## Ten en cuenta que…

- **Un solo archivo** (`compose.yml`) define y conecta **dos servicios**.
- **`depends_on: condition: service_healthy`** le dice a WordPress "no arranques
  hasta que la DB responda a ping". Sin esto, WP arranca antes y se confunde.
- **`volumes: dbdata`** es lo que vuelve persistente la DB. Sin esto, `down -v`
  borra la DB con el stack.
- La **red custom** la crea Compose automáticamente. Los servicios se llaman por
  nombre (`db`, `wordpress`) sin que tengas que mapear puertos a mano.