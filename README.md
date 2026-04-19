# TruthTriage
 What is TruthTriage?
TruthTriage is a Retrieval-Augmented Generation (RAG) medical AI assistant that provides pharmaceutical safety information backed exclusively by trusted, verified sources. Unlike general-purpose chatbots, TruthTriage:

❌ Never hallucinates — refuses to answer if no verified source is found
✅ Cites every claim with source documents, page numbers, and similarity scores
💊 Suggests medicines extracted from WHO Essential Medicines List & CDSCO drug databases
🗺️ Finds specialist doctors near you using OpenStreetMap
📊 Scores confidence using cosine similarity between your query and retrieved documents
✨ Key Features
🔍 RAG-Powered Medical Q&A
Ask any pharmaceutical safety question and get structured, evidence-based responses with:

Risk Level assessment (Low / Moderate / High)
Condition Analysis from verified medical literature
Recommended Specialist type (Cardiologist, Dermatologist, etc.)
Medicine Suggestions with dosage and indication
Precautions and contraindications
🗺️ Specialist Doctor Finder
Find healthcare facilities near any city:

50+ condition-to-specialist mappings (e.g., "chest pain" → Cardiologist)
OpenStreetMap Overpass API integration for real clinic/hospital data
Interactive Leaflet map with markers, popups, and Google Maps directions
Supports doctors, clinics, and hospitals
💊 Smart Medicine Extraction
Parses medicine names from LLM responses using multiple regex patterns
Fallback extraction from source documents using a curated list of 100+ known medicines
Filters out dosage forms (Tablet, Injection) to show only actual drug names
Displays source document for each medicine
📊 Cosine Similarity Scoring
Embeds queries and retrieved document chunks using all-MiniLM-L6-v2
Computes cosine similarity for each source
Color-coded badges: 🟢 >70% | 🟡 >40% | 🔴 <40%
Animated progress bars in the telemetry panel
🏗️ Architecture
┌──────────────────────────────────────────────────────────┐
│                    React Frontend                        │
│  ┌──────────┐  ┌──────────────┐  ┌───────────────────┐  │
│  │LeftPanel │  │  Chat View   │  │   RightPanel      │  │
│  │(History) │  │  (Markdown)  │  │ ┌───────────────┐ │  │
│  │          │  │              │  │ │  Telemetry    │ │  │
│  │          │  │   ReactMD    │  │ │  Doctors Map  │ │  │
│  │          │  │   Rendering  │  │ │  Medicines    │ │  │
│  └──────────┘  └──────┬───────┘  │ └───────────────┘ │  │
│                       │          └───────────────────┘  │
└───────────────────────┼─────────────────────────────────┘
                        │ HTTP/REST
┌───────────────────────┼─────────────────────────────────┐
│                 FastAPI Backend                          │
│  ┌────────────────────┴──────────────────────────────┐  │
│  │              RAG Service                          │  │
│  │  ┌──────────┐  ┌──────────┐  ┌────────────────┐  │  │
│  │  │ PDF Load │  │  FAISS   │  │   Groq LLM     │  │  │
│  │  │ (3 PDFs) │→ │VectorDB  │→ │ (llama-3.3-70b)│  │  │
│  │  └──────────┘  └──────────┘  └────────────────┘  │  │
│  │  ┌──────────────────┐  ┌──────────────────────┐  │  │
│  │  │ Doctor Finder    │  │ Medicine Extractor   │  │  │
│  │  │ (Overpass API)   │  │ (100+ known drugs)   │  │  │
│  │  └──────────────────┘  └──────────────────────┘  │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
📁 Project Structure
TruthTriage/
├── backend/
│   ├── main.py              # FastAPI app with /chat, /doctors, /health endpoints
│   ├── rag_service.py       # Core RAG pipeline, QA chain, doctor finder, medicine extraction
│   ├── models.py            # Pydantic models (ChatResponse, Doctor, MedicineInfo, Source)
│   └── .env                 # GROQ_API_KEY
├── data/
│   ├── WHO.pdf              # WHO Model List of Essential Medicines
│   ├── cdsco_drug_list.pdf  # CDSCO approved drug list
│   └── mohfw_nlem.pdf       # MoHFW National List of Essential Medicines
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   │   ├── HomePage.jsx           # Landing page
│   │   │   ├── ChatPage.jsx           # Chat interface
│   │   │   └── TruthDashboardPage.jsx # Forensic-themed dashboard
│   │   ├── components/
│   │   │   ├── ChatMessage.jsx        # Message with markdown rendering
│   │   │   ├── DoctorMap.jsx          # Leaflet map for doctors
│   │   │   ├── RightPanel.jsx         # 3-tab panel (Telemetry/Doctors/Medicines)
│   │   │   └── ...
│   │   └── services/
│   │       └── api.js                 # Axios API client
│   └── tailwind.config.js             # Custom forensic theme
└── README.md
🛠️ Tech Stack
Layer	Technology
LLM	Groq (llama-3.3-70b-versatile)
Embeddings	HuggingFace all-MiniLM-L6-v2
Vector Store	FAISS
Framework	LangChain + FastAPI
Frontend	React 18 + Vite + Tailwind CSS
Map	Leaflet + React-Leaflet
Markdown	react-markdown + @tailwindcss/typography
Doctor Search	OpenStreetMap Overpass API + Nominatim
Data Sources	WHO, CDSCO, MoHFW PDFs
⚡ Quick Start
Prerequisites
Python 3.12+
Node.js 18+
Groq API Key
Backend Setup
cd backend

