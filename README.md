# RAG Chatbot Starter (FastAPI + ChromaDB)

A lightweight, production-ready **Retrieval-Augmented Generation (RAG)** chatbot backend. Upload PDFs, ask questions, and receive answers directly from your documents with precise page-level source citations.

Built with **FastAPI** and **ChromaDB**, featuring a flexible LLM layer that supports **OpenAI, Claude, Gemini, and local Ollama**. Works entirely offline in retrieval mode with no API keys required, and launches with a single Docker command.

---

## Features

- **Complete RAG pipeline** — PDF upload → text extraction → intelligent chunking → embeddings → semantic retrieval → answer generation
- **Accurate source citations** — all answers include the source document name, page number, and exact matched text
- **Flexible LLM providers** — seamlessly switch between OpenAI, Claude, Gemini, or local Ollama via environment variables
- **Zero-API-key operation** — runs offline in retrieval mode, returning matched passages without external LLM calls
- **Multilingual support** — embeddings work across English, Thai, and numerous other languages
- **Docker ready** — single `docker compose up` command to get started

---

## Quick start (Docker)

```bash
cp .env.example .env
docker compose up --build
```

- API: http://localhost:8000
- Interactive API docs: http://localhost:8000/docs

By default, the service runs in `retrieval` mode with no API key required. To enable LLM-powered responses, configure `LLM_PROVIDER` and add your API key to `.env`.

## Quick start (local)

```bash
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
uvicorn app.main:app --reload
```

> On first run, the embedding model downloads automatically (a few hundred MB) and is cached for subsequent runs.

---

## API endpoints

| Method   | Path                              | Description                                |
| -------- | --------------------------------- | ------------------------------------------ |
| `POST`   | `/api/v1/documents/upload`        | Upload and index a PDF document            |
| `GET`    | `/api/v1/documents`               | Retrieve list of all indexed documents     |
| `DELETE` | `/api/v1/documents/{document_id}` | Remove a document and its vector embeddings |
| `POST`   | `/api/v1/chat`                    | Query indexed documents with a question    |

### Ask a question

```bash
curl -X POST http://localhost:8000/api/v1/chat \
  -H "Content-Type: application/json" \
  -d '{"question": "What is this document about?", "top_k": 3}'
```

---

## Configuration

All settings are managed via `.env` (copy from `.env.example` to get started). Key configuration options:

| Variable          | Default             | Description                                          |
| ----------------- | ------------------- | ---------------------------------------------------- |
| `LLM_PROVIDER`    | `retrieval`         | LLM provider: `retrieval`, `openai`, `ollama`, `gemini`, or `claude`  |
| `EMBEDDING_MODEL` | multilingual MiniLM | SentenceTransformers model used for document embeddings            |
| `*_API_KEY`       | —                   | API key for your selected LLM provider                         |

Update the model IDs (`OPENAI_MODEL`, `CLAUDE_MODEL`, `GEMINI_MODEL`, `OLLAMA_MODEL`) to match your account's available models.

---
