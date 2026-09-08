# Laboratorio 2 — Agente de Inteligencia Artificial

**Universidad de Antioquia**  
**Estudiante:** Santiago Ramírez Puentes  
**Laboratorio:** 2  
**Año:** 2026

---

## Descripción

Este laboratorio presenta la implementación y despliegue de un **sistema de agentes de inteligencia artificial orientado a tareas de investigación y generación de reportes**.

La aplicación está desarrollada principalmente en **Python** y utiliza **FastAPI** para exponer los servicios del sistema y proporcionar una interfaz de interacción. El modelo de lenguaje se ejecuta localmente mediante **Ollama**, mientras que las herramientas de investigación permiten complementar las capacidades del agente con búsqueda de información externa.

El proyecto se ejecuta dentro de un entorno **Docker**, lo que permite encapsular las dependencias y facilitar su reproducción en diferentes equipos. Adicionalmente, se utiliza **PostgreSQL** como componente de persistencia durante la ejecución.

---

## Objetivo

El objetivo del laboratorio es implementar una arquitectura de agentes capaz de recibir una solicitud del usuario y coordinar diferentes componentes para:

- analizar la solicitud;
- planificar la tarea;
- realizar procesos de investigación;
- utilizar herramientas externas cuando sea necesario;
- interactuar con un modelo de lenguaje local;
- generar un reporte;
- y permitir consultar el progreso de la tarea.

---

## Arquitectura general

El funcionamiento general del sistema puede representarse de la siguiente manera:

```text
                    ┌──────────────────────┐
                    │       Usuario        │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │    Interfaz Web      │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │       FastAPI        │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Sistema de Agentes   │
                    └──────────┬───────────┘
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
                 ▼                           ▼
        ┌─────────────────┐        ┌─────────────────┐
        │ Planning Agent  │        │ Research Tools  │
        └────────┬────────┘        └────────┬────────┘
                 │                          │
                 │                          ▼
                 │                    ┌───────────┐
                 │                    │  Tavily   │
                 │                    └───────────┘
                 │
                 ▼
        ┌─────────────────┐
        │     Ollama      │
        │   Modelo LLM    │
        └────────┬────────┘
                 │
                 ▼
        ┌─────────────────┐
        │ Reporte final   │
        └─────────────────┘
```

Docker proporciona el entorno de ejecución de la aplicación y PostgreSQL se inicializa como parte de la infraestructura utilizada por el proyecto.

---

## Tecnologías utilizadas

| Tecnología | Función |
|---|---|
| **Python** | Lenguaje principal del proyecto |
| **FastAPI** | API y servidor de la aplicación |
| **Uvicorn** | Servidor ASGI |
| **Docker** | Contenerización del sistema |
| **PostgreSQL** | Persistencia de información |
| **Ollama** | Ejecución local del modelo de lenguaje |
| **Qwen** | Modelo de lenguaje utilizado en la configuración local |
| **Tavily API** | Herramienta de búsqueda para investigación |
| **HTML / CSS** | Interfaz web |
| **GitHub** | Control de versiones y entrega del proyecto |

---

## Estructura del proyecto

El código fuente se encuentra en:

```text
Sesion03_agentes-ai-public-v2/
```

La estructura principal es:

```text
Sesion03_agentes-ai-public-v2/
│
├── docker/
│   └── entrypoint.sh
│
├── scripts/
│   └── test_ollama_connection.py
│
├── src/
│   ├── agents.py
│   ├── planning_agent.py
│   └── research_tools.py
│
├── static/
│   ├── arxiv_logo.png
│   ├── dl_logo.png
│   ├── how_interactions_are_performed.png
│   ├── tavily_logo.svg
│   └── wikipedia_logo.png
│
├── templates/
│   └── index.html
│
├── .env.example
├── .gitignore
├── Dockerfile
├── Informe_Docker_Windows.docx.pdf
├── main.py
└── requirements.txt
```

---

## Componentes principales

### `main.py`

Contiene la aplicación desarrollada con FastAPI. Desde este archivo se administra la interacción entre la interfaz web y el sistema encargado de procesar las solicitudes.

También expone los endpoints utilizados para iniciar y consultar las tareas del agente.

### `src/agents.py`

Contiene la lógica asociada con los agentes y la interacción con el modelo de lenguaje.

### `src/planning_agent.py`

Implementa la lógica relacionada con la planificación de las tareas que debe realizar el sistema.

### `src/research_tools.py`

Contiene herramientas utilizadas durante la etapa de investigación y recuperación de información.

### `scripts/test_ollama_connection.py`

Permite realizar pruebas relacionadas con la comunicación con Ollama.

### `templates/index.html`

Define la interfaz web principal desde la cual el usuario puede interactuar con la aplicación.

