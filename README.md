📚 AI Study Assistant

A private, containerized, full-stack RAG web application for document analysis, intelligent study summaries, automated quiz generation, and interactive real-time Q&A powered by local LLMs

.🚀 OverviewAI Study Assistant is a privacy-first web application designed for students, researchers, and developers. It allows users to upload study materials (PDFs, Markdown, Text) and interact with them using an on-device Retrieval-Augmented Generation (RAG) architecture.By leveraging Ollama (Llama 3) and Nomic Embed Text inside a ChromaDB vector database, all document parsing, embeddings, and context-aware responses are handled locally—ensuring zero data leakage, zero API subscription costs, and no rate limits.

✨ Key Features📄 Document Ingestion & Semantic Chunking: Upload documents and automatically split them into semantically meaningful chunks with source citation tracking.💬 Interactive Local RAG Chat: Ask questions against your specific study materials. Context is dynamically retrieved from ChromaDB and fed into Llama 3.
⚡ Streaming Token Responses: Real-time token streaming using Server-Sent Events (SSE) for zero-latency UI interactions.
📝 Automated Study Tools: Generate structured multiple-choice quizzes, flashcards, key concept summaries, and practice questions on command.
🔒 100% On-Device Privacy: No external cloud LLM APIs (OpenAI, Anthropic, etc.) required. All data remains strictly on your hardware.
🔐 Multi-Method Authentication: Secure backend authentication powered by Django REST Framework (DRF), JWT (JSON Web Tokens) with refresh token rotation, and Google OAuth 2.0.
🐳 Containerized Deployment: Pre-configured Docker Compose environment for seamless production and development setups.
🛠️ Tech StackDomainTechnologiesBackend FrameworkPython 3.11, Django 5.x, Django REST Framework (DRF)AuthenticationSimpleJWT, Django Auth, Google OAuth 2.0AI Runtime & LLMOllama, Llama 3 / Llama 3.2Embeddings & Vector DatabaseNomic Embed Text, ChromaDBRelational DatabaseSQLite (Development) / PostgreSQL (Production)Frontend UIModern HTML5, Custom CSS3, Vanilla JavaScript (Fetch API / EventSource)DevOps & InfrastructureDocker, Docker Compose, Git🧠
Architecture & RAG PipelinePlaintext 
 ┌──────────────────────────┐
 │  User Document Upload    │
 │  (PDF / TXT / Markdown)  │
 └─────────────┬────────────┘
               │
               ▼
 ┌──────────────────────────┐
 │  Text Extraction         │
 │  & Semantic Chunking     │
 └─────────────┬────────────┘
               │
               ▼
 ┌──────────────────────────┐
 │  Nomic Embed Text        │ ◄─── (Ollama Local Embedding Engine)
 └─────────────┬────────────┘
               │
               ▼
 ┌──────────────────────────┐
 │  ChromaDB Storage        │ ◄─── (Vector Store + Metadata Index)
 └─────────────┬────────────┘
               │
 user Query    │
               ▼
 ┌──────────────────────────┐
 │  Vector Similarity Search│ ───► Retrieves Top-K Relevant Passages
 └─────────────┬────────────┘
               │
 Context+Prompt│
               ▼
 ┌──────────────────────────┐
 │  Llama 3 (Local LLM)     │ ◄─── (Ollama Inference Runtime)
 └─────────────┬────────────┘
               │
               ▼
 ┌──────────────────────────┐
 │ SSE Token Stream Output  │ ───► Interactive Frontend UI
 └──────────────────────────┘
📁 Project StructurePlaintextAi-Study-Assistant/
├── src/
│   └── ai_assistant/
│       ├── manage.py
│       ├── Dockerfile
│       ├── docker-compose.yml
│       ├── requirements.txt
│       ├── .env.example
│       ├── ai_assistant/          # Core Django project configuration
│       │   ├── settings.py
│       │   ├── urls.py
│       │   └── wsgi.py
│       ├── accounts/              # Authentication & User Profiles
│       │   ├── models.py
│       │   ├── serializers.py
│       │   └── views.py
│       ├── documents/             # Ingestion, Chunking, ChromaDB integration
│       │   ├── models.py
│       │   ├── services.py
│       │   └── views.py
│       ├── chat/                  # RAG Query Pipeline & SSE Streaming
│       │   ├── rag_engine.py
│       │   └── views.py
│       ├── static/                # CSS, Frontend JavaScript
│       └── templates/             # HTML Templates
├── .gitignore
└── README.md
