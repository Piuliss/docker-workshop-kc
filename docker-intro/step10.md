# 10) Swarm: Demo de Orquestación (BONUS)

## Objetivo
Entender el modelo "servicio con réplicas" que comparten Swarm, Kubernetes,
ECS y Nomad. **No es objetivo volverse productivo en Swarm** (está en modo
mantenimiento desde 2023); es entender el modelo mental.

Init del swarm:
```bash
docker swarm init
```{{exec}}

Crear un servicio web con 3 réplicas:
```bash
docker service create --name websvc -p 8088:80 --replicas 3 nginx:alpine
```{{exec}}

```bash
docker service ls
```{{exec}}

```bash
docker service ps websvc
```{{exec}}

Escalar:
```bash
docker service scale websvc=5
```{{exec}}

Rolling update:
```bash
docker service update --image nginx:latest websvc
```{{exec}}

```bash
docker service ps websvc
```{{exec}}

Borrar servicio:
```bash
docker service rm websvc
```{{exec}}

Salir del swarm:
```bash
docker swarm leave --force
```{{exec}}

## ¿Qué deberías ver?
- `docker service ls` muestra `websvc` con réplicas activas.
- `docker service ps` lista las tareas distribuidas en el cluster.
- El rolling update cambia la imagen tarea por tarea (no downtime).

## Modelo mental

| Concepto | docker run | docker compose | docker service |
|---|---|---|---|
| Unidad | container | stack local | servicio orquestado |
| Réplicas | manual (varios `run`) | manual | declarativas |
| Distribución | un host | un host | cluster |
| Self-healing | no | no | sí |

## Ten en cuenta que…
- Lo importante de Swarm **no es Swarm**. Es entender que cualquier
  orquestador moderno responde a la misma pregunta: "yo te digo el estado
  deseado, vos mantenelo".
- Si te interesa el tema, saltá directo a **Kubernetes**. Swarm no es una
  inversión rentable hoy.
