# 🎓 Campus Connect

**AI-powered chatbot that answers student questions using your college's policy handbook.**

Built with React, FastAPI, LangChain, and ChromaDB — deployable with a single Docker command.

---

## ✨ Features

- 🤖 **RAG-Powered Answers** — Retrieves relevant policy sections and generates accurate, conversational responses
- 📄 **Automatic PDF Ingestion** — Drop your policy PDF in `backend/data/` and the system handles the rest
- 💬 **Modern Chat UI** — Clean, responsive interface with typing indicators, markdown rendering, and suggestion cards
- 🐳 **One-Command Deploy** — `docker-compose up --build` spins up everything
- 💾 **Persistent Vector Store** — ChromaDB data persists across restarts via Docker volumes
- 🔑 **Flexible LLM Support** — Works with Groq (Llama 3) or Google Gemini

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React (Vite) · Tailwind CSS · Lucide Icons |
| **Backend** | FastAPI · Python 3.10 |
| **LLM** | LangChain + ChatGroq (Llama 3.1 8B) |
| **Embeddings** | HuggingFace `all-MiniLM-L6-v2` (runs locally) |
| **Vector DB** | ChromaDB (persistent, local) |
| **Infra** | Docker · Docker Compose · Nginx |

---

## 📁 Project Structure

```
campus-connect/
├── docker-compose.yml
├── .env.example
├── backend/
│   ├── Dockerfile
│   ├── requirements.txt
│   ├── main.py              # FastAPI server + /api/chat endpoint
│   ├── rag_pipeline.py      # PDF ingestion, embeddings, retrieval chain
│   └── data/
│       └── PolicyBook.pdf   # Your college policy handbook
└── frontend/
    ├── Dockerfile            # Multi-stage: Node build → Nginx
    ├── nginx.conf            # SPA routing + API proxy
    ├── package.json
    ├── vite.config.js
    ├── tailwind.config.js
    └── src/
        ├── App.jsx           # Chat UI component
        ├── main.jsx
        └── index.css
```

---

## 🚀 Quick Start

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) & Docker Compose
- A free API key from [Groq](https://console.groq.com/keys) (or [Google AI Studio](https://aistudio.google.com/))

### Setup

```bash
# 1. Clone the repository
git clone https://github.com/your-username/campus-connect.git
cd campus-connect

# 2. Create your environment file
cp .env.example .env
# Open .env and paste your GROQ_API_KEY

# 3. Place your PDF (if not already present)
# Put your college policy PDF at: backend/data/PolicyBook.pdf

# 4. Build and run
docker-compose up --build
```

### Access

| Service | URL |
|---|---|
| 🌐 Frontend | [http://localhost](http://localhost) |
| ⚙️ Backend API | [http://localhost:8000](http://localhost:8000) |
| 💚 Health Check | [http://localhost:8000/health](http://localhost:8000/health) |

> **Note:** First startup takes a few minutes — the backend downloads the embedding model (~80 MB) and ingests the PDF. Subsequent startups are instant.

---

## ⚙️ Configuration

### Environment Variables

| Variable | Required | Description |
|---|---|---|
| `GROQ_API_KEY` | Yes* | API key for Groq (Llama 3.1) |
| `GOOGLE_API_KEY` | Yes* | API key for Google Gemini (alternative) |

*\*At least one is required. Groq is checked first.*

### Using a Different PDF

Replace `backend/data/PolicyBook.pdf` with your own PDF, then delete the ChromaDB volume to re-ingest:

```bash
docker-compose down -v
docker-compose up --build
```

---

## 📡 API Reference

### `POST /api/chat`

**Request:**
```json
{
  "query": "What is the attendance policy?"
}
```

**Response:**
```json
{
  "answer": "We require a minimum of 75% attendance..."
}
```

### `GET /health`

Returns `{"status": "healthy", "service": "campus-connect-api"}`

---

## 🙏 Acknowledgements

- [LangChain](https://python.langchain.com/) — LLM orchestration
- [ChromaDB](https://www.trychroma.com/) — Vector database
- [Groq](https://groq.com/) — Ultra-fast LLM inference
- [HuggingFace](https://huggingface.co/) — Sentence embeddings
