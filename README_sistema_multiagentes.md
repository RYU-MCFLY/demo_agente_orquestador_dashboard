# Sistema Multi-Agente para Portfolio Personal — Demo

Demostración de un sistema de **agentes y subagentes** pensado para embeberse en
una web de portfolio personal, mostrando de forma práctica: arquitectura de
orquestación multi-agente, RAG, memoria conversacional, *tool calling* y
*guardrails*.

## Contenido

- **`sistema_multiagente_portfolio.ipynb`** — Notebook técnico y ejecutable
  (probado de extremo a extremo, sin necesidad de ninguna API key) que implementa:
  - Un **agente orquestador** que clasifica la intención del visitante y enruta
    la conversación.
  - Un **subagente RAG** ("Experto en Trayectoria") con *chunking*, *embeddings*
    (implementados en Python puro para que el mecanismo sea transparente) y
    búsqueda por similitud coseno sobre una base de conocimiento de CV/proyectos.
  - Un **subagente de Contacto** que extrae datos (email, nombre, presupuesto) y
    los envía a Airtable/Google Sheets (simulado en la demo, con el código real
    comentado).
  - Un **subagente Analista de Datos** que procesa un CSV/Excel y genera un
    informe automático.
  - Memoria de ventana, *guardrails* (el agente admite cuando no sabe algo en
    vez de alucinar) y notas de cumplimiento RGPD.
  - Instrucciones de despliegue gratuito (Hugging Face Spaces + Docker,
    Streamlit Community Cloud) e integración embebida en una web.

- **`demo.html`** — Demostración interactiva y autocontenida (HTML + CSS + JS,
  sin backend ni dependencias externas) del mismo patrón de interacción:
  una burbuja de chat flotante, con los tres subagentes funcionando en el
  navegador, indicador de "escribiendo…", una etiqueta que muestra qué
  subagente se ha invocado en cada turno (transparencia de proceso), un mini
  gráfico de barras para el informe del Analista de Datos, y el aviso de IA/RGPD
  antes de capturar datos de contacto. Ábrelo directamente en el navegador.

## Cómo probarlo

```bash
# Notebook
jupyter notebook sistema_multiagente_portfolio.ipynb

# Demo HTML (sin servidor necesario)
open demo.html   # o simplemente arrástralo a una pestaña del navegador
```

## De la demo a producción

| Pieza de la demo | Sustituir en producción por |
|---|---|
| Vectorizador *bag-of-words* en Python puro | `text-embedding-3-small` (OpenAI) o modelo de Groq |
| Vector store en memoria | **Supabase** (pgvector) — tier gratuito |
| `call_llm` en modo offline | GPT-4o-mini o Llama 3 (Groq) vía API real |
| `localStorage` / `.jsonl` para contactos | Webhook a Airtable, Google Sheets o n8n |
| Clasificación de intención por palabras clave | *Function calling* / *tool choice* del LLM |
| `demo.html` standalone | Backend desplegado en Hugging Face Spaces (Docker) o Streamlit, consumido vía `fetch()` desde el widget embebido |
