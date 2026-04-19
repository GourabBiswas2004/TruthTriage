🛡️ TruthTriage
AI-Powered Pharmaceutical Safety & Medical Verification System

TruthTriage is a Retrieval-Augmented Generation (RAG) based medical AI assistant that delivers accurate, evidence-backed pharmaceutical insights using only trusted and verified sources such as WHO, CDSCO, ICMR, and MoHFW.

⚠️ Unlike generic AI systems, TruthTriage never guesses — it responds only when reliable sources are available.

🚀 Overview

TruthTriage is designed to provide safe, explainable, and verifiable medical responses. It ensures transparency by attaching source references, similarity scores, and structured outputs to every response.

✅ What makes it different?
❌ No hallucinations — refuses to answer without verified sources
📄 Source-backed answers with document references and page numbers
💊 Extracts medicines from official databases
🗺️ Helps locate nearby doctors based on symptoms
📊 Provides confidence scores using cosine similarity
✨ Key Features
🔍 RAG-Based Medical Q&A
Structured responses with:
Risk Level (Low / Moderate / High)
Condition Analysis
Recommended Specialist
Medicine Suggestions
Precautions & Warnings
🗺️ Doctor Finder
Maps symptoms → specialist types (50+ mappings)
Finds nearby doctors using OpenStreetMap
Interactive map with directions
Supports hospitals, clinics, and individual practitioners
💊 Smart Medicine Extraction
Extracts medicine names from AI responses
Uses fallback extraction from verified documents
Filters irrelevant terms (e.g., dosage forms)
Displays source for each medicine
📊 Confidence Scoring
Uses all-MiniLM-L6-v2 embeddings
Computes cosine similarity between query & sources
Visual indicators:
🟢 High (>70%)
🟡 Medium (>40%)
🔴 Low (<40%)
🏗️ System Architecture
Frontend (React + Tailwind)
        │
        ▼
FastAPI Backend (REST API)
        │
        ▼
RAG Pipeline:
- PDF Loader (WHO, CDSCO, MoHFW)
- FAISS Vector DB
- Groq LLM (LLaMA 3)
        │
        ├── Doctor Finder (Overpass API)
        └── Medicine Extractor
📁 Project Structure
TruthTriage/
├── backend/
│   ├── main.py
│   ├── rag_service.py
│   ├── models.py
│   └── .env
│
├── data/
│   ├── WHO.pdf
│   ├── cdsco_drug_list.pdf
│   └── mohfw_nlem.pdf
│
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   ├── components/
│   │   └── services/
│   └── tailwind.config.js
│
└── README.md
🛠️ Tech Stack
Layer	Technology
LLM	Groq (LLaMA 3.3 70B)
Embeddings	HuggingFace (MiniLM)
Vector DB	FAISS
Backend	FastAPI + LangChain
Frontend	React + Vite + Tailwind
Maps	Leaflet + OpenStreetMap
Data	WHO, CDSCO, MoHFW
⚡ Quick Start
🔧 Prerequisites
Python 3.12+
Node.js 18+
Groq API Key
▶️ Backend Setup
cd backend

python -m venv venv
source venv/bin/activate

pip install fastapi uvicorn langchain langchain-community \
langchain-groq langchain-text-splitters faiss-cpu \
sentence-transformers pypdf python-dotenv numpy

echo "GROQ_API_KEY=your_key_here" > .env

uvicorn main:app --reload --port 8001
💻 Frontend Setup
cd frontend

npm install
npm run dev
