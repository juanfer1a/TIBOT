# Basdonax AI RAG v1.0

Este repositorio contiene todo lo necesario para poder desplegar TIBOT chat con Inteligencia Artificial para el area de soporte tecnico y recursos humanos de TI724, que utiliza los modelos open source de Meta y de Microsoft: `Llama3-7b` y `Phi3-4b` dando la posibilidad de subir documentos y hacer consultas a los mismos.

## Requisitos previos

- Docker o Docker desktop: https://www.docker.com/products/docker-desktop/
- **(opcional)** Tarjeta gráfica RTX

## Instalación

### Elección del modelo de datos (LLM)

Esta instalación, nos brida la posibilidad si tenemos o no una tarjeta gráfica capaz de utilizar Llama3-7b o no. Si tenemos una tarjeta gráfica capaz de utilizar este modelo de datos utilizaremos el archivo `docker-compose.yml`, si no contamos con esa posibilidad vamos a eliminar el `docker-compose.yml` y vamos a renombrar el archivo `docker-compose_sin_gpu.yml` por `docker-compose.yml`. La diferencia entre un archivo y otro es que el `docker-compose_sin_gpu.yml` utiliza el LLM `Phi3-4b`, que es mucho más ligero para correrlo en el procesador de tu PC, mientras que `Llama3-7b` es mucho más pesado y si bien puede correr en CPU, es más recomendable una gráfica.

#### Docker

Tenemos que tener Docker o Docker Desktop instalado, en este video tenemos una guia para instalar Docker: https://www.youtube.com/watch?v=ZyBBv1JmnWQ

Una vez instalado y prendido el Docker Desktop si lo estamos utilizando, vamos a ejecutar en esta misma carpeta:

```
docker-compose up
```

La primera vez vamos a tener que esperar a que todo se instale correctamente, va a tardar unos cuantos minutos en ese paso.

Ahora tenemos que instalarnos nuestro modelo LLM, si tenemos una GPU que pueda soportar vamos a ejecutar el comando para traernos Llama3, inicialmente en la etapa de pruebas vamos a utilizar  Phi3 (si queremos utilizar otro modelo, en esta pagina: https://ollama.com/library tenemos una lista de todos los modelos open source posibles en esta página.)

```
docker ps
```

Te va a aparecer algo como esto:

```
CONTAINER ID   IMAGE                    COMMAND                  CREATED              STATUS              PORTS                    NAMES
696d2e45ce7c   ui                       "/bin/sh -c 'streaml…"   About a minute ago   Up About a minute   0.0.0.0:8080->8080/tcp   ui-1
28cf32abee50   ollama/ollama:latest     "/bin/ollama serve"      About a minute ago   Up About a minute   11434/tcp                ollama-1
ec09714c3c86   chromadb/chroma:latest   "/docker_entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8000->8000/tcp   chroma-1
```

En esta parte tenés que copiar el `CONTAINER ID` de la imagen llamada `ollama/ollama:latest` y utilizarla para este comando:

Un ejemplo con `Phi3-4b` y mi `CONTAINER ID`

```
docker exec 28cf32abee50 ollama pull phi3
```
docker exec [CONTAINER ID] ollama pull [nombredelmodelo]
```

Un ejemplo con `Llama3-7b` y mi `CONTAINER ID`

```
docker exec 28cf32abee50 ollama pull llama3
```



Ahora vamos a tener que esperar a que se descargue el modelo, una vez hecho esto solo nos queda modificar la prompt:

Esto se va a hacer a nuestro gusto en el archivo `./app/common/assistant_prompt.py`.

Una vez hecho todo lo anterior solo queda un paso: que entremos al siguiente link: http://localhost:8080 para poder utilizar el RAG.

## ¿Como ejecutarlo posteriormente instalado y una vez lo cerremos?

Tenemos que dejarnos en el escritorio el archivo de `open_rag.bat` si estamos en Windows y si estamos en Mac/Linux el `open_rag.sh`

Ahora tenemos que abrirlo y modificarlo, tenemos que agregar la ruta donde hicimos/tenemos el `docker-compose.yml`, por ejemplo mi ruta es:

```
C:\Users\fcore\OneDrive\Desktop\Basdonax\basdonax-rag>
```

Entonces en mi caso va a ser así el `open_rag.bat` (el .sh es lo mismo):

```
cd C:\Users\fcore\OneDrive\Desktop\Basdonax\basdonax-rag
docker-compose up -d
```

Ahora mientras que tengamos el Docker/Docker Desktop prendido y mientras que ejecutemos este archivo vamos a poder acceder al RAG en este link: http://localhost:8080

Próximo paso: disfrutar