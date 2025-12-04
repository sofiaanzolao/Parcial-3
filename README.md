# Parcial-3
# 🌱 RAG – Asesor de Crédito Agropecuario para Caficultores en Colombia

Este repositorio contiene el proyecto completo del sistema **RAG (Retrieval-Augmented Generation)** desarrollado para ofrecer **asesoría crediticia personalizada** a pequeños y medianos **caficultores colombianos**. El sistema combina documentos reales del sector agropecuario con modelos de lenguaje para responder preguntas sobre líneas de crédito, requisitos, plazos, tasas, garantías y adopción de tecnologías agrícolas como drones de aspersión.

---

## 📁 Estructura del repositorio
📦 RAG-Credito-Caficultores
1. 📜 codigo_rag.ipynb # Notebook completo con todo el pipeline RAG
2. 📄 Documento_proyecto.pdf # Informe académico (máx. 3 páginas)
3. 📂 Documentos PDF utilizados
 - 1.pdf
 - 2.pdf
 - 3.pdf
 - ...
 - 14.pdf
4. 📘 README.md # Este archivo

La carpeta BASES_DE_DATOS contiene todos los documentos utilizados por el sistema, incluyendo:

- Formularios del Banco Agrario

- Documentos sobre el Fondo Agropecuario de Garantías (FAG)

- Estudios de costos de producción de café

- Documentos técnicos sobre drones de aspersión

- Normativas aeronáuticas (RAC 100)

- Material técnico de Cenicafé

Estos archivos se cargan manualmente cuando se ejecuta el notebook, tal como lo requiere Google Colab.

---

## 🚀 ¿Qué hace este RAG?
💬 Permite que un caficultor pregunte en lenguaje natural, por ejemplo:
“Quiero comprar un dron de aspersión. ¿Qué crédito de inversión agropecuaria me sirve y qué tasa podría tener?”

El sistema:

🔍 Busca en los documentos relevantes usando embeddings.

📚 Selecciona los fragmentos más importantes con FAISS + reranker.

✍️ Genera una respuesta clara, en español colombiano y basada solo en el contexto.

📌 Muestra las fuentes exactas de donde provino la información.

---

## 🧠 Modelos utilizados
🔹 Encoder (para embeddings)
sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2
Elegido por ser rápido, ligero y muy efectivo con textos en español. Convierte los fragmentos de los PDFs en vectores que luego se almacenan en FAISS.

🔹 Reranker
mixedbread-ai/mxbai-rerank-base-v1
Ordena los fragmentos recuperados para seleccionar solo los más relevantes antes de la generación.

🔹 Decoder / Modelo generativo
mistralai/Mistral-7B-Instruct-v0.3 (4-bit)
Seleccionado por su buen rendimiento en español, su capacidad de seguir instrucciones y su compatibilidad con Google Colab en versión cuantizada.

---

## 🏗️ Arquitectura del pipeline
El sistema sigue un flujo RAG clásico:

1. Carga manual de PDFs
Los documentos se suben mediante el botón "Choose Files" que aparece cuando se corre el código.

2. Procesamiento y chunking
Los PDFs se dividen en fragmentos de ~500 caracteres con traslape de 100.

3. Vectorización con MiniLM
Cada fragmento se convierte en un embedding.

4. Indexación con FAISS
Se almacenan los vectores para búsquedas rápidas por similitud.

5. Re-ranking de resultados
Se seleccionan los mejores fragmentos según la pregunta del usuario.

6. Construcción del prompt
Se combinan consulta + contexto + instrucciones de seguridad.

7. Generación con Mistral 7B
El modelo produce una respuesta clara, precisa y citada.

---

## 🧪 Ejemplo de uso
query = (
    "Soy pequeño caficultor en Pitalito y quiero comprar un dron de aspersión. "
    "¿Qué líneas de crédito de inversión agropecuaria existen y qué plazos y tasas manejan?"
)

print(ask(query, k=10, rerank_top=5))

---

## 📌 Requisitos
Este proyecto está diseñado para ejecutarse en Google Colab, por lo que no requiere instalación local.
El único paso manual es subir los PDFs ubicados en la carpeta BASES_DE_DATOS.

---

## 🤝 Autores
Proyecto desarrollado para el curso de Inteligencia Artificial aplicada, con énfasis en sistemas RAG y aplicaciones en el sector agropecuario colombiano.
