# Cierre + evidencias

## Lo mínimo que debes dominar
- `docker image pull/ls/history/build`
- `docker container run/ls/logs/exec/inspect/diff/cp/stats`
- `docker network create/inspect`
- `docker volume create/inspect`
- `docker compose up/down/ps/logs`

## Mini-desafíos (10–15 min)
1) Lanza `nginx` mapeando a otro puerto (ej. 8099) y demuestra con `inspect` qué IP interna tiene.  
2) Crea una red custom y prueba DNS por nombre entre dos contenedores.  
3) Crea un volumen, escribe un archivo desde un contenedor, léelo desde otro.  
4) (Extra) Modifica `index.html`, rebuild `uni/web:1.1`, corre en otro puerto y compara `docker image ls`.

> Entregable sugerido: un txt/MD con comandos usados + outputs clave (copiados) + 5 líneas de explicación.
