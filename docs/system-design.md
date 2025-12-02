# RelayDesk – System Design

## 1. Overview

RelayDesk is an AI-powered personal knowledge & messaging hub.  
Users can:

- Create and manage **notes** and **tasks**
- Upload or paste **documents** into their knowledge base
- **Chat with an AI assistant**
- Ask **RAG-style questions** over their own data
- Let an AI **“assistant” tool** search notes and create tasks

The system is composed of:

- **Frontend:** Next.js + TypeScript (browser UI)
- **Backend:** FastAPI (Python REST API + AI logic)
- **Relational DB:** PostgreSQL (users, notes, messages, tasks, document metadata)
- **Vector DB:** Chroma / similar (document + note embeddings for RAG)
- **Object Storage:** S3-compatible (optional, for larger raw documents / uploads)

Everything is designed to run locally via `docker-compose`, and to be deployable to AWS (EC2 + RDS + S3, etc.).

---

## 2. Core Components

### 2.1 Frontend (Next.js + TypeScript)

**Responsibilities**

- Authentication flows (register, login)
- Dashboard UI for:
  - Notes (create, list, search)
  - Tasks (list, create, status updates)
  - Documents (upload/paste, list)
  - AI Chat and RAG queries
  - “Assistant” interface (agent-like UI)
- Calling backend REST APIs, sending/receiving JSON
- Client-side routing and basic state management

**Key Pages**

- `/login` / `/register` – user onboarding and auth
- `/dashboard`
  - Notes panel
  - Documents/RAG panel
  - Tasks panel
  - AI chat / Assistant panel

---

### 2.2 Backend (FastAPI)

**Responsibilities**

- Expose RESTful API endpoints for:
  - Auth (`/auth/register`, `/auth/login`)
  - Notes (`/notes`)
  - Tasks (`/tasks`)
  - Documents (`/documents`)
  - AI (`/ai/chat`, `/ai/rag-query`, `/ai/assistant`)
- Implement business logic and validation
- Interact with:
  - PostgreSQL (via ORM, e.g. SQLAlchemy)
  - Vector DB for embeddings and retrieval
  - Object storage for document files (if used)
  - OpenAI (or other LLM provider) for AI responses
- Enforce authentication & authorization using JWT

**Key API Flows**

- Incoming HTTP → FastAPI endpoint → validation → DB/Vector DB/LLM → JSON response back to frontend

---

### 2.3 PostgreSQL (Relational Database)

**Responsibilities**

- Source of truth for structured data:
  - Users
  - Notes
  - Messages
  - Documents (metadata)
  - Tasks

**Core Tables (simplified)**

- `users`

  - `id` (PK)
  - `email`, `hashed_password`
  - `created_at`

- `notes`

  - `id` (PK)
  - `user_id` (FK → users)
  - `title`
  - `content` (text)
  - `created_at`, `updated_at`

- `messages` (for chat transcripts, optional)

  - `id` (PK)
  - `user_id` (FK → users)
  - `role` (`user` | `assistant` | `system`)
  - `content` (text)
  - `created_at`

- `documents`

  - `id` (PK)
  - `user_id` (FK → users)
  - `title`
  - `source_type` (`uploaded_file`, `pasted_text`, `note`, etc.)
  - `storage_location` (e.g. S3 key, local path, or inline)
  - `created_at`

- `tasks`
  - `id` (PK)
  - `user_id` (FK → users)
  - `title`
  - `description`
  - `status` (`todo`, `in_progress`, `done`)
  - `due_date` (nullable)
  - `created_at`, `updated_at`
