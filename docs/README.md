# OpenScreen 文件索引

OpenScreen — 繁體中文 AI 影像/影片生成平台（SaaS + Self-hosted）

> 此專案為 ST monorepo 的一部分。實際代碼位於 `/Users/fishtv/Development/ST/apps/openscreen`
> 完整產品討論紀錄：`~/.claude/memory/openscreen-discussion.md`

## 頂層文件

| 文件 | 用途 |
|------|------|
| `../CLAUDE.md` | 專案 Spectra 指引和架構說明 |
| `../AGENTS.md` | Agent 工作入口、路由表、命令速查 |
| `PRD.md`（待建） | 產品需求文件（功能清單、頁面流程、API 規格） |
| `API-DESIGN.md`（待建） | AI Gateway、Credits、Billing API 詳細設計 |

## Agent 文件（領域指南）

Agent 工作前應按 `AGENTS.md` 的 Task Routing 表讀對應文件。

| 領域 | 文件 | 覆蓋 |
|------|------|------|
| Studio（圖片、影片、唇形同步、增強） | `agents/domains/studio.md` | 模型呼叫流程、提示詞模板、錯誤處理 |
| 帳款與點數系統 | `agents/domains/billing.md` | 訂閱方案、Stripe webhook、點數扣款邏輯 |
| 圖庫與媒體管理 | `agents/domains/gallery.md` | 上傳、分享、隱私層級、下載 |
| 身份驗證與設定 | `agents/domains/auth.md` | Supabase Auth、User Profile、API Key 管理 |
| 工作流編輯器 | `agents/domains/workflow.md` | @xyflow/react 整合、節點類型、執行引擎 |
| AI Gateway 與供應商路由 | `agents/domains/ai-gateway.md` | enhancor.ai / fal.ai 路由、Credit tracking、Failover 邏輯 |
| 多語言支援 | `agents/domains/i18n.md` | next-intl 詞典、翻譯流程、繁體中文約定 |
| Electron 桌面版 | `agents/domains/electron.md` | Self-hosted 部署、本機推理、授權模式 |

## 工作流程文件

| 工作流 | 文件 | 說明 |
|--------|------|------|
| 測試 | `agents/workflows/testing.md` | Vitest 設定、Playwright E2E、Coverage 目標 |
| 提交與 PR | `agents/workflows/commits.md` | Conventional Commits 規則、PR Review 清單 |
| 部署 | `agents/workflows/deployment.md` | Vercel 設定、環境變數、Production 核對清單 |
| Spectra SDD | `agents/spectra.md` | 規格文件位置、Propose/Apply/Archive 流程 |

## 架構參考

| 主題 | 文件 | 說明 |
|------|------|------|
| 整體架構 | `architecture.md`（待建） | Monorepo 結構、App 邊界、Shared Packages 依賴 |
| 點數系統設計 | `credits-system.md`（待建） | 資料模型、計費邏輯、RPC 函數 |
| 授權機制（Self-hosted） | `licensing.md`（待建） | 本機 vs 雲端授權、Electron 簽署、啟用流程 |

## 子資料夾

| 資料夾 | 內容 | 用途 |
|--------|------|------|
| `agents/` | Agent 工作指南（domains/ 和 workflows/） | 開發前讀取相關指南 |
| `decisions/` | ADR（Architecture Decision Records） | 設計決策與理由 |
| `runbooks/` | 操作手冊（部署、故障排除、客戶支援） | 線上操作與緊急應對 |
| `discovery/`（待填） | 需求探索、客戶訪談、競品分析 | 產品研究與對標 |

## 快速開始

### 開發者入門

1. 讀 `../AGENTS.md`（本檔案的上層目錄）
2. 確認工作涉及的領域（Task Routing 表）
3. 讀對應的 `agents/domains/<domain>.md`
4. 開始編碼；遇到不確定就再讀相關文件或 Spectra spec

### 維護原則

- **新增文件**前問：「這屬於哪個領域或工作流？」，放在 `agents/` 或對應子資料夾
- **設計決策**記在 `decisions/` 並用 ADR 格式（Date / Context / Decision / Consequences）
- **Spectra Change 期間**補充的決策、API 設計等放 `decisions/`；Change 存檔後沒改動就不用再複製到其他地方
- **文件隔離**：OpenScreen 文件在這裡；ST monorepo 或其他產品的文件各自歸屬

## 版本與狀態

| 項目 | 狀態 | 最後更新 |
|------|------|---------|
| 產品方向 | ✅ 已確認 | 2026-05-12 |
| 技術棧 | ✅ 已確認 | 2026-05-12 |
| 架構設計 | 🔄 規劃中（Spectra propose 待執行） | — |
| AGENTS.md | ✅ 已建 | 2026-05-12 |
| 領域文件 | 📝 待建（隨開發逐步補充） | — |
| 工作流指南 | 📝 待建（隨開發逐步補充） | — |

## 常見問題

**Q: 代碼在哪？**  
A: `/Users/fishtv/Development/ST/apps/openscreen`（ST monorepo 內）；此 `/Users/fishtv/Development/products/openscreen` 目錄只放文件和配置。

**Q: 有沒有現成的元件、Auth、Billing 可用？**  
A: 有。來自 ST monorepo 的 shared packages：
- `@packages/saas-core`: Supabase Auth + Stripe Billing + 點數管理（AIRE、FlowGo 也在用）
- `@packages/ai-gateway`: AI 供應商路由、Credit tracking
- `@fishkit/ui`: 共用 UI 元件庫

讀 `agents/domains/<domain>.md` 時會有詳細整合指南。

**Q: Spectra Change 要走到什麼程度才能開始寫碼？**  
A: 提案（propose）完成、analyze 檢查通過、架構圖確認後進入 apply。詳見 `AGENTS.md` 的 Spectra 說明。

**Q: 自帶 API Key 是什麼意思？**  
A: 用戶在 Settings 頁填入自己的 enhancor.ai 或 fal.ai API Key，使用 AI 功能時系統轉發到用戶的 Key（不消耗平台點數）。

**Q: Self-hosted 版怎麼授權？**  
A: 本機模型（SD/SDXL/Wan2GP）免費用；要用線上進階模型（SORA、VEO 等）需要：(a) 買平台訂閱 + 點數，或 (b) 自帶 API Key。詳見 `agents/domains/electron.md`（待建）。

## 聯繫與流程

- **產品決策**: 見 `~/.claude/memory/openscreen-discussion.md`
- **Bug / Feature**: 建 Spectra Change （propose → apply → archive）
- **文件更新**: commit 到 openscreen repo；PR review 前確認無語法錯誤
