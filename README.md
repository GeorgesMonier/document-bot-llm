Document Bot con Apache Spark

Una herramienta de Python que extrae texto de documentos (PDF, Word, TXT), genera resúmenes y responde preguntas sobre su contenido, utilizando un LLM local (sin necesidad de una API de pago) y Apache Spark para el procesamiento por lotes en paralelo.

Funcionalidades

Extracción en múltiples formatos: PDF, DOCX, TXT

Resumen y preguntas y respuestas mediante un LLM local a través de Ollama
 (llama3.2)

Procesamiento paralelo de documentos con PySpark

División automática en fragmentos (map-reduce) para documentos largos que superan el contexto del modelo

Gestión robusta de errores: un archivo corrupto no hará que falle todo el lote

Pruebas unitarias con pytest

Cómo funciona

Spark lee todos los documentos de data/ y los distribuye entre los workers.

Cada worker extrae el texto y genera un resumen (dividiéndolo automáticamente en fragmentos si el documento es largo).

Los resultados se recopilan y se muestran al usuario.

El usuario selecciona un documento y puede hacer preguntas de forma libre sobre él mediante un bucle interactivo.

Tecnologías utilizadas

Python · Apache Spark (PySpark) · Ollama (LLM local) · PyMuPDF · python-docx · pytest

Instalación

Requisitos: Python 3.10+, Java (JDK 17), Ollama

# Clonar el repositorio
git clone https://github.com//document-bot-spark.git
cd document-bot-spark

# Crear y activar un entorno virtual
python -m venv venv
venv\Scripts\Activate.ps1   # Windows
# source venv/bin/activate  # Mac/Linux

# Instalar las dependencias
pip install -r requirements.txt

# Descargar el modelo local
ollama pull llama3.2

Uso

Coloca tus documentos (PDF, DOCX o TXT) en la carpeta data/ y después ejecuta:

python main.py


El bot resumirá todos los documentos en paralelo, te permitirá seleccionar uno y abrirá una sesión interactiva de preguntas y respuestas.

Ejecutar las pruebas
pytest tests/ -v

Limitaciones conocidas

La recuperación de información para preguntas y respuestas en documentos largos utiliza una búsqueda sencilla basada en palabras clave en lugar de una búsqueda semántica. Un siguiente paso natural sería utilizar embeddings junto con un almacén vectorial (por ejemplo, ChromaDB) para una verdadera recuperación basada en RAG.

Los modelos locales pequeños (como llama3.2) pueden añadir ocasionalmente contenido adicional más allá de lo solicitado. Esto se intenta evitar mediante instrucciones (prompts) estrictas, pero no se puede garantizar al 100 %.