<!-- SPECTRA:START v1.0.2 -->

# Spectra Instructions

This project uses Spectra for Spec-Driven Development(SDD). Specs live in `openspec/specs/`, change proposals in `openspec/changes/`.

## Use `/spectra-*` skills when:

- A discussion needs structure before coding → `/spectra-discuss`
- User wants to plan, propose, or design a change → `/spectra-propose`
- Tasks are ready to implement → `/spectra-apply`
- There's an in-progress change to continue → `/spectra-ingest`
- User asks about specs or how something works → `/spectra-ask`
- Implementation is done → `/spectra-archive`
- Commit only files related to a specific change → `/spectra-commit`

## Workflow

discuss? → propose → apply ⇄ ingest → archive

- `discuss` is optional — skip if requirements are clear
- Requirements change mid-work? `ingest` → resume `apply`

## Parked Changes

Changes can be parked（暫存）— temporarily moved out of `openspec/changes/`. Parked changes won't appear in `spectra list` but can be found with `spectra list --parked`. To restore: `spectra unpark <name>`. The `/spectra-apply` and `/spectra-ingest` skills handle parked changes automatically.

<!-- SPECTRA:END -->

# AGENTS.md - OpenScreen Agent Entry

This is the always-read entrypoint for GenAI agents working in `OpenScreen`. Keep it short. Load detailed guidance only when the current task needs it.

## Project Purpose

`OpenScreen` is a **繁體中文 AI 影像/影片生成平台** with dual deployment models: SaaS (cloud) + Self-hosted (desktop/local).

Core product capabilities:

- Multi-modal AI generation: Image, Video, Lipsync, Enhancement (Upscale, Restore, Denoising)
- Workflow Studio: Node-based AI job orchestration (planned Phase 2)
- Dual AI backend: enhancor.ai (primary, $299/month 360K credits) + fal.ai (fallback, pay-per-use)
- Credits system: Subscription tiers + à la carte top-ups for advanced models
- API Key passthrough: Users bring their own credentials to avoid consuming platform credits
- Self-hosted licensing: Free local inference (SD/SDXL/Wan2GP) + optional cloud model access

Target market: Taiwan (繁體中文) with future expansion to broader Chinese-speaking regions.

## Non-Negotiable Rules

1. Always route AI requests through `@packages/ai-gateway` (never direct API calls to enhancor/fal).
2. Track enhancor.ai credit usage in real-time; auto-failover to fal.ai when credits < 5% remaining.
3. Never expose API keys in logs, error messages, or Sentry.
4. Validate payment webhooks (Stripe) with signatures before accepting billing state changes.
5. All UI copy, error messages, and toast notifications must be in 繁體中文 (use `next-intl` dictionary).
6. For self-hosted Electron version: Bundle only the local inference engines (sd.cpp, Wan2GP). Cloud models require valid subscription or API key.
7. Gallery and media operations must respect user's privacy tier (private/shared/public).
8. Spectra Change: Propose before coding new workflows, domains, or major architecture changes.

## Progressive Loading Policy

Before coding:

1. Read this file.
2. Identify the affected domain from the task routing table.
3. Read only the matching file under `docs/agents/domains/` (if it exists).
4. Read workflow files under `docs/agents/workflows/` only when the task touches that workflow.
5. Read `docs/agents/spectra.md` when planning, applying, ingesting, archiving, or asking about Spectra changes/specs.
6. Do not bulk-load all specs. Open only the specific `openspec/specs/<name>/spec.md` or active change files needed for the task.
7. For AI Gateway and credit system decisions, check `docs/ai-gateway.md` and `docs/credits-system.md` first.

## Task Routing

