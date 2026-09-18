# 9) Compose: WordPress + MariaDB (backup/restore)

## Objetivo
Levantar una app multi-servicio **real** (WordPress + MariaDB) con Compose,
configurar persistencia con volúmenes, hacer backup de la DB con `mysqldump`
desde `docker exec`, destruir todo con `down -v`, y restaurar el backup.

> **¿Por qué WordPress y no otra cosa?**
> Es la combinación canónica: una app web (PHP) + una base de datos (SQL).
> El 90% de las apps tienen esta forma (Ghost, Discourse, Moodle, Shopify…).
> La escala la pone el orquestador, no el patrón.

```bash
mkdir -p wp-lab && cd wp-lab
```{{exec}}

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

Levantar:
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
Posts → Añadir nuevo → "Post de prueba del taller" → Publicar.

## Backup
```bash
docker compose -f compose.yml exec db \
  sh -c 'exec mariadb-dump -uwp -pwppw wordpress' > backup.sql
```{{exec}}

```bash
ls -lh backup.sql
```{{exec}}

## Destruir todo (simula un desastre)
```bash
docker compose -f compose.yml down -v
```{{exec}}

> `-v` borra los volúmenes. **La DB se perdió**.

## Recrear y restaurar
```bash
docker compose -f compose.yml up -d
```{{exec}}

Esperá a que termine. El wizard vuelve a aparecer.

Restaurá el backup:
```bash
cat backup.sql | docker compose -f compose.yml exec -T db \
  mariadb -uwp -pwppw wordpress
```{{exec}}

Refrescá `http://<KC-puerto-8000>`. **El post "Post de prueba del taller"
debería estar ahí**.

Limpieza:
```bash
docker compose -f compose.yml down -v
cd ..
```{{exec}}

## ¿Qué deberías ver?
- Después del restore, el post original está vivo.
- Sin el backup, hubieras perdido todo al hacer `down -v`.

## Ten en cuenta que…
- En producción los backups se automatizan; el patrón `mysqldump` se reemplaza
  por backups incrementales o snapshots del volumen.
- **El secreto está en el volumen**: sin volumen ni backup, un `down -v`
  destruye todo. Cualquiera de los dos te salva.
