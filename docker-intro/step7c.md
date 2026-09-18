# 7c) ☕ Cierre — Fin de la Parte 1

> **Pausa.** Este step **no tiene comandos para correr**. Es el corte entre la
> Clase 1 (hoy) y la Clase 2 (próxima). Leelo, respirá, y avisale al docente
> cuando estés listo para el mini-quiz oral.

## Lo que lograste hoy (Parte 1 completa)

Marcá mentalmente lo que podés hacer sin mirar la guía:

- [ ] Correr `docker run hello-world` y entender qué pasó.
- [ ] Listar imágenes y contenedores (`docker images`, `docker ps -a`).
- [ ] Ver logs y entrar a un contenedor corriendo (`logs`, `exec`).
- [ ] Crear un volumen y entender por qué persiste entre `rm`.
- [ ] Crear una red custom y demostrar DNS por nombre.
- [ ] Levantar WordPress + MariaDB con un `compose.yml`.

Si tenés **5 de 6**, estás listo para la Parte 2.

## Lo que viene la próxima clase (Parte 2)

| Bloque | KC step |
|---|---|
| Dockerfile single-stage (live coding) | step8 |
| Multi-stage + `.dockerignore` + `USER` | step8b |
| **Compose a fondo**: backup con `mysqldump`, `down -v`, restore | step9 |
| Swarm: demo de orquestación (BONUS, en mantenimiento) | step10 |
| Seguridad básica (mini-lab) | step11 |

## Cleanup recomendado antes de cerrar

Si todavía tenés algo corriendo del Compose light (step 7b), limpialo:

```bash
docker compose -f wp-lab/compose.yml down -v 2>/dev/null || true
docker container ls -a
```{{exec}}

La salida de `docker container ls -a` debería estar **vacía** (o tener solo el
shell de KC, que se cierra solo).

## Tarea para casa (opcional, ~20 min)

Si querés llegar con más soltura a la Parte 2:

1. Releé `parte-1-fundamentos/cheatsheet.md` (1 página).
2. Corré los 5 comandos del recap en tu sesión de KC — sin mirar la guía.
3. Si te pinta, explorá el desafío de casa: `docker-workshop/desafio-casa.md`
   (OWASP Juice Shop — **exploración libre**, no exploiting).

## Mini-quiz oral (cuando estés listo)

Avisale al docente y que te tome estas 5 preguntas en voz alta:

1. ¿Qué pasa si borrás un container con `docker rm`? ¿Y con `docker rm -f`?
2. ¿Cómo listás solo los containers que están corriendo?
3. ¿Cuál es la diferencia entre `docker pull` y `docker run`?
4. Si dos containers están en la misma red custom, ¿cómo se llaman entre sí?
5. ¿Qué pasa con los datos de un container cuando lo borrás sin volumen?

5/5 correctas → arrancás la Parte 2 con buen piso.
3–4/5 → dale una releída a los ejercicios 02 y 05 antes de la próxima.
<2/5 → hablá con el docente, te armamos un plan de nivelación.

---

**Nos vemos en la Parte 2.** 🐳