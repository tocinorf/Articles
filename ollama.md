# Private AI with Llama
Esta guía te llevará a través de los pasos necesarios para probar un modelo como Llama en un servidor local usando linux y ollama:  descargaremos el modelo de llama que queramos, configuraremos ollama, y probaremos crear modelos personalizados cambiando la configuración.

# Modelos de Llama y sus requerimientos
La última version de Llama es la 3.2 y la tenemos disponible en distintas versiones dependiendo del caso de uso que queramos darle, cada una con sus requerimientos para poder hacerla funcionar de manera óptima.

Si queremos bajar LLama manualmente podemos encontrarla aqui:
https://www.llama.com/llama-downloads/
https://huggingface.co/collections/meta-llama/llama-32-66f448ffc8c32f949b04c8cf

Modelos actuales de llama y requisitos de hardware.
| Modelos              | Descripción                                                                                        | Requerimientos |                                                                                                                                                                                    |
|----------------------|----------------------------------------------------------------------------------------------------|:--------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Llama 3.2 1B`         | Modelo ligero y eficiente para usar en cualquier lugar, dispositivo móvil o edge.     |       CPU      | Procesador multinúcleo                                                               |
|                      | Ejemplo de caso de uso: Chatbots de servicio al cliente.                                                   |       RAM      | Mínimo 16GB Ram                                                                                                                                                                   |
|                      |                                                                                                    |       GPU      | NVIDIA RTX series (para rendimiento óptimo), al menos 4 GB VRAM                                                                                                                                                                                   |
|                      |                                                                                                    |     Almacenamiento | Suficiente para el almacenamiento de los archivos del modelo (Espacio necesario no especificado.)                                                                                                                            |
| `Llama 3.2 3B`         | Modelo abierto multilenguaje flexible con un nivel de razonamiento y generación de texto y código superior.|       CPU      | Procesador multinúcleo                                                                                                                                                                |
|                      | Ejemplo de caso de uso: Generación de contenido campañas de marketing, creación y depuración de codigo.                            |       RAM      | Mínimo 16GB Ram                                                                                                                                                                   |
|                      |                                                                                                    |       GPU      | NVIDIA RTX series (Para rendimiento óptimo), al menos 8 GB VRAM                                                                                                                    |
|                      |                                                                                                    |     Almacenamiento| Suficiente para el almacenamiento de los archivos del modelo (Espacio necesario no especificado.)                                                                                                                       |
| `Llama 3.2 11B Vision` | Modelo opensource de última generación multilenguaje y multimodal con una grande fuente de datos opensource.                                     |       CPU      | Procesador de alto rendimiento con al menos 16 cores (Recomendado AMD EPYC or Intel Xeon)                                                                                                     |
|                      | Ejemplo de caso de uso: Análisis visual de productos y sistemas de recomendación                          |       RAM      | Mínimo: 64GB, Recomendado: 128GB o superior                                                                                                                                          |
|                      |                                                                                                    |       GPU      | GPU de alto rendimiento con al menos 22GB VRAM. Recomendado: NVIDIA A100 (40GB) o A6000 (48GB), Múltiples GPUs se pueden usar en paralelo para producción                                                                                                                    |
|                      |                                                                                                    |     Almacenamiento| NVMe SSD con al menos 100GB de espacio libre (22GB para el modelo)                                                                                                                           |
| `Llama 3.2 90B Vision` | Modelo opensource de última generación multilenguaje y multimodal con una inmensa fuente de datos opensource                                     |       CPU      | Procesador de muy alto rendimiento con al menos 32 cores Recomendado: Última generación de AMD EPYC o Intel Xeon                                                                                    |
|                      | Ejemplo de caso de uso: Análisis complejo de datos y ayuda en la toma de decisiones estrategicas.                 |       RAM      | Mínimo: 256GB  RAM Recomendado: 512GB o superior para rendimiento óptimo                                                                                                       |
|                      |                                                                                                    |       GPU      | GPU de última generación con al menos 180GB VRAM para poder soportar la carga del modelo completo. Recomendado: NVIDIA A100 con al menos 80GB VRAM o superior. Para Inferencia: Múltiples GPU de un nivel inferior en paralelo podria ser una opción. |
|                      |                                                                                                    |     Almacenamiento    | NVMe SSD con al menos  500GB de espacio libre. Aproximadamente 180GB son necesario solo para almacenar el modelo.                                                                                       |                                                                                     |

# Llama 3.2 usando Ollama
Para esta guia hemos usado la última versión de Ubuntu 22.04 pero si quisiermos ollama tambien está disponible para Windows y Mac.

Para más información sobre Ollama y su desarrollo, puedes visitar su [repositorio en GitHub](https://github.com/ollama/ollama) o en su [web](https://ollama.com/)


## Instalar Llama con Ollama en Linux

### 1. Instalación de Ollama

El primer paso es instalar Ollama, una herramienta de línea de comandos para gestionar y ejecutar modelos de lenguaje. La instalación se realiza de forma sencilla usando el siguiente comando:

`curl -fsSL https://ollama.com/install.sh | sh` 

Una vez que se complete la instalación, puedes verificar que Ollama está correctamente instalado ejecutando el comando `ollama` en la terminal, lo cual mostrará las opciones disponibles.

### 2. Descargar y Ejecutar el Modelo Llama

Antes de proceder a ejecutar el modelo, es importante tener en cuenta que Ollama requiere que tu equipo o máquina virtual tenga aproximadamente el doble de la memoria RAM del tamaño del modelo que deseas ejecutar. Esto asegura que el modelo funcione de manera fluida. Para modelos muy grandes podemos seguir los requisitos que describiamos en la sección anterior.

Para correr el modelo Llama 3.2, usa el siguiente comando en la terminal:

`ollama run llama3.2` 

si queremos especificar una versión en concreto podemos usar:

`ollama run llama3.2:1b` o `ollama run llama3.2:3b`

Esto descargará el modelo (si no lo tienes ya) y lo ejecutará en tu entorno de trabajo.

Si queremos probar otros modelos o parametrizaciones distintas podemos consultar las opciones en su [libreria](https://ollama.com/library)
### 3. Crear un Nuevo Modelfile

Ollama permite crear configuraciones personalizadas para los modelos usando un archivo llamado `Modelfile`. Aquí puedes especificar el modelo base, parámetros de configuración, y ajustes de personalización para crear una experiencia única.

Para crear un archivo `Modelfile`, abre un editor de texto como `vi` y escribe el siguiente contenido:

```text
FROM llama3.2:3b

# set the temperature to 1 [higher is more creative, lower is more coherent]
PARAMETER temperature 1

# set the system prompt
SYSTEM """
You are Mario from Super Mario Bros. Answer as Mario, the assistant, only.
"""
```

Este archivo especifica que el modelo base es `llama3.2:3b`, y configura el sistema para que el modelo responda como si fuera Mario, el personaje de Super Mario Bros. También ajusta la "temperatura" del modelo, lo que afecta la creatividad en las respuestas generadas.

Para guardar el archivo y crear un nuevo modelo con esta configuración, usa el siguiente comando:

`ollama create mario -f Modelfile` 

### 4. Ejecutar el Modelo Personalizado

Una vez que hayas creado el nuevo modelo, puedes probarlo ejecutando el siguiente comando:

`ollama run mario` 

Este comando iniciará el modelo configurado, permitiendo que Mario sea quien te responda a tus preguntas con su estilo propio.

## Recursos Adicionales

Si quisieras tener una interfaz web para interactuar con ollama una muy buena opción seria usar utilizar [open-webui](https://github.com/open-webui/open-webui) en tu servidor.
