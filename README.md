# Prooftext

> *prooftext (n.)* — a passage quoted as evidence for a claim.

An AI research assistant that lets analysts ask plain-English questions about a corpus of SEC filings and get **sourced, citable, grounded answers** — every claim links back to the exact filing and passage that proves it. If the corpus doesn't support an answer, it says so instead of guessing.

> 📚 **This is my learning build.** I'm rebuilding this project to learn full-stack AI engineering, following [Dave Ebbelaar's](https://github.com/daveebbelaar) Document Copilot tutorial. Original repo: [daveebbelaar/document-copilot](https://github.com/daveebbelaar/document-copilot). Commits here are my own daily progress.

## The problem it solves

The client (fictional **Driftwood Capital**, an equity-research firm) has ~40 analysts who each spend **half their week** just reading 10-Ks and 10-Qs — scanning for risk factors, MD&A, and segment data, comparing year-over-year — before they can produce any original analysis. Prooftext eats that intake work so they jump straight to insight.

Because this is a research firm, **trust is non-negotiable**:

- **Never invent facts** — if it's not in the corpus, the bot says so.
- **Always cite** — every claim links to the source filing + page.
- **Show the passage** — the analyst can verify the answer in one click.

Full brief: [docs/client-brief.md](docs/client-brief.md) · Architecture: [docs/architecture.md](docs/architecture.md)

## Stack

| Layer            | Choice                                                  |
| ---------------- | ------------------------------------------------------- |
| Backend          | Python + FastAPI + PydanticAI (typed LLM agent)         |
| Frontend         | Vite + React SPA + TypeScript                           |
| Database         | Supabase Postgres (users, chats, documents, chunks)     |
| Migrations       | SQLAlchemy models + Alembic                             |
| Retrieval        | Supabase `pgvector` (semantic) + Postgres full-text + RRF fusion |
| Auth             | Supabase Auth (email only)                              |
| Hosting          | Railway                                                 |
| LLM + embeddings | OpenAI                                                  |

## Repo layout

```text
prooftext/
├── AGENTS.md           # coding-agent instructions (read first)
├── README.md           # this file
├── data/               # local corpus + SEC EDGAR download script (payloads gitignored)
├── docs/
│   ├── client-brief.md # the client one-pager
│   ├── architecture.md # target architecture
│   └── guides/         # Supabase / backend / frontend setup
├── backend/            # FastAPI service
└── frontend/           # React SPA (Vite)
```

## Status

🚧 **Learning in progress.** Following the 13-step implementation sequence in [docs/architecture.md](docs/architecture.md):

- [x] Repo scaffolding + docs + SEC data downloader
- [ ] Backend FastAPI app + config
- [ ] SQLAlchemy models + Alembic migrations (pgvector, documents, chunks, chats, citations)
- [ ] Supabase Auth + JWT verification
- [ ] Chat streaming endpoint
- [ ] React chat UI
- [ ] Markdown ingestion → chunking → embeddings
- [ ] Hybrid retrieval (pgvector + full-text + RRF)
- [ ] PydanticAI document agent
- [ ] Citation validation + grounding enforcement

## Learning log

Daily notes on what I built and learned:

| Date | What I learned |
| ---- | -------------- |
| _2026-06-18_ | Set up my own repo (renamed to Prooftext); learned SSH vs PAT and how to run two GitHub accounts on one machine with a `github-personal` SSH alias; understood the architecture & PydanticAI grounding contract |

## Running locally

Setup guides (work through these as the build progresses):

- [Supabase](docs/guides/supabase-setup.md) — account + hosted project
- [Backend](docs/guides/backend-setup.md)
- [Frontend](docs/guides/frontend-setup.md)

## Sample SEC data

Fetch a small local 10-K sample from SEC EDGAR. Edit the params at the top of `data/download.py` (especially `USER_AGENT`), then run:

```bash
uv run data/download.py
```

By default this downloads the latest 5 10-K filings for AAPL, MSFT, NVDA, AMZN, and GOOGL into year folders under `data/downloads/`. Downloaded files are gitignored.

## Credit

Built by following [Dave Ebbelaar](https://github.com/daveebbelaar)'s Document Copilot tutorial. All credit for the original design goes to him; this repo is my hands-on learning version.
