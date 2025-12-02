# 🚀 RelayDesk  
### An AI-powered personal knowledge & messaging hub  
Full-stack • RAG • Agent Tools • Next.js • FastAPI • PostgreSQL • Vector Search

---

## 🌟 Overview

**RelayDesk** is an AI-powered personal knowledge hub that helps users manage notes, documents, tasks, and conversations — all amplified by Retrieval-Augmented Generation (RAG) and a simple agent-like assistant.

Think of it as a **personal command center** where you can:

- 📝 Take and organise **notes**
- 📄 Upload or paste **documents**
- 🗂 Ask questions using **RAG** over your private knowledge base
- 🤖 Use an **AI assistant** that can answer questions or execute tools (e.g., search notes, create tasks)
- 📅 Track **tasks** created manually or by the assistant
- 🔐 Secure all data with JWT authentication and scoped access

RelayDesk is designed to demonstrate real production engineering skills across **frontend, backend, AI, databases, DevOps, and cloud** — all in one cohesive system.

---

## 🧱 Architecture Summary

RelayDesk consists of five main components:

| Component | Technology | Responsibilities |
|----------|------------|------------------|
| **Frontend** | Next.js + TypeScript | UI, authentication, dashboards, AI chat & RAG interface |
| **Backend** | FastAPI (Python) | REST API, auth, AI logic, RAG pipeline, agent tools |
| **Relational DB** | PostgreSQL | Users, notes, tasks, docs, messages |
| **Vector DB** | Chroma | Embeddings + semantic retrieval for RAG |
| **Object Storage** | S3 / Minio | Stores uploaded files |

A detailed breakdown lives here:  
📄 [`docs/system-design.md`](./docs/system-design.md)

---

## 🧩 Features

### ✨ AI & RAG Capabilities
- Retrieval-Augmented Generation over user documents
- Query embeddings + top-k semantic search
- Tool-driven AI assistant:
  - `search_notes(query)`
  - `create_task(title, due_date)`
- Chat interface with stored message history

### 📝 Notes & Documents
- CRUD notes with rich-text content
- Document ingestion:
  - Paste text directly
  - Upload PDF/HTML/text files
- Automatic chunking → embeddings → vector DB indexing
- Source-linked citations in RAG responses

### 🔐 Authentication & Security
- JWT-based auth
- Input validation with Pydantic
- Per-user data isolation
- CORS, HTTPS (prod), rate-limit-ready design

### 🖥️ Full-Stack Web
- Next.js App Router
- Responsive dashboard UI
- Protected routes, session persistence
- Reusable API client with token injection

### 🛠️ DevOps & Infrastructure
- Dockerfiles for frontend + backend
- `docker-compose.yml` for local sta
