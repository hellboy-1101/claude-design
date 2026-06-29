# claude-design

AIネイティブ・デザインシステム。**DESIGN.md ハーネス 3 層モデル**で、AI が generic SaaS 顔へ流れるのを構造的に抑制する。

- Layer 1: `CLAUDE.md` + `design/DESIGN.md`（憲法・Quick/Standard/Full 読込モード）
- Layer 2: `packages/contracts/*.json`（DTCG tokens + rules + components の SSOT）
- Layer 3: `packages/harness/`（TS schema 検証・drift 検出・a11y・CI）

## 不変ルール (Quick・常時適用)

- SSOT は `packages/tokens/*.json`（DTCG 2025.10）のみ。第二の正本（手書き CSS 変数等）を作らない
- 色は **OKLCH 必須**（modern browsers のみ対応 — Chrome / Edge / Firefox / Safari の最新 2 バージョン。IE11 / 旧 Safari / 旧 Android は対象外）
- `design/DESIGN.md` および derived contracts は **手編集禁止**。必ず source を直して再生成する
- 再利用前提コンポは **`packages/ui` 必須**、apps/web 固有の組み合わせ（page / layout / route）は `apps/web/src/` で OK
- package manager は **`pnpm@11.9.0` 固定**（npm / bun 禁止）。Node.js v22 以上が必須

## DS 詳細（読込モード）

@design/CLAUDE.md

## リポジトリ構造

```
claude-design/
├── apps/web/                 # Showcase / playground (Vite + React 19)
├── packages/
│   ├── ui/                   # DS 本体・publishable component 実装
│   ├── tokens/               # DTCG JSON SSOT
│   ├── contracts/            # rules.json + components/*.json
│   └── harness/              # TS validation / drift-check / export-designmd
├── design/
│   ├── DESIGN.md             # Phase 1 で auto-generate (手編集禁止)
│   ├── CLAUDE.md             # DS 読込規約 (Quick/Standard/Full)
│   └── README.md
├── docs/
│   ├── research/             # Grok 調査正本 (historical record, 変更不可)
│   └── review/               # 設計レビュー記録 (Codex 含む)
└── CLAUDE.md                 # ← このファイル (Claude Code 起動時自動読込起点)
```

## ドキュメント目次

- [docs/research/2026-06-29-grok-claude-design-research.md](docs/research/2026-06-29-grok-claude-design-research.md) — 2026-06 時点の AI ネイティブ DS 業界動向（Grok WebSearch）
- [docs/review/2026-06-29-codex-review-request-phase0.md](docs/review/2026-06-29-codex-review-request-phase0.md) — Phase 0 設計レビュー（v1→v3.1、Codex 3 回レビュー）

## 共通コマンド

```bash
pnpm install            # workspace 全体の依存解決
pnpm dev                # apps/web の開発サーバ
pnpm build              # 全 package のビルド (turbo)
pnpm typecheck          # 型チェック
pnpm lint               # ESLint
```

## Phase 進行

- Phase 0（基盤）: 進行中 — [docs/review/](docs/review/) 参照
- Phase 1（tokens 正本 + DESIGN.md auto-generate）: 未着手
- Phase 2（components 実装）: 未着手
- Phase 3（harness + CI + a11y）: 未着手
- Phase 4（showcase）: 未着手
