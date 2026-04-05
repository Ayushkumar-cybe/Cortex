# Cortex

**Discover, test, and deploy AI swarms for real business outcomes.**

Cortex is an **AI Agents Marketplace** where users can discover, evaluate, test, and deploy specialized AI swarms for business goals like SEO, lead generation, competitor research, and content operations. Instead of feeling like a prompt library or a generic tools directory, Cortex is designed as a **trust-first execution platform** with live visibility into what each swarm is doing [file:1].

---

## Table of Contents

- [About](#about)
- [Key Features](#key-features)
- [Tech Stack](#tech-stack)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Project Architecture](#project-architecture)
- [API Overview](#api-overview)
- [Socket Events](#socket-events)
- [Database Collections](#database-collections)
- [Development Workflow](#development-workflow)
- [Demo Mode / Replay Fallback](#demo-mode--replay-fallback)
- [Testing](#testing)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

---

## About

Cortex is built around one core idea: users should not need to assemble AI tools manually to get work done. Instead, they should be able to **discover deployable swarms**, inspect their trust signals, try them in a sandbox, and run them with real-time execution visibility [file:1].

A **Swarm** is a packaged multi-agent execution system built for one specific outcome. Each swarm typically includes a lead strategist plus specialist worker agents that execute scoped subtasks in the background [file:1].

---

## Key Features

- **Marketplace Discovery** — browse swarms by category, use-case, and business goal [file:1].
- **Swarm Profile Pages** — view purpose, worker roles, trust indicators, and sample outputs [file:1].
- **Try-It-Now Sandbox** — test a swarm with lightweight input before committing to a full run [file:1].
- **Full Swarm Execution** — launch a swarm run for a real task and receive structured output [file:1].
- **Handshake Protocol** — monitor live execution through a structured, terminal-style event stream [file:1].
- **Proof-of-Execution** — attach run metadata and trust-backed completion records [file:1].
- **Replay / Golden Path Mode** — show a stable fallback demo using prerecorded execution traces [file:1].
- **Privacy-Aware Inference** — sanitize sensitive values before sending prompts to AI services [file:1].

---

## Tech Stack

Cortex is designed as a startup-grade MVP with a practical, hackathon-friendly stack [file:1].

- **Frontend:** React, Vite, TailwindCSS [file:1]
- **Backend:** Node.js, Express [file:1]
- **Realtime:** Socket.io [file:1]
- **Database:** MongoDB Atlas [file:1]
- **AI Runtime:** Groq / Llama [file:1]
- **Optional Trust Layer:** Polygon, Solidity, Ethers.js [file:1]
- **Deployment:** Vercel, Render / Railway, MongoDB Atlas [file:1]

---

## Repository Structure

Cortex is intended to be built as a **monorepo** organized around product domains, not just frontend/backend separation [file:1].

```txt
cortex/
├─ apps/
│  ├─ server/
│  │  └─ src/
│  │     ├─ app/
│  │     ├─ routes/
│  │     ├─ controllers/
│  │     ├─ modules/
│  │     │  ├─ marketplace/
│  │     │  ├─ swarms/
│  │     │  ├─ runs/
│  │     │  ├─ sandbox/
│  │     │  ├─ replay/
│  │     │  ├─ proof/
│  │     │  ├─ privacy/
│  │     │  ├─ creator/
│  │     │  └─ sockets/
│  │     ├─ orchestrator/
│  │     ├─ adapters/
│  │     ├─ db/
│  │     ├─ middleware/
│  │     ├─ validators/
│  │     └─ utils/
│  └─ web/
│     └─ src/
│        ├─ pages/
│        ├─ routes/
│        ├─ features/
│        │  ├─ marketplace/
│        │  ├─ swarm-profile/
│        │  ├─ sandbox/
│        │  ├─ workspace/
│        │  ├─ handshake/
│        │  └─ creator/
│        ├─ components/
│        ├─ services/
│        ├─ store/
│        ├─ hooks/
│        └─ lib/
├─ packages/
│  ├─ shared-types/
│  ├─ shared-schemas/
│  ├─ shared-events/
│  └─ shared-config/
├─ data/
│  ├─ seed/
│  └─ replay-traces/
├─ contracts/
├─ scripts/
└─ README.md
```

---

## Getting Started

### Prerequisites

- Node.js 18+ or 20+
- npm / pnpm / yarn
- MongoDB Atlas connection string
- AI provider API key
- Optional: Polygon wallet / RPC config for proof anchoring

### 1. Clone the repository

```bash
git clone https://github.com/your-username/cortex.git
cd cortex
```

### 2. Install dependencies

If you are using a monorepo with separate apps and packages:

```bash
npm install
```

Or, if your workspace uses pnpm:

```bash
pnpm install
```

### 3. Configure environment variables

Create a `.env` file in the backend and frontend as needed.

```bash
cp .env.example .env
```

### 4. Start the development servers

#### Backend

```bash
cd apps/server
npm run dev
```

#### Frontend

```bash
cd apps/web
npm run dev
```

If you have a workspace script at the root:

```bash
npm run dev
```

### 5. Open the app

- Frontend: `http://localhost:5173`
- Backend: `http://localhost:5000` or your configured port

---

## Environment Variables

### Backend `.env`

```env
PORT=5000
MONGODB_URI=your_mongodb_connection_string
GROQ_API_KEY=your_groq_api_key
JWT_SECRET=your_jwt_secret
CLIENT_URL=http://localhost:5173

# Optional trust layer
POLYGON_RPC_URL=your_polygon_rpc_url
PRIVATE_KEY=your_wallet_private_key
CONTRACT_ADDRESS=your_contract_address
```

### Frontend `.env`

```env
VITE_API_BASE_URL=http://localhost:5000
VITE_SOCKET_URL=http://localhost:5000
```

---

## Project Architecture

Cortex is built around four core product layers [file:1]:

1. **Marketplace Discovery**  
   Users browse, search, and filter AI swarms.

2. **Swarm Execution**  
   A user submits a task and launches a run session.

3. **Trust Visibility**  
   Handshake Protocol shows real-time execution steps and progress.

4. **Creator Deployability**  
   Creators can package and publish swarms into the marketplace.

The system treats every execution as a **run session** with a unique run ID, streamed events, final output, and proof metadata [file:1].

---

## API Overview

Below is the intended API surface for Cortex [file:1].

### Marketplace

- `GET /api/swarms`
- `GET /api/swarms/:slug`
- `GET /api/categories`

### Sandbox

- `POST /api/sandbox/run`

### Execution

- `POST /api/runs/start`
- `GET /api/runs/:runId`
- `GET /api/runs/:runId/result`

### Replay

- `POST /api/replay/start`

### Proof

- `POST /api/proof/generate`
- `GET /api/proof/:runId`

### Creator

- `POST /api/creators/swarms/validate`
- `POST /api/creators/swarms/deploy`
- `GET /api/creators/swarm/:id/status`

---

## Socket Events

Cortex’s **Handshake Protocol** should use structured Socket.io events rather than raw logs [file:1].

### Core event types

- `run:started`
- `agent:delegated`
- `agent:working`
- `agent:output`
- `run:progress`
- `run:completed`
- `run:failed`
- `run:replay-mode`

These events should be rendered in the UI as a readable terminal-style execution feed [file:1].

---

## Database Collections

Cortex’s MongoDB data model should stay lean and product-oriented [file:1].

### `swarms`
Stores swarm definitions and marketplace metadata.

### `creators`
Stores creator identity and deploy metadata.

### `runs`
Stores execution sessions and final results.

### `runevents`
Stores the live event stream for Handshake Protocol and replay.

### `proofs`
Stores proof-of-execution metadata.

### `replayruns`
Stores deterministic demo-safe fallback traces.

---

## Development Workflow

A good build order for Cortex is [file:1]:

1. Backend foundation.
2. Swarm registry.
3. Run system.
4. Orchestrator.
5. Handshake Protocol.
6. Replay fallback.
7. Privacy layer.
8. Proof layer.
9. Sandbox mode.
10. Frontend product shell.

This order keeps the execution spine working before the UI is fully polished [file:1].

---

## Demo Mode / Replay Fallback

Cortex should always support a **demo-safe replay mode**. If live AI or runtime calls fail, the app should replay a believable execution trace through the same UI and event stream so the demo remains stable [file:1].

This is a core product requirement, not just a nice-to-have [file:1].

---

## Testing

Before demo day, test the following flows carefully [file:1]:

- Marketplace loads correctly.
- Swarm profile pages render without missing data.
- Sandbox run can start and finish.
- Full deployment run creates a run session.
- Handshake Protocol streams events in order.
- Replay mode uses the same UI surface as live mode.
- Proof metadata is attached after completion.
- No sensitive data leaks into logs or prompts.

---

## Deployment

### Frontend

Deploy the frontend to Vercel or a similar static hosting platform.

### Backend

Deploy the backend to Render or Railway.

### Database

Use MongoDB Atlas for production storage.

### Optional trust layer

If enabled, deploy smart contracts to Polygon and keep proof anchoring lightweight.

---

## Contributing

If you are extending Cortex, keep these rules in mind:

- Preserve the marketplace-first product direction.
- Keep execution visible and trustworthy.
- Avoid overengineering the orchestration layer.
- Keep replay mode available for demo safety.
- Prefer clear product modules over deeply coupled logic.

### Suggested contribution flow

1. Create a feature branch.
2. Build one module at a time.
3. Test the primary demo path.
4. Open a pull request with screenshots or flow notes.

---

## License

Add your project license here.

---

## Acknowledgements

Built for an AI Agents Marketplace problem statement with an emphasis on:
- discovery,
- showcasing,
- deployment,
- trust,
- and demo reliability [file:1].
