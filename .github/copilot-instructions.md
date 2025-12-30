## Purpose

This file tells AI coding agents how to be immediately productive in this repository: a Vite + React TypeScript app that integrates Google GenAI (Gemini) and uses Tailwind styles.

## Big picture

- Frontend: React + TypeScript built with Vite. Primary UI components live in the `components/` folder and pages in `app/`.
- Services: API wrappers and AI integration are under `services/` (see [services/geminiService.ts](services/geminiService.ts)).
- Public assets: `public/` and `assets/images` are used for static imagery; many components reference external image URLs.
- Note: `package.json` contains a `next` dependency but the project runs with Vite (`npm run dev`). Follow `vite` scripts in `package.json`.

## Developer workflows (explicit)

- Install: `npm install`
- Run locally: `npm run dev` (starts Vite on port 3000 as configured in [vite.config.ts](vite.config.ts)).
- Build: `npm run build`; Preview: `npm run preview`.
- Env: Set `GEMINI_API_KEY` in `.env.local` (Vite maps it to `process.env.API_KEY` in `vite.config.ts`). The `services/geminiService.ts` expects `process.env.API_KEY`.

## Project-specific patterns & conventions

- File types: `.tsx` React functional components with default exports (e.g., `components/PandaAdsHome.tsx`).
- Styling: Tailwind CSS utility classes and large, reusable UI sections; prefer editing copy/props in component files rather than global CSS for rapid changes.
- Aliasing: `@` is aliased to the repo root in [vite.config.ts](vite.config.ts); use `@/path` when adding imports.
- AI integration: Use the single `GeminiService` instance exported from `services/geminiService.ts`. Methods of interest: `chat()`, `analyzeImage()`, `generateImage()`, and `getLiveSession()` — follow existing patterns for `generateContent` usage and response parsing.

## Integration points & external deps

- Google GenAI: `@google/genai` (see `services/geminiService.ts`) — models referenced include `gemini-3-flash-preview` and image models. Responses often include `candidates` and `parts`; image outputs appear in `inlineData`.
- Environment: `GEMINI_API_KEY` mapped to `process.env.API_KEY` in Vite config.

## When editing AI code

- Mirror patterns in `services/geminiService.ts`: initialize `GoogleGenAI` once, reuse the instance, and normalize returned data (check `response.candidates` and `parts`).
- For images, return base64 with `data:image/png;base64,` prefix after scanning `parts` for `inlineData`.

## Files to look at first (examples)

- [package.json](package.json) — scripts and deps.
- [vite.config.ts](vite.config.ts) — env mapping and aliasing.
- [services/geminiService.ts](services/geminiService.ts) — AI integration patterns.
- [components/PandaAdsHome.tsx](components/PandaAdsHome.tsx) — representative component style and Tailwind usage.
- [README.md](README.md) — run instructions and notes about `.env.local`.

## What NOT to change without confirmation

- Global build tooling: do not switch from Vite to Next.js — repo currently runs with Vite.
- API key handling: keep `GEMINI_API_KEY` in `.env.local` and the Vite mapping to `process.env.API_KEY`.

## Quick examples

- Import root alias: `import X from '@/components/MyComponent'` (alias set in `vite.config.ts`).
- Call gemini chat: `import { gemini } from '@/services/geminiService'; await gemini.chat('hello')`.

If anything here is unclear or you want additional rules (linting, test commands, or CI conventions), tell me which area to expand.