### `docker/entrypoint.sh`

Inicializa los servicios requeridos dentro del contenedor, configura PostgreSQL y posteriormente inicia el servidor de la aplicación.

### `Dockerfile`

Define el entorno Docker del proyecto, instala las dependencias necesarias y configura la ejecución de la aplicación.

---

## Requisitos

Para ejecutar el proyecto se requiere tener instalado:

- **Docker Desktop**
- **Ollama**
- un modelo compatible disponible en Ollama;
- una clave válida para **Tavily API**;
- conexión a Internet para las funcionalidades que utilizan servicios externos.

Las dependencias de Python utilizadas por la aplicación se encuentran definidas en:

```text
requirements.txt
```

y son instaladas durante la construcción de la imagen Docker.

---

## Configuración de variables de entorno

El proyecto utiliza un archivo:

```text
.env
```

para almacenar la configuración local y las credenciales necesarias.

Por razones de seguridad, el archivo `.env` real **no se encuentra incluido en este repositorio**.

En su lugar se proporciona:

```text
.env.example
```

como plantilla de configuración.

Para ejecutar el proyecto se debe crear una copia de `.env.example`:

```bash
cp .env.example .env
```

En Windows también puede realizarse manualmente copiando el archivo y cambiando su nombre a:

```text
.env
```

Posteriormente deben configurarse las variables correspondientes, especialmente la clave de Tavily y la configuración del modelo local.

> **Importante:** la configuración exacta esperada por el proyecto se encuentra documentada en `.env.example`.

Nunca se deben publicar claves reales de API en GitHub.

---

# Ejecución

## 1. Iniciar Docker Desktop

Abrir **Docker Desktop** y esperar hasta que el motor de Docker se encuentre disponible.

Puede comprobarse mediante:

```bash
docker version
```

---

## 2. Verificar Ollama

Comprobar que Ollama se encuentra instalado:

```bash
ollama --version
```

Consultar los modelos disponibles:

```bash
ollama list
```

También puede comprobarse que el servidor de Ollama está respondiendo mediante:

```bash
curl http://localhost:11434/api/tags
```

Si el modelo configurado en el archivo `.env` no está instalado, debe descargarse previamente con Ollama.

Por ejemplo:

```bash
ollama pull qwen3:4b-instruct
```

---

## 3. Ubicarse en la carpeta del proyecto

Desde una terminal, ingresar a:

```text
Sesion03_agentes-ai-public-v2
```

Todos los siguientes comandos deben ejecutarse desde esta carpeta.

---

## 4. Construir la imagen Docker

Ejecutar:

```bash
docker build -t agentic-ai-v2 .
```

Este comando:

1. crea la imagen Docker;
2. instala las dependencias de Python;
3. copia el código de la aplicación;
4. configura el script de inicio;
5. y prepara el entorno necesario para ejecutar el sistema.

---

## 5. Ejecutar el contenedor

En Windows se puede iniciar la aplicación mediante:

```bash
docker run --rm -it -p 8000:8000 --env-file .env --name fpsvc-v2 agentic-ai-v2
```

El parámetro:

```text
-p 8000:8000
```

establece la comunicación entre el puerto `8000` del computador anfitrión y el puerto `8000` del contenedor.

El parámetro:

```text
--env-file .env
```

carga las variables de entorno requeridas por la aplicación.

Durante el inicio se configura PostgreSQL y posteriormente se ejecuta FastAPI mediante Uvicorn.

Una ejecución correcta debe mostrar finalmente un mensaje similar a:

```text
Uvicorn running on http://0.0.0.0:8000
```

---

## Interfaz de usuario

Con el contenedor ejecutándose, la interfaz principal puede abrirse desde el navegador en:

```text
http://localhost:8000/
```

Desde esta interfaz el usuario puede interactuar con el sistema y solicitar la generación de reportes.

---

## Documentación de la API

FastAPI genera automáticamente una interfaz para consultar y probar los endpoints disponibles.

Se encuentra en:

```text
http://localhost:8000/docs
```

Los principales endpoints de la aplicación son:

| Método | Endpoint | Función |
|---|---|---|
| `GET` | `/` | Carga la interfaz principal |
| `GET` | `/api` | Verificación de la API |
| `POST` | `/generate_report` | Inicia la generación de un reporte |
| `GET` | `/task_progress/{task_id}` | Consulta el progreso de una tarea |
| `GET` | `/task_status/{task_id}` | Consulta el estado de una tarea |

---

## Flujo de ejecución

El flujo general del sistema es:

