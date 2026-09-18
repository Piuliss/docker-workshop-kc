# Publicar el escenario en KillerCoda

Esta carpeta es un **repo Git listo para pushear** a GitHub y luego importar a
KillerCoda. Los pasos exactos:

## Pre-requisitos

- Cuenta en [github.com](https://github.com)
- Cuenta en [killercoda.com](https://killercoda.com)
- Haber commiteado los cambios localmente (ver más abajo)

## Paso 1 — Verificar el estado local

```bash
cd docker-workshop/killer-coda/docker-workshop-kc
git log --oneline -5
```

Deberías ver al menos un commit con los 12 steps.

## Paso 2 — Crear el repo en GitHub

Ir a https://github.com/new y crear un repo nuevo. Sugerencias:

- **Nombre**: `docker-workshop-kc` (o el que prefieras)
- **Visibilidad**: público (KillerCoda necesita acceso)
- **NO inicializar** con README, .gitignore ni licencia (ya los tenemos)

Si querés usar el mismo nombre que el upstream (`Piuliss/docker-workshop-kc`),
podés hacer un **fork** desde https://github.com/Piuliss/docker-workshop-kc
y trabajar sobre el fork.

## Paso 3 — Conectar el repo local con GitHub

Reemplazá `<USER>` por tu usuario de GitHub:

```bash
cd docker-workshop/killer-coda/docker-workshop-kc
git remote add origin git@github.com:<USER>/docker-workshop-kc.git
git branch -M main
git push -u origin main
```

Si usás HTTPS en vez de SSH:

```bash
git remote add origin https://github.com/<USER>/docker-workshop-kc.git
```

## Paso 4 — Importar en KillerCoda

1. Ir a https://killercoda.com/creators (o https://killercoda.com → "Creator")
2. Click **"Import from GitHub"**
3. Pegar la URL del repo: `https://github.com/<USER>/docker-workshop-kc`
4. KillerCoda detecta automáticamente `structure.json` y crea el escenario
   `docker-intro` con los 12 steps
5. Click **"Publish"** y copiar el link público

## Paso 5 — Compartir el link con los alumnos

El link se ve así: `https://killercoda.com/<USER>/docker-intro`

Compartilo por el aula virtual al inicio de la Sesión 1.

## Verificación post-importación

Para validar que KC cargó todo:

```bash
# Abrir cada step en el browser y verificar que el primer comando se puede ejecutar
```

Si algún step falla al cargar, revisar:
- Que `docker-intro/index.json` referencie archivos que existen (ej: `step8b.md`)
- Que la sintaxis `{{exec}}` esté en cada bloque de código que querés ejecutable

## Actualizar el escenario después

```bash
# Después de editar archivos
git add .
git commit -m "fix: ..."
git push

# En killercoda.com, click "Refresh from GitHub" en el escenario
```

## Si KC no detecta el escenario

Verificar:
1. `structure.json` está en la **raíz** del repo y dice `{"items": [{"path": "docker-intro"}]}`
2. `docker-intro/index.json` tiene el formato correcto con `title`, `description`,
   `details.steps[]`
3. El repo en GitHub es público
4. KC tiene permisos para leer el repo (revocar y reconectar OAuth si hace falta)

## Backup del link público

Una vez publicado, **guardá el link**. KC no te lo notifica si cambia.
Sugerencia: anotalo en `../README.md` en la sección "Acceso al laboratorio".
