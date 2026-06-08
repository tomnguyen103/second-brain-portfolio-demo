# Second Brain Portfolio Demo

This repository is the public, static Netlify demo for Second Brain. It is intentionally separate from the main `second-brain` development repository so the portfolio demo can be deployed without touching the local-first app workspace.

## What This Repo Contains

- A static-exportable Next.js frontend in `frontend/`
- Public-safe fixture data only
- A browser-side read-only API adapter for chat, search, sources, status, feedback, tasks, research, and briefings
- Netlify build settings in `netlify.toml`
- Optional casual passcode gating through `NEXT_PUBLIC_DEMO_ACCESS_HASH`

It does not contain the backend, database, Redis, private notes, Gemini keys, API tokens, admin tokens, or local runtime state.

## Netlify Setup

Connect Netlify to this repository and keep the committed `netlify.toml` settings:

```toml
[build]
base = "frontend"
command = "npm ci && npm run build"
publish = "out"
```

Optional access gate:

```text
NEXT_PUBLIC_DEMO_ACCESS_HASH=<lowercase sha256 hash of your demo passcode>
```

Omit `NEXT_PUBLIC_DEMO_ACCESS_HASH` if the portfolio demo should be open to everyone.

## Local Verification

```powershell
cd frontend
npm ci
npm run lint
$env:NEXT_PUBLIC_SECOND_BRAIN_DEMO_MODE="static"
$env:NEXT_PUBLIC_AGENTIC_RAG_ENABLED="true"
npm run build
python -m http.server 4173 --directory out
```

Open `http://localhost:4173/chat/` and verify the static demo answers public-safe prompts with citations.

## Main Project

The full local-first Second Brain project lives separately at:

https://github.com/tomnguyen103/second-brain

