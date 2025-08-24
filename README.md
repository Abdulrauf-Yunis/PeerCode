# PeerCode
a# 🖊️ PeerCode — Real-time Collaborative Code Editor with Sandboxed Runner

PeerCode is a collaborative coding platform that lets multiple users **edit code in real time (CRDT-powered)**, run it in **secure ephemeral sandboxes**, and **replay sessions** for training, interviews, and team collaboration.  

This project demonstrates **real-world engineering depth**:  
- Real-time collaboration (Yjs + WebSockets)  
- Secure containerized execution environments  
- CRDT-based conflict resolution  
- Session recording & replay  
- AI-assisted feedback (planned)  

---

## 🚀 Features
- **Real-time code editing** — powered by CRDTs (Yjs), multiple users edit simultaneously without conflicts.  
- **Presence indicators** — see collaborator cursors & usernames live.  
- **Sandboxed code execution** — Docker containers with CPU/memory limits, streamed logs.  
- **Session recording & replay** — watch collaborative sessions like a video.  
- **Multi-language support** — JavaScript & Python (MVP).  
- **AI-assisted code review (planned)** — automated feedback & hints.  

---

## 🏗️ Architecture

### High-level overview
- **Frontend (React + TypeScript)** — Monaco editor, CRDT sync, replay UI  
- **Realtime Gateway (Node.js + WebSocket)** — handles Yjs document sync & presence  
- **Sandbox Runner (Docker + Redis queue)** — spins up ephemeral containers for code execution  
- **Backend API (NestJS/Express)** — auth, sessions, replay storage  
- **Storage** — PostgreSQL (users/sessions), Redis (queue/state), S3 (session replays)

### Diagram (Mermaid)
```mermaid
flowchart TD
    subgraph Frontend [Frontend: React + TS]
        E[Monaco Editor]
        R[Replay Player]
    end

    subgraph Gateway [Realtime Gateway: Node.js + WebSocket]
        Y[Yjs Sync]
        P[Presence Service]
    end

    subgraph Backend [API Backend: NestJS/Express]
        A[Auth Service]
        M[Metadata API]
    end

    subgraph Runner [Sandbox Runner: Docker + Redis]
        Q[Job Queue]
        C[Ephemeral Containers]
    end

    subgraph Storage
        DB[(PostgreSQL)]
        Redis[(Redis)]
        S3[(S3 / Minio)]
    end

    E <-- CRDT Sync --> Y
    R <-- Replay Data --> M
    Y --> DB
    P --> Redis
    A --> DB
    M --> DB
    Q --> C
    C --> Redis
    C --> S3

