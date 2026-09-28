📚 AI Study AssistantA private, containerized, full-stack RAG web application for document analysis, intelligent study summaries, automated quiz generation, and interactive real-time Q&A powered by local LLMs.🚀 OverviewAI Study Assistant is a privacy-first web application designed for students, researchers, and developers. It allows users to upload study materials (PDFs, Markdown, Text) and interact with them using an on-device Retrieval-Augmented Generation (RAG) architecture.By leveraging Ollama (Llama 3) and Nomic Embed Text inside a ChromaDB vector database, all document parsing, embeddings, and context-aware responses are handled locally—ensuring zero data leakage, zero API subscription costs, and no rate limits.✨ Key Features📄 Document Ingestion & Semantic Chunking: Upload documents and automatically split them into semantically meaningful chunks with source citation tracking.💬 Interactive Local RAG Chat: Ask questions against your specific study materials. Context is dynamically retrieved from ChromaDB and fed into Llama 3.⚡ Streaming Token Responses: Real-time token streaming using Server-Sent Events (SSE) for zero-latency UI interactions.📝 Automated Study Tools: Generate structured multiple-choice quizzes, flashcards, key concept summaries, and practice questions on command.🔒 100% On-Device Privacy: No external cloud LLM APIs (OpenAI, Anthropic, etc.) required. All data remains strictly on your hardware.🔐 Multi-Method Authentication: Secure backend authentication powered by Django REST Framework (DRF), JWT (JSON Web Tokens) with refresh token rotation, and Google OAuth 2.0.🐳 Containerized Deployment: Pre-configured Docker Compose environment for seamless production and development setups.🛠️ Tech StackDomainTechnologiesBackend FrameworkPython 3.11, Django 5.x, Django REST Framework (DRF)AuthenticationSimpleJWT, Django Auth, Google OAuth 2.0AI Runtime & LLMOllama, Llama 3 / Llama 3.2Embeddings & Vector DatabaseNomic Embed Text, ChromaDBRelational DatabaseSQLite (Development) / PostgreSQL (Production)Frontend UIModern HTML5, Custom CSS3, Vanilla JavaScript (Fetch API / EventSource)DevOps & InfrastructureDocker, Docker Compose, Git🧠 Architecture & RAG PipelinePlaintext ┌──────────────────────────┐
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
⚙️ Getting StartedPrerequisitesEnsure you have the following installed on your system:Docker & Docker Compose (Recommended)Python 3.11+ (If running natively)Ollama installed on your host machine or run via Docker.1. Model Setup via OllamaBefore launching the app, pull the required LLM and Embedding models using Ollama:Bash# Pull Llama 3 model
ollama pull llama3

# Pull Nomic Embed Text model for vector embeddings
ollama pull nomic-embed-text
Verify installed models:Bashollama list
2. Running with Docker Compose (Quickest)Clone the repository:Bashgit clone https://github.com/IRFAN-DEVo/Ai-Study-Assistant.git
cd Ai-Study-Assistant/src/ai_assistant
Configure Environment Variables:Copy .env.example to .env:Bashcp .env.example .env
Build and Run:Bashdocker compose up --build
Open your browser and navigate to http://localhost:8000.3. Local Native Setup (Without Docker)Create and Activate a Virtual Environment:Bash# Windows
python -m venv venv
venv\Scripts\activate

# Linux/macOS
python3 -m venv venv
source venv/bin/activate
Install Dependencies:Bashpip install -r requirements.txt
Apply Database Migrations:Bashpython manage.py migrate
Start the Development Server:Bashpython manage.py runserver
🔧 Environment Configuration (.env)Create a .env file in your root backend folder with the following variables:Code snippet# Django Settings
DEBUG=True
SECRET_KEY=your-django-super-secret-key-change-in-production
ALLOWED_HOSTS=127.0.0.1,localhost

# Ollama Local Configuration
OLLAMA_HOST=http://host.docker.internal:11434
OLLAMA_MODEL=llama3
OLLAMA_EMBEDDING_MODEL=nomic-embed-text

# JWT Settings
ACCESS_TOKEN_LIFETIME_MINUTES=60
REFRESH_TOKEN_LIFETIME_DAYS=7

# Vector Database
CHROMADB_DIR=./chroma_db
🔌 API Endpoints ReferenceAuthenticationMethodEndpointDescriptionPOST/api/accounts/register/Register a new user profilePOST/api/accounts/token/Obtain JWT pair (Access & Refresh)POST/api/accounts/token/refresh/Refresh expired JWT access tokenPOST/api/accounts/google/Authenticate using Google OAuth 2.0Document Management & RAGMethodEndpointDescriptionGET/api/documents/List all user-uploaded study documentsPOST/api/documents/upload/Upload document (PDF/TXT/MD) for parsingDELETE/api/documents/<id>/Delete document and clear ChromaDB vectorsPOST/api/chat/stream/Stream AI response tokens for a prompt (RAG)POST/api/documents/<id>/quiz/Generate dynamic flashcards/quizzes🔒 Security Best PracticesNo Secret Leaks: Always keep .env inside .gitignore.CORS & Throttling: Configured rate-limiting in Django REST Framework to prevent GPU/CPU exhaustion during LLM inference.Token Invalidation: JWT refresh tokens are rotated and blacklisted upon user logout.👨‍💻 AuthorMohammed Irfan K (IRFAN-DEVo)Python Django & Full Stack DeveloperGitHub: @IRFAN-DEVo
