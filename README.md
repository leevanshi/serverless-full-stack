
# Serverless Stack

A modern full-stack application built with TypeScript, React, and Node.js.


## Tech Stack

- **Package Manager**: pnpm workspaces
- **Runtime**: Node.js 24
- **Language**: TypeScript 5.9
- **Frontend**: React 19, Vite 7
- **Backend**: Express 5
- **Database**: PostgreSQL with Drizzle ORM
- **Validation**: Zod
- **Styling**: TailwindCSS 4
- **Build**: esbuild

## Project Structure

This is a monorepo using pnpm workspaces:

- `artifacts/` - Application artifacts (frontend apps, API server)
- `lib/` - Shared libraries and integrations
- `scripts/` - Build and utility scripts

## Getting Started

### Prerequisites

- Node.js 24+
- pnpm 9+
- PostgreSQL 16+

### Installation
=======
<div align="center">

# 🌐 Slang Translator

### Translate slang, internet lingo, and regional expressions — powered by OpenAI.

A full-stack SaaS application built on a **pnpm monorepo** with a contract-first API, React 19 frontend, Express 5 backend, PostgreSQL database, and a dedicated OpenAI integration layer.

[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-19-149ECA?style=flat-square&logo=react&logoColor=white)](https://react.dev/)
[![Express](https://img.shields.io/badge/Express-5-000000?style=flat-square&logo=express&logoColor=white)](https://expressjs.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Drizzle_ORM-4169E1?style=flat-square&logo=postgresql&logoColor=white)](https://orm.drizzle.team/)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT-412991?style=flat-square&logo=openai&logoColor=white)](https://openai.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=flat-square)](#license)

</div>

---

## 📖 Overview

Slang Translator is a full-stack web application that uses OpenAI to translate slang terms, internet lingo, Gen-Z expressions, regional jargon, and cultural phrases into plain language — and vice versa. The project is structured as a **pnpm monorepo** with a contract-first OpenAPI architecture, so the frontend and backend are always in sync via generated TypeScript types and React Query hooks.

---

## ✨ Features

- 🔤 **AI-powered translations** — uses OpenAI GPT to explain slang, acronyms, and internet expressions in plain language
- 🌍 **Multi-dialect support** — handles regional slang, Gen-Z lingo, AAVE, British/Australian colloquialisms, and more
- ⚡ **Contract-first API** — a single OpenAPI spec generates both server-side Zod validators and fully-typed React Query client hooks, eliminating runtime type mismatches
- 🗄️ **Persistent translation history** — all translations are stored in PostgreSQL via Drizzle ORM for lookup and reuse
- 🎨 **Polished UI** — built with shadcn/ui (Radix primitives), TailwindCSS 4, and Framer Motion animations
- 🔁 **Resilient AI calls** — the server-side OpenAI integration uses `p-retry` and `p-limit` for automatic retries and concurrency control

---

## 🧱 Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 19, TypeScript, Vite 7, TailwindCSS 4, shadcn/ui (Radix UI), wouter, TanStack Query, React Hook Form, Framer Motion, Recharts |
| Backend | Node.js 24, Express 5, Pino (structured logging), cookie-parser, cors |
| AI Integration | OpenAI SDK, `p-retry` (retries), `p-limit` (concurrency) |
| Database | PostgreSQL, Drizzle ORM, `drizzle-zod` |
| API Contract | OpenAPI spec → **Orval** codegen → Zod schemas + React Query hooks |
| Tooling | pnpm workspaces, TypeScript project references, esbuild |

---

## 🏗️ Architecture

The repo is a pnpm monorepo split into `artifacts/` (deployable apps) and `lib/` (shared packages). The OpenAPI spec in `lib/api-spec` is the **single source of truth** — Orval codegen derives both the server-side Zod validators and the client-side React Query hooks from it, so the API contract is always consistent.

```
                    ┌──────────────────────────────┐
                    │     lib/api-spec/openapi.yaml │
                    │       (source of truth)       │
                    └──────────────┬───────────────┘
                                   │  Orval codegen
                  ┌────────────────┴────────────────┐
                  ▼                                   ▼
      ┌───────────────────────┐        ┌──────────────────────────┐
      │    lib/api-zod         │        │  lib/api-client-react     │
      │  (Zod request/response │        │  (generated React Query   │
      │   validators)          │        │   hooks + TS types)       │
      └──────────┬────────────┘        └─────────────┬────────────┘
                 │                                     │
                 ▼                                     ▼
      ┌──────────────────────┐         ┌──────────────────────────┐
      │ artifacts/api-server  │◄───────►│  artifacts/slang-         │
      │ Express 5 REST API    │  HTTP   │  translator (React + Vite) │
      └──────────┬────────────┘         └──────────────────────────┘
                 │
        ┌────────┴──────────┐
        │                   │
        ▼                   ▼
┌──────────────┐   ┌─────────────────────────────┐
│   lib/db      │   │ lib/integrations-openai-     │
│ Drizzle ORM   │   │ ai-server                    │
│ + PostgreSQL  │   │ (OpenAI SDK + retry/limit)   │
└──────────────┘   └─────────────────────────────┘
```

---

## 📁 Project Structure

```
serverless-full-stack/
├── artifacts/
│   ├── slang-translator/        # Frontend — React 19 + Vite app
│   │   └── src/
│   │       ├── pages/            # App routes/pages
│   │       ├── components/       # UI components (shadcn/ui)
│   │       ├── hooks/            # Custom React hooks
│   │       └── lib/              # Utilities and API helpers
│   ├── api-server/               # Backend — Express 5 REST API
│   │   └── src/
│   │       └── routes/           # API route handlers
│   └── mockup-sandbox/           # Internal design preview sandbox
├── lib/
│   ├── api-spec/                 # openapi.yaml (source of truth) + Orval config
│   ├── api-zod/                  # Generated Zod schemas (server-side validation)
│   ├── api-client-react/         # Generated TanStack Query hooks (client-side)
│   ├── db/                       # Drizzle ORM schema + PostgreSQL connection
│   ├── integrations-openai-ai-server/  # Server-side OpenAI integration (retry + concurrency)
│   └── integrations-openai-ai-react/   # Client-side OpenAI React utilities
└── scripts/                      # Workspace utility scripts
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** 24+
- **pnpm** 9+
- **PostgreSQL** running locally or hosted
- An **OpenAI API key**

### 1. Clone the repository

```bash
git clone https://github.com/leevanshi/serverless-full-stack.git
cd serverless-full-stack
```

### 2. Install dependencies
>>>>>>> 442ec6fdf3d65d846472567268b55c81b41e3880

```bash
pnpm install
```

<<<<<<< HEAD
### Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/dbname
AI_INTEGRATIONS_OPENAI_API_KEY=your_openai_api_key
AI_INTEGRATIONS_OPENAI_BASE_URL=https://api.openai.com/v1
```

### Running the Project

```bash
# Run the API server
pnpm --filter @workspace/api-server run dev

# Run typecheck across all packages
pnpm run typecheck

# Build all packages
pnpm run build

# Regenerate API hooks and Zod schemas from OpenAPI spec
pnpm --filter @workspace/api-spec run codegen

# Push DB schema changes (dev only)
pnpm --filter @workspace/db run push
```

## Development

### Database Setup

```bash
# Push schema changes to database
pnpm --filter @workspace/db run push

# Generate database client
pnpm --filter @workspace/db run generate
```

### API Development

The API server uses OpenAPI specifications. After modifying the OpenAPI spec:

```bash
pnpm --filter @workspace/api-spec run codegen
```

This regenerates:
- TypeScript types
- React hooks for API calls
- Zod schemas for validation

## Available Scripts

- `pnpm run typecheck` - Full typecheck across all packages
- `pnpm run build` - Typecheck + build all packages
- `pnpm run typecheck:libs` - Typecheck library packages only

## License

MIT
=======
### 3. Configure environment variables

```bash
cp .env.example .env
```

Fill in your `.env`:

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/slang_translator

# OpenAI
AI_INTEGRATIONS_OPENAI_API_KEY=your_openai_api_key
AI_INTEGRATIONS_OPENAI_BASE_URL=https://api.openai.com/v1

# Server port (optional)
PORT=8080
```

### 4. Set up the database

```bash
pnpm --filter @workspace/db run push
```

### 5. Run the app

Open two terminals:

```bash
# Terminal 1 — API server
pnpm --filter @workspace/api-server run dev

# Terminal 2 — Frontend
pnpm --filter @workspace/slang-translator run dev
```

The API server starts on the port defined in `.env` (default `8080`). The Vite dev server will print its local URL.

---

## 🔌 API Reference

All routes are served under `/api` and defined in `lib/api-spec/openapi.yaml`.

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/healthz` | Health check |
| `POST` | `/translations` | Translate a slang term or phrase via OpenAI |
| `GET` | `/translations` | List translation history |
| `GET` | `/translations/:id` | Get a single translation |
| `DELETE` | `/translations/:id` | Delete a translation |

---

## 🛠️ Available Scripts

Run from the repo root:

| Command | Description |
|---|---|
| `pnpm install` | Install all workspace dependencies |
| `pnpm run build` | Typecheck and build all packages |
| `pnpm run typecheck` | Typecheck all packages |
| `pnpm --filter @workspace/db run push` | Push Drizzle schema to PostgreSQL |
| `pnpm --filter @workspace/api-server run dev` | Start the API server in watch mode |
| `pnpm --filter @workspace/slang-translator run dev` | Start the frontend dev server |
| `pnpm --filter @workspace/api-spec run codegen` | Regenerate API hooks & Zod schemas from the OpenAPI spec |

---

## 🔑 Environment Variables

| Variable | Description |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string |
| `AI_INTEGRATIONS_OPENAI_API_KEY` | Your OpenAI API key |
| `AI_INTEGRATIONS_OPENAI_BASE_URL` | OpenAI base URL (default: `https://api.openai.com/v1`) |
| `PORT` | Port for the Express API server (default: `8080`) |

---

## 🤝 Contributing

Contributions are welcome! Fork the repo, create a feature branch, and open a pull request. For non-trivial changes, please open an issue first to discuss what you'd like to change.

```bash
git checkout -b feature/your-feature-name
git commit -m "feat: your feature description"
git push origin feature/your-feature-name
```

---

## 📄 License

Licensed under the [MIT License](./LICENSE).
>>>>>>> 442ec6fdf3d65d846472567268b55c81b41e3880
