# NGO Command — AI Decision & Action System

## Overview

A full-stack AI-powered decision support tool for Indian NGO coordinators. Transforms messy community data (WhatsApp messages, field notes, informal reports) into structured needs, prioritized action plans, and intelligent volunteer assignments.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **Frontend**: React + Vite (artifacts/ngo-decision-tool)
- **API framework**: Express 5 (artifacts/api-server)
- **Database**: PostgreSQL + Drizzle ORM
- **AI**: Google Gemini 2.5 Flash (via Replit AI Integrations)
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)

## Key Features

1. **AI Data Extraction** — Paste messy field notes/WhatsApp messages, get structured extracted needs
2. **Before vs After** — Side-by-side view of raw input and structured AI output
3. **Prioritization Engine** — Priority Score = urgency_weight + log(people affected)
4. **Action Plans** — Specific, actionable NGO instructions (who, what, where, when)
5. **Explainable AI** — "Why this priority?" + "Risk if delayed" for each need
6. **Impact Preview** — Shows people helped, resolution time, severity reduction
7. **Volunteer Matching** — Location-based matching (same city > same state > far)
8. **Report History** — Save, view, and manage all past analyses
9. **Dashboard** — Stats overview: total reports, volunteers, high-urgency count, people helped

## India-Specific Data

All demo scenarios are India-specific:
- Flood in Rohtak, Haryana
- Food shortage in Patna, Bihar
- School disruption in Delhi NCR
- Heatwave in Jaipur, Rajasthan

Volunteers pre-seeded across Rohtak (Haryana), Patna (Bihar), New Delhi (Delhi), Jaipur (Rajasthan).

## Architecture

```
Frontend (React+Vite) → API Server (Express) → PostgreSQL DB
                              ↓
                    Gemini 2.5 Flash (AI extraction)
```

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally

## Important Notes

- After codegen, manually fix `lib/api-zod/src/index.ts` to only export `./generated/api` (not `./generated/types`) to avoid duplicate export errors
- Gemini integration uses `AI_INTEGRATIONS_GEMINI_BASE_URL` and `AI_INTEGRATIONS_GEMINI_API_KEY` env vars (auto-provisioned)
- `@google/*` was removed from esbuild externals so `@google/genai` gets bundled correctly
