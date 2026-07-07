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

| Technology | Version | Why This Choice |
|---|---|---|
| **Node.js** | 18+ | Event-driven, non-blocking I/O — ideal for handling many concurrent WebSocket connections without thread overhead |
| **Express.js** | 4.x | Minimal, unopinionated framework — gives full control over middleware pipeline and routing |
| **Socket.io** | 4.x | Handles WebSocket connections with automatic fallback to HTTP long-polling; built-in room abstraction maps perfectly to the room model |
| **Mongoose** | 8.x | ODM for MongoDB — schema validation, refs between collections, and clean async/await API |
| **MongoDB** | 7.x | Flexible document model — code strings of any length, embedded subdocuments for chat/history, fast writes for frequent code updates |

### Authentication

| Technology | Why This Choice |
|---|---|
| **Passport.js** | Industry-standard auth middleware; pluggable strategies mean the same pipeline works for local, OAuth, JWT |
| **passport-local** | Username/password strategy; delegates to the User model's `authenticate()` method |
| **passport-local-mongoose** | Mongoose plugin that adds `register()`, `authenticate()`, bcrypt hashing, and salt management to the User schema automatically |
| **jsonwebtoken** | Stateless API tokens — no session lookup needed for API routes; signed with HS256 and a server secret |
| **express-session** | Server-side sessions for web routes; session ID stored in an HttpOnly cookie |
| **connect-mongo** | Persists sessions to MongoDB so they survive server restarts |
| **bcryptjs** | Used internally by passport-local-mongoose; adaptive hashing with salt means rainbow table attacks are infeasible |

### Views & Middleware

| Technology | Why This Choice |
|---|---|
| **EJS** | Server-side templating with partials — no build step, no bundler, straightforward `res.render()` model |
| **connect-flash** | One-time session messages for user feedback (success/error) — cleared after being read once |
| **method-override** | HTML forms only support GET/POST; this middleware intercepts `?_method=DELETE` to enable RESTful routes |
| **morgan** | HTTP request logger — logs method, path, status, and response time in development |
| **dotenv** | Keeps secrets (DB URI, JWT secret) out of source code; loaded from `.env` at startup |

### Frontend (In-browser)

| Technology | Why This Choice |
|---|---|
| **CodeMirror 5** | Battle-tested browser code editor; 15+ language modes, Dracula theme, bracket matching — no build step needed via CDN |
| **Socket.io Client** | Pairs with the server-side Socket.io; handles reconnection automatically |
| **Vanilla JS** | No React/Vue needed — the editor's real-time logic is event-driven and doesn't require a virtual DOM |

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        BROWSER                              │
│                                                             │
│   ┌─────────────┐    WebSocket     ┌──────────────────┐    │
│   │  CodeMirror │◄────────────────►│  Socket.io Client│    │
│   │   Editor    │                  └──────────────────┘    │
│   └─────────────┘         HTTP         ┌──────────────┐    │
│                      ◄────────────────►│  EJS Pages   │    │
│                                        └──────────────┘    │
└──────────────────────────────────┬──────────────────────────┘
                                   │
                    ┌──────────────▼──────────────┐
                    │         NODE.JS SERVER       │
                    │                             │
                    │  ┌─────────┐  ┌──────────┐  │
                    │  │ Express │  │Socket.io │  │
                    │  │ Routes  │  │ Handler  │  │
                    │  └────┬────┘  └────┬─────┘  │
                    │       │            │         │
                    │  ┌────▼────────────▼─────┐  │
                    │  │      Middleware        │  │
                    │  │  Passport · Session   │  │
                    │  │  Flash · JWT · Morgan │  │
                    │  └────────────┬──────────┘  │
                    │               │             │
                    └───────────────┼─────────────┘
                                    │
                    ┌───────────────▼─────────────┐
                    │           MONGODB            │
                    │                             │
                    │  users · rooms · sessions   │
                    └─────────────────────────────┘
```

---

## Data Flow — What Happens When You Type

```
You press a key
    │
    ▼
CodeMirror fires 'change' event
    │
    ▼
300ms debounce resets (batches fast typing)
    │
    ▼  (after 300ms pause)
socket.emit('code-change', { roomId, code, language })
    │
    ▼  [crosses the internet via WebSocket]
Server receives 'code-change'
    │
    ├──► socket.to(roomId).emit('code-update') ──► All other users' browsers
    │                                                      │
    │                                                      ▼
    │                                             editor.setValue(newCode)
    │                                             (cursor position preserved)
    │
    └──► Room.findOneAndUpdate({ code }) ──► MongoDB persisted ✓
