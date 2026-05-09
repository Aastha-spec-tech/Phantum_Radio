# RadioWorld — CLAUDE.md

## Project overview
A global radio explorer with a 3D Three.js globe. Users spin the globe, click 
stations, and listen live. Six core features: globe explorer, AI live translation, 
time travel (Internet Archive), social listening rooms, ambient desktop widget, 
and mood-based station matching.

## Repo structure
radioworld/
  apps/
    web/          # React + Vite + Three.js frontend
    api/          # Node.js + Express + Socket.io backend
    ai-service/   # Python FastAPI — Whisper + Claude proxy
    extension/    # Chrome/Firefox Manifest V3 extension
    desktop/      # Electron or Tauri ambient widget
  packages/
    shared/       # Types, constants, API contracts (TypeScript)
  CLAUDE.md

## Critical constraints
- Globe must load in <3s — use instanced meshes, LOD, lazy station dots
- Audio streams: always proxy through our backend (CORS + rate limiting)
- Never expose Claude API key or Radio Browser API key to the frontend
- Whisper runs server-side — never in the browser
- Internet Archive requests must be cached aggressively (60min TTL)
- Social rooms: use Socket.io rooms, not polling
- All AI calls go through apps/ai-service, never direct from frontend

## Code style
- TypeScript everywhere (strict mode)
- React functional components + hooks only
- No class components
- Tailwind for styling, no CSS modules
- Zod for all API request/response validation
- Prettier + ESLint enforced

## Key external APIs
- Radio Browser API: https://api.radio-browser.info (no key needed)
- Internet Archive: https://archive.org/advancedsearch.php
- Claude API: via ANTHROPIC_API_KEY env var
- Whisper: self-hosted via faster-whisper (Python)

## Environment variables
See .env.example in each app directory. Never commit .env files.

## When adding a new feature
1. Define the API contract in packages/shared/src/types/
2. Build the backend route first (with Zod validation)
3. Then build the frontend component
4. Write at least one integration test per new route
