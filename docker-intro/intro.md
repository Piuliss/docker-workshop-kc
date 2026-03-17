# Taller Introductorio de Docker 

![Docker Workshop](docker-full.png)

**Objetivo**: que puedas explicar y ejecutar con evidencia:
- Imagen vs contenedor
- Ciclo de vida de contenedores
- Inspección (metadatos, filesystem, recursos)
- Redes y persistencia
- Compose como “mínimo viable” de despliegue
- **Dockerfile**: crear una imagen propia (build) y publicarla por puerto
- **Bonus**: Swarm en 1 nodo (services/stack/scale)

## ¿Cómo usar este taller?
- **Modo recomendado**: ejecuta cada bloque y valida el “qué deberías ver” antes de seguir.
- **Si algo falla**: vuelve a correr el comando, revisa el error, y usa `docker logs` / `docker ps -a` / `docker inspect` como “fuente de verdad”.
- **Ritmo sugerido**: pasos 1–7 (75–90 min), Dockerfile (20 min), Compose (25 min), Swarm bonus (10–15 min).

## Reglas
1) Copia/pega comandos, pero **lee el objetivo** antes de ejecutar.  
2) Cada paso termina con “Ten en cuenta que…” con notas o tips importantes de Docker.  
3) Al final tienes mini-desafíos opcionales para que **te auto-chequees**, no hay entregables.

## UI (KillerCoda)
- Para abrir servicios HTTP, usa el selector de tráfico/puertos en la UI.
- En pasos con web, verás enlaces o variables como `{{TRAFFIC_HOST1_8090}}` para abrir la página directo.