```

---

## Folder Structure

```
devcollab/
│
├── bin/
│   └── www                   ← Server entry point; boots HTTP + Socket.io
│
├── config/
│   ├── db.js                 ← Mongoose connection
│   ├── passport.js           ← Local strategy + serialize/deserialize
│   └── socket.js             ← All real-time event handlers
│
├── middleware/
│   └── auth.js               ← isLoggedIn · isLoggedOut · generateToken
│                               verifyToken · isRoomOwner
│
├── models/
│   ├── User.js               ← Schema + passport-local-mongoose plugin
│   └── Room.js               ← Schema with embedded chat + history
│
├── routes/
│   ├── index.js              ← GET / · GET /dashboard
│   ├── auth.js               ← /auth/* — register, login, logout, token
│   ├── room.js               ← /room/* — create, join, editor, save, delete
│   └── api.js                ← /api/* — JWT-protected REST endpoints
│
├── views/
│   ├── partials/
│   │   ├── header.ejs        ← Navbar + flash messages (included on every page)
│   │   └── footer.ejs        ← Footer + closing HTML tags
│   ├── auth/                 ← login · register · profile · token
│   ├── room/                 ← editor · new · join · history
│   ├── dashboard.ejs
│   ├── index.ejs             ← Landing page
│   └── error.ejs
│
├── public/
│   └── css/
│       ├── style.css         ← Sunset Dusk theme · all web pages
│       └── editor.css        ← Full-screen editor layout
│
├── app.js                    ← Express app · middleware · route registration
├── .env                      ← Secrets (not committed)
└── package.json
```

---

## API Reference

All API routes require `Authorization: Bearer <token>` header.
Get your token at `/auth/token` after logging in.

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| `GET` | `/api/me` | Returns current user's info | JWT |
| `GET` | `/api/rooms` | Lists all rooms owned by user | JWT |
| `GET` | `/api/rooms/:roomId` | Returns room details + current code | JWT |
| `POST` | `/api/rooms/:roomId/code` | Updates room code via API | JWT |

```bash
# Example: fetch your rooms
curl -H "Authorization: Bearer YOUR_TOKEN" \
  https://devcollab.up.railway.app/api/rooms
```

---

## Web Routes

| Method | Route | Description | Guard |
|---|---|---|---|
| `GET` | `/` | Landing page | Public |
| `GET` | `/dashboard` | User dashboard | Session |
| `GET` | `/auth/register` | Register form | Guest only |
| `POST` | `/auth/register` | Create account | Guest only |
| `GET` | `/auth/login` | Login form | Guest only |
| `POST` | `/auth/login` | Authenticate | Guest only |
| `DELETE` | `/auth/logout` | End session | Session |
| `GET` | `/auth/token` | View JWT token | Session |
| `GET` | `/room/new` | Create room form | Session |
| `POST` | `/room` | Create room | Session |
| `GET` | `/room/join` | Join room form | Session |
| `POST` | `/room/join` | Join by Room ID | Session |
| `GET` | `/room/:roomId` | Open editor | Session |
| `POST` | `/room/:roomId/save` | Save code snapshot | Session |
| `GET` | `/room/:roomId/history` | View snapshots | Session + Owner |
| `DELETE` | `/room/:roomId` | Delete room | Session + Owner |

---

## Socket.io Events

### Client → Server

| Event | Payload | Trigger |
|---|---|---|
| `join-room` | `{ roomId, username }` | Page load |
| `code-change` | `{ roomId, code, language }` | After 300ms debounce |
| `language-change` | `{ roomId, language }` | Language dropdown change |
| `cursor-move` | `{ roomId, position }` | Cursor activity |
| `chat-message` | `{ roomId, message }` | Chat send |

### Server → Client

| Event | Payload | Meaning |
|---|---|---|
| `code-snapshot` | `{ code, language }` | Initial code on room join |
| `code-update` | `{ code, language }` | Peer changed the code |
| `room-users` | `[{ username, color }]` | User list updated |
| `user-joined` | `{ username }` | Someone joined |
| `user-left` | `{ username }` | Someone disconnected |
| `cursor-update` | `{ username, color, position }` | Peer cursor moved |
| `chat-message` | `{ username, color, message, timestamp }` | Incoming chat |

---

## Key Engineering Decisions

**Why full-string sync instead of diffs?**
Operational transforms (Google Docs) and CRDTs (Figma) handle conflict resolution when two users edit the same character simultaneously — but they require complex state machines and merge algorithms. Full-string sync is simpler and correct for the collaboration pattern this app is built for: developers who coordinate via chat. It's a deliberate trade-off, not an oversight. The next evolution is Yjs CRDT integration.

**Why MongoDB for sessions instead of Redis?**
Redis would be faster for session lookups, but introduces another infrastructure dependency. MongoDB is already required for the app's data — using connect-mongo keeps the deployment to a single external service. At the current scale (single Node.js instance), MongoDB session latency is imperceptible.

**Why EJS instead of React?**
The majority of pages (landing, dashboard, auth) are server-rendered with data from MongoDB and don't require a reactive UI. EJS with `res.render()` is faster to build, simpler to reason about, and has zero build step. The editor page — the only page that needs reactivity — handles it with vanilla JS event listeners and CodeMirror's own API.

**Why two auth systems?**
Web browsers use cookies — session-based auth is the natural, secure model for pages. REST API clients (curl, mobile apps, scripts) can't easily manage cookies — JWT is the standard stateless approach. Supporting both makes DevCollab a complete platform, not just a website.

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

## Running Locally

```bash
# Prerequisites: Node.js 18+, MongoDB running locally

git clone https://github.com/YOUR_USERNAME/devcollab.git
cd devcollab
npm install

# Configure environment
cp .env.example .env
# Edit .env with your MongoDB URI and secret keys

npm run dev
# → http://localhost:3000
```

---

## Deployment

Deployed on **Railway** with **MongoDB Atlas** (M0 free tier).

```
Railway  ──►  Node.js process (bin/www)
                    │
                    ├──► Express HTTP routes
                    └──► Socket.io WebSocket server
                                │
                         MongoDB Atlas
                    (users · rooms · sessions)
```

Environment variables managed via Railway dashboard. Auto-deploys on every push to `main`.

---

<div align="center">

**Built by Hemant**

*Node.js · Express · Socket.io · MongoDB · Passport · JWT · EJS*

</div>
