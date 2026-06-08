# Second Brain Portfolio Demo

This repository is the public, zero-cost Netlify static demo for
[Second Brain](https://github.com/tomnguyen103/second-brain). It exists so
portfolio visitors can click through the product experience without deploying
the private local-first runtime.

![Second Brain static portfolio demo chat screen](docs/assets/second-brain-portfolio-demo-chat.png)

## Important Note

This repo is only the static portfolio demo. The real application logic,
backend architecture, local-first setup, MCP tools, eval workflow, worker,
database, and operations documentation live in the main repository:

https://github.com/tomnguyen103/second-brain

Use this repo to review the deployable public frontend experience. Use the main
`second-brain` repo to review how the full system is built and operated.

## What This Demo Shows

- Clickable Second Brain web shell with chat, search, sources, status,
  feedback, tasks, research, briefing, and admin surfaces.
- Public-safe fixture corpus for regular RAG and Agentic RAG prompts.
- Deterministic cited answers with citation markers and source snippets.
- Static search over the public demo corpus.
- Read-only behavior for writes such as capture, ingest, task creation,
  source edits, research enqueue, eval promotion, deletion, and retention purge.
- A demo context notice that tells viewers this is a static portfolio preview
  and links to the full architecture repo.
- Optional casual passcode gate through `NEXT_PUBLIC_DEMO_ACCESS_HASH`.

## What This Repo Does Not Contain

This repository intentionally does not include:

- FastAPI backend services
- PostgreSQL, pgvector, Redis, or worker runtime
- Gemini, Ollama, or embedding API calls
- Private notes, raw transcripts, uploaded files, or local runtime state
- Admin tokens, API tokens, database URLs, `.env` files, or secrets
- Durable write or ingest workflows

Only public-safe fixture data belongs in this static demo.

## Project Shape

```text
frontend/                         Static-exportable Next.js app
frontend/lib/demo/public-demo-data.ts
                                  Public-safe fixture corpus
frontend/lib/api/demo-client.ts    Browser-side read-only API adapter
frontend/lib/demo/static-search.ts Static search and citation helpers
docs/portfolio-demo-netlify.md     Netlify runbook
docs/assets/                       README screenshots
netlify.toml                       Static Netlify build settings
```

## Netlify Setup

Connect Netlify to this repository and keep the committed `netlify.toml`
settings:

```toml
[build]
base = "frontend"
command = "npm ci && npm run build"
publish = "out"
```

The static demo build environment is already declared in `netlify.toml`:

```text
NEXT_PUBLIC_SECOND_BRAIN_DEMO_MODE=static
NEXT_PUBLIC_AGENTIC_RAG_ENABLED=true
NEXT_TELEMETRY_DISABLED=1
```

Optional casual access gate:

```text
NEXT_PUBLIC_DEMO_ACCESS_HASH=<lowercase sha256 hash of your demo passcode>
```

`NEXT_PUBLIC_DEMO_ACCESS_HASH` is shipped to the browser because this is a
static site. It is useful for casual access control, but it is not a security
boundary.

Do not add backend secrets or runtime variables to Netlify for this repo:

```text
DATABASE_URL
SECOND_BRAIN_API_TOKEN
SECOND_BRAIN_ADMIN_TOKEN
GEMINI_API_KEY
SECOND_BRAIN_TEST_DATABASE_URL
```

## Local Verification

From `frontend/`:

```powershell
npm ci
npm run lint
$env:NEXT_PUBLIC_SECOND_BRAIN_DEMO_MODE="static"
$env:NEXT_PUBLIC_AGENTIC_RAG_ENABLED="true"
npm run build
python -m http.server 4173 --directory out --bind 127.0.0.1
```

Open:

```text
http://127.0.0.1:4173/chat/
```

Check that:

- `/chat/` shows the demo context notice and answers public-safe prompts with
  citations.
- The Agentic RAG toggle updates the header state and returns static agentic
  retrieval metadata when enabled.
- `/search/` returns fixture corpus hits.
- `/sources/` shows the public demo source and documents.
- `/status/` reports `static-demo` runtime and no MCP mutations.
- Write actions show read-only behavior instead of calling a backend.

## Repository Split

| Repository | Purpose |
| --- | --- |
| `second-brain-portfolio-demo` | Static Netlify demo for portfolio viewers |
| [`second-brain`](https://github.com/tomnguyen103/second-brain) | Full local-first app, backend, MCP server, evals, data model, runtime setup, and operations docs |

