# aichat
# 🧠 AI Knowledge Assistant

A local, open-source RAG-powered internal knowledge assistant.
Ask questions about your company documents and get grounded answers with citations.

---

## 📌 What It Does

- Reads internal documents (Markdown, PDF, DOCX, CSV)
- Chunks and embeds them into a local vector database (ChromaDB)
- Answers natural language questions using a local LLM (Ollama)
- Returns answers with source citations
- Exports answers to Word or Excel
- Tracks usage metrics and latency

---

## 🏗️ Architecture
User Question
↓
Next.js Chat UI (port 3000)
↓
FastAPI Backend (port 8000)
↓
Embed Question → BGE-small-en (Sentence Transformers)
↓
Search ChromaDB → Top-K relevant chunks
↓
Build Prompt → System + Context + Question
↓
Ollama LLM (llama3.2:1b) → Generate Answer
↓
Return Answer + Citations to UI

---

## 🗂️ Project Structure
AICHAT/
├── api/
│   ├── main.py          # FastAPI endpoints (/ask, /reindex, /health)
│   └── export.py        # Word and Excel export endpoints
├── core/
│   ├── parser.py        # Document loader (MD, PDF, DOCX, CSV)
│   ├── chunker.py       # Text chunking with overlap
│   ├── embeddings.py    # Sentence-transformer embeddings
│   ├── vectorstore.py   # ChromaDB vector store
│   ├── llm.py           # Ollama LLM client
│   └── pipeline.py      # Full RAG pipeline
├── data/
│   ├── docs/            # Source documents
│   └── chroma_db/       # Vector index (auto-generated)
├── logs/
│   └── query_log.jsonl  # Query logs with latency
├── tools/
│   ├── synth_data.py    # Synthetic document generator
│   └── report.py        # Metrics visualizer
├── ui/
│   └── app/
│       └── page.tsx     # Next.js chat interface
├── .env                 # Configuration
└── requirements.txt     # Python dependencies

---

## ⚙️ Tech Stack

| Layer | Tool | Cloud Equivalent |
|---|---|---|
| LLM | Ollama (llama3.2:1b) | Azure OpenAI / AWS Bedrock |
| Embeddings | BGE-small-en-v1.5 | Azure Embeddings |
| Vector DB | ChromaDB | Azure Cognitive Search |
| API | FastAPI | Azure Functions |
| UI | Next.js | Azure Web App |
| Monitoring | JSON logs + Matplotlib | App Insights |

---

## 🚀 Quick Start

### Prerequisites
- Python 3.11+
- Node.js 18+
- Ollama installed → https://ollama.com

### 1. Clone and setup
```bash
git clone <your-repo-url>
cd aichat
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Pull the LLM model
```bash
ollama pull llama3.2:1b
```

### 3. Configure environment
```bash
OLLAMA_MODEL=llama3.2:1b
EMBEDDING_MODEL=BAAI/bge-small-en-v1.5
CHROMA_PATH=./data/chroma_db
CHUNK_SIZE=100
CHUNK_OVERLAP=20
API_KEY=changeme123
```

### 4. Generate synthetic data
```bash
python -m tools.synth_data
```

### 5. Build the index
```bash
python -m core.pipeline index
```

### 6. Start the backend
```bash
uvicorn api.main:app --reload --port 8000
```

### 7. Start the frontend
```bash
cd ui
npm install
npm run dev
```

### 8. Open the chat
http://localhost:3000

---

## 📡 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/ask` | Ask a question, get answer + citations |
| POST | `/ask/stream` | Streaming version of /ask |
| POST | `/reindex` | Rebuild vector index from docs |
| POST | `/export/docx` | Export answer as Word file |
| POST | `/export/xlsx` | Export answer as Excel file |
| GET | `/health` | Health check |

All endpoints require `X-API-Key` header.

---

## 📊 Evaluation

Run the metrics report:
```bash
python -m tools.report
```

Output:
- Total queries, avg latency, min/max latency
- Recall@K evaluation against known questions
- Charts saved to `logs/metrics_report.png`

---

## 🔒 Security

- All API endpoints protected with API key
- Email and phone numbers redacted from responses
- Sensitive credentials stored in `.env` (never committed)
- `.env` is listed in `.gitignore`

---

## 📈 Sample Questions
What is the engineer onboarding process?
What are the Python best practices?
What are the password requirements?
What is the data security policy?
How do I set up my dev environment?
What is the company overview?

---

## 🔮 Future Improvements

- Hybrid search (keyword + vector)
- Re-ranking model for better accuracy
- Usage dashboard with query trends
- Docker containerization
- Deploy to Azure/AWS free tier

---

## 👤 Author

Built as part of the AI Engineering onboarding project.
Demonstrates end-to-end RAG pipeline from data ingestion to LLM-powered chat.