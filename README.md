# PDF RAG Chat Application - Build for exploration (completely api based)
A Flask-based PDF Retrieval-Augmented Generation (RAG) chat application that allows users to upload PDFs and ask questions about their content. Built to showcase modern RAG architecture.

## 🚀 Features

- **PDF Upload & Processing**: Automatic chunking and embedding generation
- **Intelligent Retrieval**: Multi-query expansion with semantic search
- **Advanced Reranking**: Jina AI reranker for improved relevance
- **Grounded Answers**: LLM responses with inline citations and source snippets
- **Session Isolation**: Per-session namespaces prevent data contamination
- **Multiple LLM Support**: OpenRouter, OpenAI, or Google Gemini

## 🏗️ Architecture

### Vector Database Pipeline
- **Pinecone**: Cloud-hosted vector database
- **Index**: `apirag-1024` (1024-dimensional embeddings)
- **Embeddings**: Jina AI Embeddings v3 via API
- **Chunking**: 1000 tokens with 150 token overlap
- **Isolation**: Session-scoped namespaces (`pdf_{session_id}`)

### RAG Processing Flow
1. **Document Processing**: PDF → PyPDFLoader → RecursiveCharacterTextSplitter → Jina Embeddings → Pinecone
2. **Query Processing**: MultiQueryRetriever → Pinecone similarity search → Jina reranking
3. **Answer Generation**: Retrieved context → LLM → Grounded response with citations

### Key Components
- **Embeddings**: Jina AI v3 (API-based, 1024-dimensional)
- **Reranker**: Jina AI reranker for relevance optimization
- **LLM**: OpenRouter/OpenAI/Gemini support
- **Frontend**: Simple upload interface with real-time chat

## 🛠️ Setup

### 1. Environment Setup
```bash
# Create virtual environment (Windows)
py -m venv .venv

# Activate virtual environment (PowerShell)
. .venv\Scripts\Activate.ps1

# Install dependencies
pip install -r requirements.txt
```

### 2. Environment Configuration
Create `.env` file from template:
```bash
cp env.example .env
```

Required environment variables:
```bash
# LLM Configuration
OPENAI_API_KEY
OPENAI_API_BASE
OPENAI_MODEL 

# Vector Database
PINECONE_API_KEY
PINECONE_INDEX_NAME

# Jina AI (Embeddings + Reranking)
JINA_API_KEY
EMBEDDING_MODEL
JINA_EMBEDDING_DIMENSIONS

# Flask
FLASK_SECRET_KEY
```

### 3. Run Application
```bash
python app.py
```


## 📝 Usage

1. **Upload PDF**: Click "Upload PDF" and select your document
2. **Wait for Processing**: The app will chunk and embed your PDF (may take 1-2 minutes)
3. **Ask Questions**: Chat with your PDF using natural language
4. **Get Answers**: Receive grounded responses with inline citations and source snippets

## 🚀 Deployment

### Render.co
The app is optimized for Render deployment:
- Uses Gunicorn with memory-optimized settings
- API-based embeddings (no local model loading)
- Configured via `render.yaml`

```

## 👁️ Key Features

- **Memory Efficient**: API-based embeddings eliminate local model overhead
- **Session Isolated**: Each upload creates a separate namespace
- **Citation Tracking**: Answers include source references and snippets
- **Multi-LLM Support**: Works with OpenRouter, OpenAI, or Gemini
- **Batch Processing**: Efficient embedding generation for large documents

## 🛠️ Technical Notes

- **Embedding Dimension**: 1024 (Jina v3)
- **Chunk Size**: 1000 tokens with 150 token overlap
- **Retrieval**: Top-5 similarity search + Jina reranking (top-3)
- **Session Management**: Automatic cleanup on page refresh
- **API Rate Limits**: Batched requests (50 texts per API call)
