# OpenScreen

> 繁體中文 AI 影像/影片生成平台（SaaS + Self-hosted），基於 open-generative-ai + ST monorepo

## GitHub

https://github.com/fishtvlvoe/openscreen

## Tech Stack

- Next.js 16 + React 19 + TypeScript
- Tailwind CSS 4 + @fishkit/ui
- Supabase Auth + PostgreSQL
- Stripe（訂閱 + 點數加值）
- next-intl（i18n，預設繁體中文）
- @xyflow/react（Workflow Studio）
- Zustand（狀態管理）
- Electron（Self-hosted 桌面版）

## AI 供應商

- 主力：enhancor.ai（$299/月 Enterprise，360K credits）
- 備援：fal.ai（按次計費）
- 路由層：@packages/ai-gateway

## 架構

ST monorepo 內的 app，實際代碼位於 `/Users/fishtv/Development/ST/apps/openscreen`
共用 packages：@packages/saas-core（Auth + Billing + Credits）、@packages/ai-gateway

## 產品討論摘要

完整決策紀錄：`/Users/fishtv/Development/ST/.claude/memory/openscreen-discussion.md`
