# Asistente Turístico de Tenerife · RAG + Diálogo + `get_weather`

Prototipo reproducible que responde preguntas sobre una guía turística de **Tenerife** usando **búsqueda semántica (RAG)**, mantiene **memoria de conversación** y ejecuta una **function call** simulada `get_weather(fecha)` con validación y logging. Proyecto del módulo LLM (Máster en IA, Cloud & DevOps).

<div align="left">

![Python](https://img.shields.io/badge/Python-3.11+-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Lab-orange)
![LangChain](https://img.shields.io/badge/LangChain-0.2+-informational)
![FAISS](https://img.shields.io/badge/Vector%20DB-FAISS-green)
![Model](https://img.shields.io/badge/LLM-Gemini%201.5%20Flash-6aa84f)

</div>

---

## 1) Características

- **RAG**: extracción del PDF, *chunking* (500/100), **embeddings** `text-embedding-004`, **FAISS** como vector store.  
- **Diálogo multiturturno**: mantenimiento de historial con `ConversationalRetrievalChain` y recuperación `k=3`.  
- **Function calling**: `get_weather(fecha)` (simulada) con **Pydantic**, manejo de errores y **log** en `logs.txt`.  
- **Persistencia** del índice (`vs_tenerife/`) para evitar recomputar embeddings.  
- **Notebook único** con celdas ordenadas y comentarios breves.

---

## 2) Requisitos

- **Python 3.11+**
- **API key de Gemini** (free tier) en un archivo `.env`:
  ```
  GOOGLE_API_KEY=TU_CLAVE_AQUI
  ```
  > El `.env` está ignorado por Git.

---

## 3) Instalación rápida

```bash
git clone git@github.com:bittordani/llm-tourist-assistant.git
cd llm-tourist-assistant
pip install -r requirements.txt
```

Coloca el PDF en la ruta:
```
data/TENERIFE.pdf
```

Inicia Jupyter:
```bash
jupyter lab
```
Abre `notebook.ipynb` y ejecuta las celdas en orden.

---

## 4) Estructura del repositorio

```
.
├─ data/
│  └─ TENERIFE.pdf         # guía turística (entrada)
├─ notebook.ipynb          # flujo principal (RAG + diálogo + weather)
├─ requirements.txt
├─ README.md
└─ .gitignore              # incluye .env y vs_tenerife/
```

---

## 5) Cómo funciona (visión técnica)

- **Extracción**: `pypdf` obtiene el texto del PDF.  
- **Chunking**: `chunk_size=500`, `overlap=100`.  
- **Embeddings**: `models/text-embedding-004` (dim=768).  
- **Vector store**: FAISS; persistencia en `vs_tenerife/`.  
- **Diálogo**: `ConversationalRetrievalChain` (Gemini 1.5 Flash) con recuperación `k=3`.  
- **Weather**: `get_weather(fecha)` valida `AAAA-MM-DD` con **Pydantic**, genera un pronóstico determinista por fecha y registra cada intento en `logs.txt`.

---

## 6) Uso recomendado en el notebook

1. **Verificación** de API y entorno.  
2. **Lectura del PDF** → variable `texto`.  
3. **Chunking** → lista `chunks`.  
4. **Embeddings + FAISS** → índice `vs` (guardar y recargar).  
5. **Búsqueda de prueba** (3 fragmentos).  
6. **Chat conversacional** con fuentes.  
7. **`get_weather(fecha)`**:  
   - 3 llamadas correctas.  
   - 1 llamada con error (formato) → ver en `logs.txt`.

### Ejemplos de prueba

- “Voy 2 días a Tenerife. ¿Qué no me puedo perder?”  
- “Mejores playas cerca de Costa Adeje.”  
- “¿Qué tiempo hará el **2025-12-25** en Tenerife?”  
- “¿Qué tiempo hará el **2025-99-99**?” → debe mostrar error de validación.

---

## 7) Problemas frecuentes

- **`Falta GOOGLE_API_KEY en .env`** → crea `.env` y reinicia el kernel.  
- **PDF sin texto** (escaneado) → usar OCR (no incluido).  

---

## 8) Mejoras futuras

- Conexión a un servicio real de meteorología.

---

## 9) Licencia y créditos

Proyecto académico para fines educativos. La guía original de Tenerife pertenece a sus autores/propietarios.
Trabajo realizado por Víctor Daniel Martínez para el proyecto de LLM dentro del de Pontia en Inteligencia Artificial, Cloud Computing y DevOps.

---

**Notas finales**  
El flujo completo está en `notebook.ipynb`. Ejecuta de arriba a abajo; las celdas incluyen comentarios breves y ejemplos de verificación.