```text
Solicitud del usuario
        │
        ▼
    FastAPI
        │
        ▼
Planificación de la tarea
        │
        ▼
Sistema de agentes
        │
   ┌────┴─────┐
   │          │
   ▼          ▼
Ollama    Herramientas
 + LLM    de investigación
   │          │
   └────┬─────┘
        │
        ▼
Procesamiento de información
        │
        ▼
Generación del reporte
        │
        ▼
Resultado para el usuario
```

De esta manera, el modelo de lenguaje no constituye por sí solo toda la aplicación. El sistema incorpora diferentes componentes encargados de coordinar la planificación, investigación, generación de contenido y comunicación con el usuario.

---

## Docker y PostgreSQL

Docker permite encapsular las dependencias de la aplicación y proporcionar un entorno reproducible.

Durante el inicio del contenedor, el script:

```text
docker/entrypoint.sh
```

se encarga de preparar PostgreSQL y posteriormente iniciar la aplicación mediante Uvicorn.

De manera simplificada:

```text
┌─────────────────────────────┐
│      Contenedor Docker      │
│                             │
│  ┌───────────────────────┐  │
│  │       FastAPI         │  │
│  └──────────┬────────────┘  │
│             │               │
│  ┌──────────▼────────────┐  │
│  │ Sistema de agentes    │  │
│  └───────────────────────┘  │
│                             │
│  ┌───────────────────────┐  │
│  │      PostgreSQL       │  │
│  └───────────────────────┘  │
│                             │
└─────────────────────────────┘
             │
             ▼
      Ollama / Servicios
          externos
```

---

## Compatibilidad entre Windows y Linux

Durante la ejecución del proyecto en Windows se presentó una diferencia entre los finales de línea utilizados por Windows (`CRLF`) y Linux (`LF`).

Esto podía producir el error:

```text
env: 'bash\r': No such file or directory
```

al intentar ejecutar:

```text
docker/entrypoint.sh
```

Para garantizar la compatibilidad, el `Dockerfile` normaliza los finales de línea antes de ejecutar el script:

```dockerfile
RUN sed -i 's/\r$//' /entrypoint.sh
```

Posteriormente se asignan los permisos de ejecución correspondientes.

Esta modificación permite construir la imagen desde Windows y ejecutar correctamente el script dentro del entorno Linux utilizado por Docker.

---

## Detener la aplicación

Si el contenedor se está ejecutando en primer plano, puede detenerse mediante:

```text
Ctrl + C
```

También puede detenerse desde otra terminal:

```bash
docker stop fpsvc-v2
```

El parámetro:

```text
--rm
```

utilizado al iniciar el contenedor hace que Docker elimine automáticamente el contenedor cuando finaliza su ejecución.

La imagen:

```text
agentic-ai-v2
```

permanece disponible y puede utilizarse nuevamente.

Por lo tanto, no es necesario ejecutar `docker build` cada vez que se inicia el proyecto, salvo que se hayan realizado cambios que requieran reconstruir la imagen.

---

## Seguridad

El proyecto utiliza `.gitignore` para evitar que información privada sea incorporada al repositorio.

En particular:

```gitignore
.env
```

impide versionar el archivo local que puede contener credenciales.

La distribución correcta es:

```text
.env           → configuración privada, no se publica
.env.example   → plantilla pública de configuración
```

Cada usuario debe proporcionar sus propias credenciales antes de ejecutar el proyecto.

---

## Reproducibilidad

Una de las principales ventajas de la implementación es la posibilidad de reproducir el entorno utilizando Docker.

El procedimiento general en un nuevo equipo es:

```text
1. Obtener el proyecto
        ↓
2. Instalar/iniciar Docker Desktop
        ↓
3. Instalar/iniciar Ollama
        ↓
4. Instalar el modelo requerido
        ↓
5. Crear .env a partir de .env.example
        ↓
6. Configurar las credenciales
        ↓
7. Construir la imagen Docker
        ↓
8. Ejecutar el contenedor
        ↓
9. Abrir localhost:8000
```

Esto reduce los problemas asociados con diferencias entre versiones de Python, dependencias y configuraciones locales.

---

## Resultado del laboratorio

El resultado final corresponde a una aplicación de inteligencia artificial con una arquitectura modular que integra:

- agentes especializados;
- planificación de tareas;
- herramientas de investigación;
- un modelo de lenguaje ejecutado localmente;
- búsqueda de información mediante servicios externos;
- una API REST desarrollada con FastAPI;
- una interfaz web;
- PostgreSQL;
- y un entorno reproducible mediante Docker.

La implementación permite observar cómo un sistema basado en agentes puede combinar un modelo de lenguaje con herramientas, planificación e infraestructura de software para desarrollar tareas más complejas que una interacción directa con un LLM.

---

## Autor

**Santiago Ramírez Puentes**  
Universidad de Antioquia  
2026