# Create virtual environment
python -m venv venv
source venv/bin/activate

# Install dependencies
pip install fastapi uvicorn langchain langchain-community langchain-groq \
    langchain-text-splitters faiss-cpu sentence-transformers pypdf \
    python-dotenv numpy

# Set your API key
echo "GROQ_API_KEY=your_key_here" > .env

# Start the server
uvicorn main:app --reload --port 8001
Frontend Setup
cd frontend

# Install dependencies
npm install

# Start dev server
npm run dev
Open the App
Navigate to http://localhost:5173 → Click Init Verification → Start querying!

📸 Features in Action
💬 Structured Medical Responses
🔍 Risk Level: Moderate

📋 Condition Analysis:
Ibuprofen is an NSAID that may interact with antihypertensive medications,
potentially reducing their effectiveness and increasing blood pressure...

👨‍⚕️ Recommended Specialist: Cardiologist

💊 Suggested Medicines:
- Paracetamol — safer alternative for fever (WHO Essential Medicines List)
- Propranolol — antihypertensive (WHO, 2023)

⚠️ Precautions:
- NSAIDs can increase blood pressure in hypertensive patients
- Monitor kidney function with long-term NSAID use

📌 Recommendation: Consult your cardiologist before combining ibuprofen with BP medication.
🗺️ Doctor Finder
Type a medical query → System auto-detects the specialist type
Enter your city → Interactive map shows nearby healthcare facilities
Each marker includes name, address, phone, and Google Maps directions
📊 Telemetry Panel
Confidence meter based on average cosine similarity
Risk assessment with color-coded indicators
Source cards with similarity score bars per retrieved document
🔌 API Endpoints
Method	Endpoint	Description
POST	/chat	Send a medical query, receive structured answer with sources and medicines
POST	/doctors	Find specialist doctors near a location
GET	/health	Health check
GET	/documents	List loaded PDF documents
Example: /chat
// Request
{ "query": "Can I take ibuprofen for fever while on BP medicine?" }

// Response
{
  "answer": "🔍 **Risk Level**: Moderate\n\n📋 **Condition Analysis**: ...",
  "sources": [
    {
      "content": "NSAIDs may reduce the effectiveness of...",
      "metadata": { "source": "WHO.pdf", "page": 18 },
      "similarity_score": 0.7234
    }
  ],
  "medicines": [
    { "name": "Paracetamol", "usage": "fever and pain relief", "source": "WHO.pdf" }
  ],
  "specialist_type": "cardiologist"
}
👥 Team
Built with ❤️ by:

Nayan Pal — Project Lead
Meghnad Debnath — Developer
Nirupam Ghosh — Developer
Gourab Biswas — Developer
⚠️ Disclaimer
TruthTriage is an educational and research tool. It is NOT a substitute for professional medical advice, diagnosis, or treatment. Always consult a qualified healthcare professional before making any medical decisions. The medicine suggestions are sourced from WHO and government databases and should only be used under medical supervision.

📄 License
This project is for educational purposes. All medical data is sourced from publicly available WHO, CDSCO, and MoHFW documents.
