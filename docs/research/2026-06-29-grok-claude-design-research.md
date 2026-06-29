2026年6月時点の「AIネイティブ・デザインシステム」動向を、X と web 検索で横断調査します。まず複数キーワードで並行検索し、出典付きで整理します。
# 「claude design」調査レポート — 2026年6月時点

Web search と GitHub/npm のライブ確認を主根拠に整理しました。重い個別ページの全文取得は避け、検索スニペット・README API・npm registry を優先しています。

---

## 1. 「デザインシステムをハーネス化する」提唱者・実践事例

2026年時点で、この思想は **「DESIGN.md（人間可読の憲法）＋ machine-readable contracts（JSON/YAML）＋ CI harness（検証）」** の3層として収束しつつあります。デザイナーはコンポーネント1個のレビューから、**契約・ルール・トークンの育成**へシフトする、という主張が複数独立に現れています。

### 日本語圏

| 主体 | 内容 | 出典 |
|------|------|------|
| **@tsubotax** | **melta UI** — `DESIGN.md` + `design/contracts/`（tokens.json, rules.json, components/）+ TypeScript harness + CI + MCP。README に「Built for AI coding agents」「人間にも、AIにも、読める」と明記。`DESIGN.md` は Google spec から **tokens.json を自動 export**（手編集禁止） | [github.com/tsubotax/melta-ui](https://github.com/tsubotax/melta-ui), [melta.tsubotax.com](https://melta.tsubotax.com/), [zenn.dev/tsubotax](https://zenn.dev/tsubotax/articles/7f0d3693f70e2f), [@tsubotax X](https://x.com/tsubotax/status/2070069911322611746) |

### 英語圏・グローバル

| 主体 | 内容 | 出典 |
|------|------|------|
| **Google Labs** | **design.md** 仕様（YAML front matter + Markdown prose）。**Stitch** と連携し、視覚的アイデンティティを coding agent 向けに記述 | [github.com/google-labs-code/design.md](https://github.com/google-labs-code/design.md) **v0.3.0**, [stitch.withgoogle.com/docs/design-md](https://stitch.withgoogle.com/docs/design-md/overview/), [blog.google — Stitch design.md](https://blog.google/innovation-and-ai/models-and-research/google-labs/stitch-design-md/) |
| **@pbakaus — Impeccable** | Claude の `frontend-design` skill を発展。**`/impeccable init`** で `PRODUCT.md` / `DESIGN.md` を生成。**44 deterministic detector rules** + 23 commands（`polish`, `audit`, `distill` 等）。CLI **v3.1.0** | [impeccable.style](https://impeccable.style/), [github.com/pbakaus/impeccable](https://github.com/pbakaus/impeccable) |
| **VoltAgent — awesome-claude-design** | **68個の DESIGN.md** テンプレ集。Claude Design に drop して UI scaffold を一発生成 | [github.com/VoltAgent/awesome-claude-design](https://github.com/VoltAgent/awesome-claude-design), [getdesign.md](https://getdesign.md/) |
| **Anthropic — Claude Design** | 永続的 design system workspace（tokens, components, preview）。DESIGN.md / 既存 DS import、code round-trip | [claude.ai/design](https://claude.ai/design), [anthropic.com/news/claude-design](https://www.anthropic.com/news/claude-design-anthropic-labs), [support.claude.com — design system setup](https://support.claude.com/en/articles/14604397-set-up-your-design-system-in-claude-design) |
| **OpenAI — Harness Engineering** | agent が長時間自律動作するための **「ハーネス」** 概念を公式提唱。DS 文脈では contracts + validation に直結 | [openai.com/index/harness-engineering](https://openai.com/index/harness-engineering/) |
| **oh-my-design** (韓国) | `npx oh-my-design` で skill-driven design bootstrap。**356社の実 DS リファレンス**、ゼロ AI call install | [oh-my-design.kr](https://oh-my-design.kr/), [github.com/kwakseongjae/oh-my-design](https://github.com/kwakseongjae/oh-my-design), **oh-my-design-cli v1.8.7** |
| **Design Systems Collective** | 「DS を Claude skill にする」ワークフロー、DESIGN.md + Claude Code handoff 解説 | [designsystemscollective.com — Claude skill](https://www.designsystemscollective.com/design-systems-in-2026-turn-your-system-into-a-claude-skill-3dd4d8bf5feb), [design-md workflow](https://www.designsystemscollective.com/the-design-md-workflow-how-google-stitch-claude-code-quietly-changed-the-design-to-code-handoff-c4213f97ed8f) |
| **Supernova** | 2026 enterprise DS トレンドとして **AI agent 向け DS** を公式位置づけ | [supernova.io/blog/2026-trends](https://www.supernova.io/blog/the-future-of-enterprise-design-systems-2026-trends-and-tools-for-success), [supernova.io/for-ai](https://www.supernova.io/for-ai) |
| **実務ブログ群** | AI-readable tokens フレームワーク（4-step / 7-step）、Cursor 向け token 固定化、agent への DS 教育 | [medium.muz.li — 4-step framework](https://medium.muz.li/my-4-step-framework-to-make-design-systems-ai-readable-74ba07145312), [learn.thedesignsystem.guide](https://learn.thedesignsystem.guide/p/design-tokens-that-ai-can-actually), [medium.com/@sachin88 — Cursor tokens](https://medium.com/@sachin88/how-we-fixed-design-tokens-to-make-cursor-generate-reliable-ui-code-74d699e72e38), [nayamoss.com](https://nayamoss.com/how-i-use-ai/teach-ai-agents-design-system) |
| **Prisme AI — DSUL** | Design System Unified Language。agent 向け machine-readable DS 記述 | [docs.prisme.ai/dsul](https://docs.prisme.ai/dsul/overview) |
| **Figma MCP** | Claude Code ↔ Figma 双方向。design context を agent に注入 | [help.figma.com — Claude Code MCP](https://help.figma.com/hc/en-us/articles/39888612464151-Claude-Code-and-Figma-Set-up-the-MCP-server), [figma.com/blog/claude-code-to-figma](https://www.figma.com/blog/introducing-claude-code-to-figma/) |

### 概念の収束パターン

```
Layer 1: DESIGN.md / CLAUDE.md     ← AI が最初に読む「憲法」
Layer 2: contracts/*.json          ← SSOT（tokens, rules, components）
Layer 3: harness (TS) + CI         ← schema検証・drift検出・a11yテスト
```

@tsubotax の melta UI がこの構造を最も明示的に実装しており、Google design.md spec・Impeccable・OpenAI harness engineering と思想的に一致しています。

---

## 2. DESIGN.md ハーネスを中核に採用する価値 — 評価

### 採用すべき理由（2026年時点で実証済み）

1. **AI の「汎用 SaaS 顔」問題を構造的に抑制できる**  
   Impeccable は Inter + purple gradient + nested cards 等の **deterministic 検出** を提供。@pbakaus の実践は、DESIGN.md 単体より **rules + detectors** の併用が効くことを示す。

2. **非デザイナー PR の品質を「契約」で底上げできる**  
   melta UI は 99 tokens + 99 rules + 33 component contracts を CI で検証。デザイナーは個別 PR ではなく **rules.json の severity / detector** を育てる側に回れる。

3. **コンテキスト効率が良い**  
   melta UI の段階的読み込み（Quick = `DESIGN.md` のみ / Full = contracts）は、agent の token 消費を抑えつつ精度を保つ実装例。

4. **エコシステムが成熟した**  
   Google design.md v0.3.0、awesome-claude-design（68 templates）、getdesign.md、Anthropic Claude Design の import 対応により、**フォーマット互換**が現実的。

### 落とし穴

| 落とし穴 | 根拠 |
|----------|------|
| **DESIGN.md 手書きは必ず drift する** | melta UI は `DESIGN.md` を tokens から **自動生成・手編集禁止**。手動運用の Medium 記事群は drift リスクが高い |
| **DTCG 2025.10 はまだ全ツール対応ではない** | Style Dictionary 公式: 「2025.10 does not have full support yet in Style Dictionary — WIP in v5」 ([styledictionary.com/info/dtcg](https://styledictionary.com/info/dtcg/)) |
| **視覚ツールと code SSOT の二重管理** | Claude Design / Open Design は強力だが、code harness なしだと **実装とビジュアルが乖離**（VentureBeat が token-burning 問題も指摘: [venturebeat.com](https://venturebeat.com/technology/anthropic-ships-major-claude-design-overhaul-with-design-system-imports-code-round-trips-and-a-fix-for-its-token-burning-problem)） |
| **over-specification で context が膨張** | contracts を肥大化させると agent が読み切れない。melta の Quick/Full 分離が対策 |
| **harness なき DESIGN.md は「おまじない」** | Reddit / DSC コミュニティでも「DESIGN.md だけでは generic UI が止まらない」という声。Impeccable の detector が補完 |

### 実装のコツ（事例から抽出）

1. **SSOT は JSON（DTCG）、DESIGN.md は export 物** — melta UI パターンを踏襲  
2. **rules.json に ID + severity + detector を持たせる** — LLM 批判だけに頼らない  
3. **Impeccable の 44 rules か、同等の lint を CI に組み込む**  
4. **Claude skill 化** — DSC の「DS → Claude skill」記事、oh-my-design の bootstrap  
5. **MCP でトークン検索・ルール照会** — melta UI の MCP サーバー設計  
6. **段階的読み込みを CLAUDE.md に明記** — agent ごとの手順書を分離

**結論: 中核として採用する価値は高い。** ただし「DESIGN.md ファイル1枚」ではなく、**contracts + harness + skill の束**として中核に据えるべき。

---

## 3. 2026年の技術的土台 — ベストプラクティス（バージョン付き）

### W3C DTCG（Design Tokens Format）

| 項目 | 状態 |
|------|------|
| **最新安定版** | **DTCG Format Module 2025.10**（2025-10-28 初の安定版到達） |
| **成熟度** | Community Group 初の stable release。エコシステムは追従中だが、**2026年の新規 SSOT 形式として DTCG を選ぶのが正解** |
| **出典** | [w3.org/community/design-tokens/2025/10/28](https://www.w3.org/community/design-tokens/2025/10/28/design-tokens-specification-reaches-first-stable-version/), [designtokens.org/TR/2025.10/format](https://www.designtokens.org/TR/2025.10/format/) |

### トークン変換ツール

| ツール | 最新版（npm/GitHub 確認） | 2026推奨用途 |
|--------|--------------------------|--------------|
| **Style Dictionary** | **v5.5.0**（2026-06-21） | 多 platform 出力（CSS, iOS, Android, JSON）。DTCG first-class（v4+）。**2025.10 完全対応は v5 WIP** |
| **Terrazzo** | **@terrazzo/cli v2.4.0**, **@terrazzo/parser v2.4.0** | DTCG-native、**Tailwind v4 / CSS variables 出力に特化**。Web 単一スタックなら第一選択 |
| **Tokens Studio** | DTCG 2025.10 export 対応（ドキュメント記載） | Figma ↔ DTCG 編集 UI |

**推奨:** Web + Tailwind 中心なら **Terrazzo 2.4.0** を primary、multi-platform なら **Style Dictionary 5.5.0** を併用。

### OKLCH 採用

| 判断 | 根拠 |
|------|------|
| **新規 DS では OKLCH を正とする（推奨）** | Tailwind CSS v4 が bleeding-edge color space を前提。shadcn/ui 新規プロジェクトは **HSL → OKLCH 変換**を公式に実施 ([ui.shadcn.com/docs/tailwind-v4](https://ui.shadcn.com/docs/tailwind-v4)) |
| **既存 HSL 資産は段階移行** | shadcn は non-breaking。v3/React 18 既存アプリはそのまま動作 |
| **注意** | Tailwind v4 は modern browser 前提。レガシー IE 系は対象外 |

### Tailwind CSS v4

| 項目 | 値 |
|------|-----|
| **最新版** | **v4.3.1**（npm registry 確認） |
| **GA** | v4.0.0 — 2025-01-21 |
| **要点** | CSS-first `@theme`、native CSS variables、OKLCH、Vite/PostCSS 統合簡素化 |

### shadcn/ui

| 項目 | 値 |
|------|-----|
| **CLI 最新** | **shadcn@4.12.0**（2026-06-26） |
| **Tailwind v4** | フル対応。新規 init は v4 + React 19 がデフォルト |
| **変更点** | `data-slot` 属性、`new-york` style デフォルト、`toast` → `sonner`、OKLCH colors |
| **出典** | [ui.shadcn.com/docs/tailwind-v4](https://ui.shadcn.com/docs/tailwind-v4), [github.com/shadcn-ui/ui](https://github.com/shadcn-ui/ui) |

---

## 4. 「AIにデザインを描かせる」系ツール — 2026年動向

### Anthropic Claude Design

- **claude.ai/design** — 永続 DS workspace。DESIGN.md import、component preview、code round-trip
- 2026年に major overhaul（DS import 強化、token-burning 改善）— [venturebeat.com](https://venturebeat.com/technology/anthropic-ships-major-claude-design-overhaul-with-design-system-imports-code-round-trips-and-a-fix-for-its-token-burning-problem)
- **awesome-claude-design** と連携し、DESIGN.md 起点の scaffold が標準フローに

### Open Design（@OpenDesignHQ 系）

- **open-design.ai** — open-source Claude Design alternative
- **最新版: open-design-v0.12.0**（2026-06-26）。v0.10 で「Agentic design workspace」— 参考収集・インタラクティブ編集・motion・Code Agent handoff を単一ウィンドウで
- 150+ design systems、261 plugins（README 記載）
- 出典: [open-design.ai](https://open-design.ai/), [github.com/nexu-io/open-design](https://github.com/nexu-io/open-design)

### Cursor（@cursor_ai）

- **Browser Visual Editor** — IDE 内 drag-and-drop による visual editing。デザインモード改善が changelog に継続 ([cursor.com/blog/browser-visual-editor](https://cursor.com/blog/browser-visual-editor), [cursor.com/changelog](https://cursor.com/changelog))
- DS harness との関係: **visual edit → code diff → harness CI** のループが現実的な統合点。単体では DS 契約の代替にならない

### その他の動向

| ツール | 役割 |
|--------|------|
| **Google Stitch** | 視覚プロンプト → design.md 生成の入口 |
| **Figma MCP** | 既存 Figma DS → Claude Code context。双方向 sync |
| **Impeccable browser extension** | live iteration + deterministic audit |
| **Supernova** | enterprise DS → AI-readable export |

**2026年の整理:** 視覚ツール（Claude Design / Open Design / Cursor visual editor）は **探索・発見・初期 scaffold** に強い。**本番品質の正本は依然として repo 内 contracts + harness** である、という二層構造が業界標準になりつつある。

---

## 5. 「claude-design」新規プロジェクト — 断定的推奨構成

以下を **2026年6月時点の最善構成** として推奨します。

### アーキテクチャ概要

```
┌─────────────────────────────────────────────────────────┐
│  SSOT: tokens/*.json (DTCG 2025.10)                     │
│    ↓ Terrazzo 2.4.0                                     │
│    ├→ src/styles/tokens.css (@theme CSS variables)      │
│    ├→ tailwind CSS vars (OKLCH)                         │
│    └→ DESIGN.md (Google spec 0.3.0, auto-export)        │
├─────────────────────────────────────────────────────────┤
│  Contracts: design/contracts/                           │
│    tokens.json · rules.json · components/*.json         │
├─────────────────────────────────────────────────────────┤
│  Harness: scripts/design/ + CI + Playwright/axe           │
│    + Impeccable cli 3.1.0 detectors (optional layer)    │
├─────────────────────────────────────────────────────────┤
│  Agent Entry: DESIGN.md + CLAUDE.md + .claude/skills/   │
│    Optional: MCP server (token lookup, rule check)      │
├─────────────────────────────────────────────────────────┤
│  Implementation: React 19 + Tailwind 4.3.1 + shadcn   │
│    4.12.0 (new-york)                                    │
└─────────────────────────────────────────────────────────┘
```

### レイヤー別の具体指定

#### A. トークン正本（SSOT）

```text
tokens/
  color.light.json      # DTCG 2025.10, OKLCH values
  color.dark.json
  typography.json
  spacing.json
  radius.json
  motion.json
```

- **変換:** `Terrazzo 2.4.0`（primary）→ `src/styles/tokens.css` + Tailwind `@theme`
- **multi-platform 必要時:** `Style Dictionary 5.5.0` を secondary に追加
- **色空間:** **OKLCH を正**。semantic tokens（`--color-primary`）は decision tokens から alias

#### B. DESIGN.md ハーネス

```text
design/
  DESIGN.md              # Terrazzo export → Google design.md 0.3.0 format
                         # Brand prose + 7原則 + Quick Reference（手書きは prose のみ）
  CLAUDE.md              # 読み込み順序・npm scripts・PR 手順
  contracts/
    tokens.json          # flattened reference（agent 向け）
    rules.json           # { id, severity, detector, message }
    components/
      button.json        # variant, size, a11y, allowed rules
```

- **生成ルール:** `DESIGN.md` front matter は **必ず tokens から自動生成**（melta UI 方式）
- **読み込みモード:**
  - Quick: `DESIGN.md` のみ（小規模 UI 変更）
  - Standard: + 対象 `components/*.json`
  - Full: + `rules.json` + `tokens.json`

#### C. Harness（検証）

```text
scripts/design/
  validate.ts            # JSON Schema validation
  export-designmd.ts     # tokens → DESIGN.md
  drift-check.ts         # CSS vars vs tokens.json
  lint-generated.ts      # 生成物の整合性
.github/workflows/design-check.yml
tests/a11y/              # Playwright + axe-core
```

- **追加:** `npx impeccable install` → 44 detector rules を PR gate に
- **ルール例:** 禁止 gradient、最小 contrast、禁止 font stack、component API 逸脱

#### D. 実装スタック

| 技術 | バージョン | 理由 |
|------|-----------|------|
| **React** | **19.x** | shadcn 4.x デフォルト |
| **Tailwind CSS** | **4.3.1** | CSS-first, OKLCH native, `@theme` |
| **shadcn/ui** | **4.12.0** | copy-paste 所有モデル、Tailwind v4 完全対応、`data-slot` |
| **Style** | **new-york** | shadcn 2026 デフォルト |
| **Bundler** | Vite 6.x | Tailwind v4 推奨構成 |

#### E. AI 統合

```text
.claude/skills/claude-design/
  SKILL.md               # DS 固有 skill（init, audit, component-add）
.mcp.json                # optional: ds-mcp（getToken, checkRule, getComponent）
```

- **Claude Code:** `CLAUDE.md` で Quick/Standard/Full の使い分けを強制
- **Claude Design / Open Design:** **探索・モック用途のみ**。確定したトークンは SSOT に逆流 import してから merge
- **Figma MCP:** 既存 visual asset がある場合のみ接続。正本は repo

### 採用しないもの（2026年時点）

| 避ける | 理由 |
|--------|------|
| HSL-only token SSOT | OKLCH + Tailwind v4 が標準化済み |
| DESIGN.md 手書きメンテ | drift 必至（melta UI が実証） |
| Figma-only DS | agent は machine-readable contracts を読めない |
| Style Dictionary のみ（Web 単一） | Terrazzo 2.4.0 の方が Tailwind v4 出力が直接的 |
| harness なき Claude Design 単独運用 | code と visual の乖離 |

### 初期化コマンド（推奨）

```bash
# 1. App scaffold
npx shadcn@4.12.0 init          # Tailwind 4.3.1 + React 19 + new-york

# 2. Token pipeline
npm i -D @terrazzo/cli@2.4.0 @terrazzo/parser@2.4.0

# 3. Harness
npx impeccable install           # Impeccable cli 3.1.0

# 4. DS skill
# design/DESIGN.md + design/CLAUDE.md + design/contracts/ を配置
# CLAUDE.md に読み込みモードを記述
```

---

## 総括

2026年6月時点で「claude design」= AI-native design system の最善構成は:

> **DTCG 2025.10 JSON（OKLCH）を SSOT とし、Terrazzo 2.4.0 で Tailwind 4.3.1 + OKLCH CSS variables を生成、Google design.md 0.3.0 形式の DESIGN.md を自動 export、JSON contracts + TypeScript harness + CI で破りを検知、React 19 + shadcn/ui 4.12.0 で実装、Claude Code は DESIGN.md + CLAUDE.md + skill で駆動、視覚ツール（Claude Design / Open Design / Cursor visual editor）は探索層に留める。**

@tsubotax の melta UI がこの構成の **最も完成度の高い参照実装**であり、Google design.md spec・Impeccable・OpenAI harness engineering・Anthropic Claude Design が理論とツールを補完しています。日本語圏では @tsubotax が先行事例、英語圏では pbakaus（Impeccable）と VoltAgent（awesome-claude-design）が同じ方向に収束しています。
