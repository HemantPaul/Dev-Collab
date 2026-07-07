<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=700&size=32&pause=1000&color=C2566A&center=true&vCenter=true&width=600&lines=⚡+DevCollab;Real-time+Collaborative+Code+Editor;Built+for+Engineers%2C+by+an+Engineer" alt="DevCollab" />

<br/>

[![Node.js](https://img.shields.io/badge/Node.js-18+-339933?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Express](https://img.shields.io/badge/Express.js-4.x-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com)
[![MongoDB](https://img.shields.io/badge/MongoDB-7.x-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](https://mongodb.com)
[![Socket.io](https://img.shields.io/badge/Socket.io-4.x-010101?style=for-the-badge&logo=socket.io&logoColor=white)](https://socket.io)
[![JWT](https://img.shields.io/badge/JWT-Auth-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)](https://jwt.io)
[![Passport](https://img.shields.io/badge/Passport.js-Local-34E27A?style=for-the-badge&logo=passport&logoColor=white)](http://passportjs.org)

<br/>

> **Multi-user collaborative code editor** — real-time sync, room-based auth, live cursors,
> persistent session history, and a REST API. Built entirely from scratch.

**[🌐 Live Demo](https://devcollab.up.railway.app)** &nbsp;·&nbsp; **[📂 Source Code](../devcollab/)** &nbsp;·&nbsp; **[📸 Screenshots](#screenshots)**

</div>

---

## What Is This?

DevCollab is a **browser-based code editor where multiple developers write code together simultaneously** — the same way Google Docs lets multiple people edit a document, but purpose-built for code.

A user creates a **Room**, shares the Room ID with teammates, and everyone who joins sees the same editor. Every keystroke from any user appears on everyone else's screen in under 200ms. The session persists in MongoDB, so the code is never lost even if everyone disconnects.

---

## The Problem It Solves

| Scenario | Without DevCollab | With DevCollab |
|---|---|---|
| Remote pair programming | Screen sharing (one-way, laggy) | Both engineers type live in the same editor |
| Technical interviews | Sharing code over chat/paste | Interviewer watches candidate code in real time |
| Teaching / code review | Talk through code verbally | Instructor edits directly alongside student |
| Quick collaboration | Git commit + push + pull cycle | Instant shared editor, no setup |

---

## Core Features

### ⚡ Real-time Code Sync

### 🔐 Dual-layer Authentication

### 👥 Live Presence

### 💬 Built-in Chat

### 🗂️ Session History

### 🌐 REST API

---

## Tech Stack — Every Choice Explained

### Backend

| Technology | Version |
|---|---
| **Node.js** | 18+ | 
| **Express.js** | 4.x |
| **Socket.io** | 4.x | 
| **Mongoose** | 8.x | 
| **MongoDB** | 7.x |

### Authentication

| Technology |
|---|
| **Passport.js** |
| **passport-local** | 
| **passport-local-mongoose** | 
| **jsonwebtoken** |
| **express-session** |
| **connect-mongo** |
| **bcryptjs** | 

### Views & Middleware

| Technology | 
|---|
| **EJS** |
| **connect-flash** |
| **method-override** | 
| **morgan** |
| **dotenv** |

### Frontend (In-browser)

| Technology | 
|---|
| **CodeMirror 5** | 
| **Socket.io Client** | 
| **Vanilla JS** |

---

## Key Engineering Decisions

**Why a simplified synchronization approach?**
Real-time collaborative editing can be implemented using advanced conflict-resolution techniques such as Operational Transforms (OT) or CRDTs. For this project, I chose a simpler synchronization strategy that provides responsive collaboration while keeping the implementation maintainable. More advanced synchronization algorithms are planned as a future enhancement.

**Why a document database?**
A document-oriented database aligns well with the application's collaborative data model, making it straightforward to store rooms, users, and session history while keeping the deployment architecture simple. This approach balances development speed, scalability, and maintainability.

**Why server-side rendering?**
Most pages are content-driven and don't require a complex client-side framework. A lightweight server-rendered approach reduces application complexity, improves performance, and allows development effort to focus on the real-time collaboration experience.

**Why support both browser and API authentication?**
The application serves both interactive users and external clients. Supporting browser-based authentication alongside token-based API access provides a secure experience for web users while enabling integrations with scripts, automation tools, and future client applications.

---

## Known Limitations & Roadmap

| Limitation | Planned Solution |
|---|---|
| Last-write-wins on simultaneous same-line edits | Yjs CRDT library for proper conflict resolution |
| Single Node.js instance (no horizontal scaling) | Redis adapter for Socket.io + Redis session store |
| No code execution | Judge0 API integration (sandboxed runner) |
| No automated tests | Jest + Supertest for route and socket testing |
| No rate limiting on auth routes | express-rate-limit middleware |

---

## Deployment

Deployed on **Railway** with **MongoDB Atlas** (M0 free tier).

Environment variables managed via Railway dashboard. Auto-deploys on every push to `main`.

---

<div align="center">

**Built by Hemant Paul**

</div>