| If task mentions | Read next |
|---|---|
| image, video, lipsync, enhance, generation, studio, 影像, 影片, 唇形同步, 增強 | `docs/agents/domains/studio.md` |
| credits, billing, subscription, points, tier, payment, 點數, 帳款, 訂閱 | `docs/agents/domains/billing.md` |
| gallery, media, library, upload, download, sharing, 圖庫, 素材, 分享 | `docs/agents/domains/gallery.md` |
| auth, user, profile, settings, account, dashboard, 登入, 帳戶, 設定 | `docs/agents/domains/auth.md` |
| workflow, node, editor, orchestration, flow, 工作流, 節點, 編輯器 | `docs/agents/domains/workflow.md` |
| enhancor, fal, AI provider, gateway, routing, failover, model, 供應商, 路由 | `docs/agents/domains/ai-gateway.md` |
| i18n, translation, locale, language, intl, 多語言, 翻譯 | `docs/agents/domains/i18n.md` |
| Electron, self-hosted, desktop, local, offline, 桌面版, 本地 | `docs/agents/domains/electron.md` |
| tests, coverage, Vitest, Playwright, 測試 | `docs/agents/workflows/testing.md` |
| commit, PR, review, release, 提交, 版本 | `docs/agents/workflows/commits.md` |
| deploy, production, Vercel, Docker, 部署 | `docs/agents/workflows/deployment.md` |
| Spectra, openspec, spec, proposal, change, 規格 | `docs/agents/spectra.md` |

## Tech Baseline

- **Monorepo**: Part of ST (Supastarter) monorepo; actual code in `/Users/fishtv/Development/ST/apps/openscreen`
- **Frontend**: Next.js 16 + React 19 + TypeScript 5.x
- **Styling**: Tailwind CSS 4.x + @fishkit/ui (shared component library)
- **State**: Zustand
- **UI Graph**: @xyflow/react for Workflow Studio
- **i18n**: next-intl (default: 繁體中文, future: expansion)
- **Auth**: Supabase Auth + JWT
- **Database**: PostgreSQL via Supabase
- **Billing**: Stripe (subscriptions + credits top-ups)
- **Icons**: Lucide React (no emoji per L019)
- **Shared Packages**:
  - `@packages/ai-gateway`: AI provider routing + credit tracking
  - `@packages/saas-core`: Auth + Billing + Credits (shared with AIRE, FlowGo)
- **Self-hosted (Electron)**: Electron (TBD) + local inference engines (sd.cpp, Wan2GP)

Confirm exact versions from `ST/package.json` and `apps/openscreen/package.json` before version-sensitive work.

## Common Commands

```bash
# From ST/apps/openscreen:
npm run dev          # Start Next.js dev server
npm run build        # Build Next.js app
npm run lint         # Run ESLint + TypeScript check
npm run test         # Run Vitest
npm run preview      # Preview production build locally

# Monorepo (from ST root):
npm run --workspace=openscreen dev    # Develop openscreen in monorepo context
npm run --workspace=openscreen build
npm run --workspace=openscreen test
```

## Spectra & SDD Status

- **Phase**: Initial (no active changes yet)
- **Config**: Check `openspec/config.yaml` before starting any Spectra workflow
- **Latest**: See `openspec/changes/` for any in-progress proposals or tasks

## Key Decisions & References

Complete decision log: `/Users/fishtv/Development/ST/.claude/memory/openscreen-discussion.md`

Key architectural decisions:
- ✅ AI Provider: enhancor.ai (primary, $299/month) + fal.ai (fallover, pay-per-use)
- ✅ Credits system: Subscription tiers (Basic/Creator/Professional/Enterprise) + point-based usage
- ✅ Self-hosted: Free local models + optional cloud (requires subscription or user's own API key)
- ✅ i18n: Single-language at launch (繁體中文), pre-architected for multi-language expansion

## Communication Norms

- Task routing table is source of truth; if you're unsure, ask here first.
- Spectra Change for any feature touching multiple domains or architectural decisions.
- All commit messages: conventional commits (e.g., `feat(studio): add lipsync endpoint`)
- Chinese user-facing text; English for code comments (unless context is all-Chinese).
- Before deployment to production: verify Stripe webhook and enhancor.ai credit checks.
