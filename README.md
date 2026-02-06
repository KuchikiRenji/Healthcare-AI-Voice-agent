# Healthcare AI Voice Agent – AI-Powered Medical Assistant & Voice Triage

**Healthcare AI Voice Agent** is a full-stack healthcare application that uses **AI-powered voice interactions** for patient triage, symptom analysis, specialist matching, and appointment booking. Built with **FastAPI**, **PostgreSQL**, **React**, **LangGraph**, and leading AI models (Gemini, GPT-4).

---

## Author & Contact

| | |
|---|---|
| **Author** | **KuchikiRenji** |
| **Email** | [KuchikiRenji@outlook.com](mailto:KuchikiRenji@outlook.com) |
| **GitHub** | [github.com/KuchikiRenji](https://github.com/KuchikiRenji) |
| **Discord** | `kuchiki_renji` |

For questions, collaboration, or support, reach out via the links above or open an issue on GitHub.

---

## Table of Contents

- [What Is Healthcare AI Voice Agent?](#what-is-healthcare-ai-voice-agent)
- [Key Features](#key-features)
- [System Architecture & User Flow](#system-architecture--user-flow)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Quick Start](#quick-start)
- [API Keys & Configuration](#api-keys--configuration)
- [Running the Application](#running-the-application)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## What Is Healthcare AI Voice Agent?

This **healthcare AI assistant** streamlines the patient journey by providing:

- **Intelligent voice triage** – Natural-language symptom collection and analysis via voice
- **AI-powered symptom analysis** – Symptom normalization and specialist recommendation (Gemini, GPT-4, LangGraph)
- **Smart doctor matching** – Healthcare provider lookup by specialization (PostgreSQL)
- **Appointment booking** – Scheduling with optional payment (Razorpay)
- **Voice-enabled UI** – React frontend with Web Speech API for a conversational experience

Ideal for **medical triage**, **symptom checker** projects, **healthcare chatbots**, and **AI voice assistants** in healthcare.

---

## Key Features

| Area | Features |
|------|----------|
| **Voice** | Web Speech API, real-time speech-to-text, voice-guided triage |
| **AI/ML** | Gemini & GPT-4 integration, LangGraph workflows, symptom normalization, specialist mapping |
| **Healthcare** | Doctor database, specialization filter, appointment scheduling, medical history |
| **Payments** | Razorpay integration, test mode, transaction handling |

---

## System Architecture & User Flow

```
Voice UI (React) → Auth (FastAPI + PostgreSQL) → Voice symptoms (Web Speech API)
    → AI analysis (LangGraph + Gemini/GPT-4) → Specialist lookup (DB)
    → Doctor list & slots → Appointment booking → Payment (Razorpay) → Done
```

### Component Flow

1. **Authentication** – Email/password → FastAPI `/login` → `sp_login_user` → JWT/session
2. **Voice pipeline** – Microphone → Web Speech API → text → phrase array → LangGraph
3. **AI workflow** – Raw symptoms → Gemini/GPT-4 → normalized symptoms → specialist mapping → recommendations
4. **Data** – FastAPI → PostgreSQL stored procedures → JSON → frontend
5. **Booking** – Doctor + slot → patient details → slot check → `sp_create_appointment` → payment

### Technical Layers

| Layer | Stack |
|-------|--------|
| **Frontend** | React, Vite, Web Speech API |
| **Backend** | FastAPI, Python |
| **AI** | LangGraph, LangChain, Gemini, GPT-4 |
| **Database** | PostgreSQL, stored procedures |
| **Payments** | Razorpay |

---

## Technology Stack

| Component | Technology |
|-----------|------------|
| Frontend | React 19, Vite, JavaScript |
| Backend | FastAPI, Python, Uvicorn |
| AI/ML | LangGraph, LangChain, OpenAI GPT-4, Google Gemini |
| Database | PostgreSQL |
| Voice | Web Speech API |
| Payments | Razorpay |

---

## Project Structure

```
Healthcare-AI-Voice-agent/
├── backend/                 # FastAPI backend
│   ├── main.py             # API entry, routes
│   ├── config.py           # Configuration
│   ├── db.py               # DB connection
│   ├── models.py           # Pydantic models
│   ├── langgraph_llm_agents.py  # LangGraph AI workflow
│   └── requitements.txt    # Python dependencies
├── src/                    # React frontend (Vite)
│   ├── App.jsx, main.jsx, Root.jsx
│   ├── components/         # Assistant, Dashboard, Recommendation, etc.
│   ├── context/            # UserContext
│   └── gemini.js           # Gemini client
├── sql/
│   ├── schema.sql          # Tables (users, patients, doctors, appointments, etc.)
│   └── functions/          # Stored procedures
│       ├── sp_login_user.sql
│       ├── sp_get_patient_details.sql
│       ├── sp_get_specialists.sql
│       ├── sp_get_doctors_by_specialists.sql
│       ├── sp_get_medical_history.sql
│       ├── sp_get_patient_id.sql
│       └── sp_create_appointment.sql
├── public/
├── index.html
├── package.json            # Frontend deps
├── vite.config.js
└── README.md
```

---

## Quick Start

### Prerequisites

- **Python 3.8+**
- **Node.js 16+**
- **PostgreSQL 12+**
- **Git**

### 1. Database setup

```bash
psql -U postgres -c "CREATE DATABASE healthcare;"
psql -U postgres -c "CREATE USER fastapi_user WITH PASSWORD 'your_secure_password';"
psql -U postgres -c "GRANT ALL PRIVILEGES ON DATABASE healthcare TO fastapi_user;"
```

Apply schema and functions (from project root):

```bash
psql -U fastapi_user -d healthcare -f sql/schema.sql
psql -U fastapi_user -d healthcare -f sql/functions/sp_login_user.sql
psql -U fastapi_user -d healthcare -f sql/functions/sp_get_patient_details.sql
psql -U fastapi_user -d healthcare -f sql/functions/sp_get_specialists.sql
psql -U fastapi_user -d healthcare -f sql/functions/sp_get_doctors_by_specialists.sql
psql -U fastapi_user -d healthcare -f sql/functions/sp_get_medical_history.sql
psql -U fastapi_user -d healthcare -f sql/functions/sp_get_patient_id.sql
psql -U fastapi_user -d healthcare -f sql/functions/sp_create_appointment.sql
```

### 2. Backend setup

```bash
cd backend
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requitements.txt
```

Create `backend/.env` (see [backend/.env.example](backend/.env.example)):

```env
DB_HOST=localhost
DB_PORT=5432
DB_USER=fastapi_user
DB_PASSWORD=your_secure_password
DB_NAME=healthcare
FRONTEND_ORIGIN=http://localhost:5173
OPENAI_API_KEY=your_openai_api_key
GEMINI_API_KEY=your_gemini_api_key
```

Start backend:

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

### 3. Frontend setup

From project root:

```bash
npm install
```

Create `.env.local` (see [.env.local.example](.env.local.example)):

```env
VITE_BACKEND_URL=http://localhost:8000
VITE_GEMINI_API_KEY=your_gemini_api_key
VITE_RAZORPAY_KEY_ID=your_razorpay_key_id
```

Start frontend:

```bash
npm run dev
```

Open **http://localhost:5173**.

---

## API Keys & Configuration

- **OpenAI** – [platform.openai.com](https://platform.openai.com/) → API Keys → add to `backend/.env`
- **Google Gemini** – [Google AI Studio](https://makersuite.google.com/) → API key → backend `.env` and frontend `.env.local`
- **Razorpay** – [Razorpay Dashboard](https://dashboard.razorpay.com/) → Test mode → API keys → frontend `.env.local`

---

## Running the Application

1. Start PostgreSQL.
2. Backend: `cd backend && uvicorn main:app --reload --port 8000`
3. Frontend: from root, `npm run dev`
4. Visit **http://localhost:5173**

**Health check:** `curl http://localhost:8000/`

---

## Troubleshooting

| Issue | What to check |
|-------|----------------|
| Database errors | PostgreSQL running, `.env` credentials, DB and user created |
| Voice not working | Use HTTPS or localhost; allow microphone; check Web Speech API support |
| API errors | No extra spaces in API keys; correct keys in backend vs frontend; quotas |

---

## License

This project is licensed under the **MIT License**. See [LICENSE](LICENSE) for details.

---

**Healthcare AI Voice Agent** – built for better healthcare accessibility.  
**Author:** [KuchikiRenji](https://github.com/KuchikiRenji) · [Contact](mailto:KuchikiRenji@outlook.com) · Discord: `kuchiki_renji`

---

## For GitHub (Description & Topics)

Use the following in your repo **About** section:

**Description (short):**
```text
Full-stack healthcare AI voice assistant: voice triage, symptom analysis, specialist matching & appointment booking. React, FastAPI, LangGraph, PostgreSQL.
```

**Topics (add these in GitHub → About → Topics):**
```text
healthcare
healthcare-ai
voice-assistant
medical-assistant
symptom-checker
patient-triage
langgraph
fastapi
react
postgresql
openai
gemini
razorpay
full-stack
ai-agents
```
