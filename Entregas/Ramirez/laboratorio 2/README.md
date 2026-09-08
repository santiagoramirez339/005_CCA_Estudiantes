# Laboratorio 2 - Agente de Inteligencia Artificial

Este laboratorio implementa un agente de inteligencia artificial utilizando Python, FastAPI, Docker, PostgreSQL y un modelo de lenguaje ejecutado localmente mediante Ollama.

## Tecnologías utilizadas

- Python
- FastAPI
- Docker
- PostgreSQL
- Ollama
- Qwen
- Tavily API

## Estructura del proyecto

El código principal se encuentra en la carpeta:

`Sesion03_agentes-ai-public-v2`

Dentro del proyecto se encuentran los módulos encargados de la lógica del agente, la interfaz web, las herramientas de búsqueda y la configuración del entorno Docker.

## Ejecución

Primero se debe tener Docker Desktop y Ollama ejecutándose.

Construir la imagen:

```bash
docker build -t agentic-ai-v2 .
