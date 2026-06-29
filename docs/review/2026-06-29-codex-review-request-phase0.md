# Codex レビュー依頼 — claude-design Phase 0 着手前チェック

**作成日**: 2026-06-29
**レビュー対象**: 新規プロジェクト `claude-design`（AIネイティブ・デザインシステム）の方針確定とPhase 0 実行計画
**依頼者状況**: Claude Code セッションでユーザー承認済み・Phase 0 着手直前
**Codex への期待**: 方針・スタック・構成・順序に**致命的な抜け or 誤判断**が無いか独立検証してほしい。問題なければ「承認」と書いてくれて構わない（[sycophancy-awareness.md](../../../.claude/rules/sycophancy-awareness.md) に従い、捏造の指摘は不要）。

---

## 0. レビューの対象範囲（明確化）

| 範囲内 | 範囲外 |
|---|---|
| 3層ハーネス方針そのものの妥当性 | Grok レポート本文の事実検証（既に出典付きで検証済み） |
| スタック選定（DTCG/Terrazzo/Tailwind v4/shadcn/React 19/Vite）| 個々のライブラリの API 詳細 |
| モノレポ構成（apps/web + packages/*）| Phase 1 以降の実装内容（本レビュー範囲外） |
| Phase 0 タスク順序と不可逆性の判断 | コミットメッセージ規約等の細部 |

---

## 1. プロジェクト概要

`/Users/satoshiamenomori/Projects/claude-design`（greenfield, 2026-06-29 着手）に **AIネイティブなデザインシステム** を新規構築する。

- **目的**: AI コーディングエージェント（Claude Code 等）が "汎用 SaaS 顔" を生み出さないよう、契約とハーネスで制約する DS。
- **GitHub オーナー**: `hellboy-1101`（gh アクティブ垢・git global user 共に確定済み）
- **リポ名**: `claude-design`（未作成・本セッションで作成予定）
- **前提**: 前セッションで Grok WebSearch リサーチを実施、独立 4 ソース（@tsubotax/melta UI / Google design.md / pbakaus/Impeccable / Anthropic Claude Design）が同方向に収束していることを確認済み。

---

## 2. 確定した中核方針（採用）

「**DESIGN.md ハーネス**」モデル。DESIGN.md ファイル1枚ではなく、`contracts + harness + skill` の束として中核に据える。

```
Layer 1: DESIGN.md + CLAUDE.md     ← AI が最初に読む「憲法」(Quick/Standard/Full)
Layer 2: contracts/*.json          ← SSOT（DTCG tokens + rules + components）
Layer 3: harness (TS) + CI         ← schema検証・drift検出・a11yテスト
```

### 不変ルール（明示）
- **DESIGN.md は手書き禁止**。tokens から自動 export する（melta UI 方式）
- **SSOT は DTCG 2025.10 JSON**（手で書くのはここだけ）
- **色空間は OKLCH**（Tailwind v4 / shadcn 4.x 前提）
- **rules.json は `id + severity + detector + message` の構造**で LLM 主観に依存しない
- **段階読込**: Quick = DESIGN.md のみ / Standard = + 対象 `components/*.json` / Full = + `rules.json` + `tokens.json`

---

## 3. 採用スタック（npm 実地裏取り済み・2026-06-29 時点）

`npm view <pkg> version time` で全て確認済み。

| 役割 | パッケージ | Grok 主張 | npm 実在最新 | 採用 | 備考 |
|---|---|---|---|---|---|
| トークン形式 | DTCG | 2025.10 | 仕様（npm 外） | **2025.10** | W3C CG 初の安定版 |
| 主変換 | `@terrazzo/cli` | 2.4.0 | **2.4.0** (2026-06-13 published) | **2.4.0** | Web+Tailwind v4 出力に特化 |
| 主変換 | `@terrazzo/parser` | 2.4.0 | **2.4.0** | **2.4.0** | 同上 |
| 副変換 | `style-dictionary` | 5.5.0 | **5.5.0** (2026-06-21) | **5.5.0** | multi-platform 必要時のみ |
| CSS | `tailwindcss` | 4.3.1 | **4.3.1** (2026-06-26) | **4.3.1** | `@theme` + CSS vars |
| UI | `shadcn` (CLI) | 4.12.0 | **4.12.0** (2026-06-26) | **4.12.0** | new-york / data-slot / OKLCH |
| Runtime | `react` | 19.x | **19.2.7** | **19.x latest** | shadcn 4.x デフォルト |
| Bundler | `vite` | 6.x | **8.1.0** (2026-06-25) | **8.1.0** | ⚠️ Grok の主張が古い |
| Detector | Impeccable CLI | 3.1.0 | 未検証（記事ベース）| **保留** | Phase 3 で再検証 |

**懸念点（Codex に検証してほしい点）**:
- DTCG 2025.10 を Terrazzo 2.4.0 がどの程度サポートしているか？（Style Dictionary 公式は「2025.10 完全対応は v5 WIP」と明記）
- Tailwind v4.3.1 と shadcn 4.12.0 の組合せは Vite 8.1.0 で安定動作するか？（vite が新しすぎないか）
- Impeccable CLI 3.1.0 の実在性は npm view で未検証。Phase 3 着手時に再検証する方針で良いか？

---

## 4. ディレクトリ構成（モノレポ・ユーザー承認済み）

```
claude-design/
├── apps/
│   └── web/                    # shadcn init で scaffold（showcase / playground）
│       ├── src/
│       ├── package.json        # vite + react 19 + tailwind v4
│       └── ...
├── packages/
│   ├── tokens/                 # DTCG JSON SSOT（Phase 1 で実装）
│   │   └── package.json
│   ├── contracts/              # rules.json + components/*.json（Phase 1 で実装）
│   │   └── package.json
│   └── harness/                # TS 検証スクリプト（Phase 3 で実装）
│       └── package.json
├── design/
│   ├── DESIGN.md               # 自動生成・手編集禁止（Phase 1 で auto-export）
│   └── CLAUDE.md               # Quick/Standard/Full 読込モード定義
├── docs/
│   ├── research/               # Grok 正本（既に配置済み）
│   │   ├── 2026-06-29-grok-claude-design-research.md
│   │   └── 2026-06-29-grok-claude-design-prompt.md
│   └── review/                 # 本ファイルが置かれる場所
├── scripts/design/             # Phase 1 以降で実装
├── .github/workflows/          # Phase 3 で CI 整備
├── .claude/skills/             # 既に空ディレクトリあり
├── package.json                # workspaces ルート
├── .gitignore
└── README.md
```

### 構成判断の理由
- **DS 本体（packages/*）と showcase（apps/web）の分離**: DS は将来 npm 公開・他リポからの再利用を想定。混在させると分離が困難になる。
- **`design/` 配下に DESIGN.md / CLAUDE.md**: melta UI に倣う。ルート CLAUDE.md は別途プロジェクト全体の方針用に分ける選択肢あり（要レビュー）。
- **`docs/research` をコミット**: Grok 調査の出典が将来の判断根拠として価値があるため。

---

## 5. Phase 0 タスク順序（実行直前）

| # | タスク | 不可逆性 | 検証方法 |
|---|---|---|---|
| 1 | `apps/web` を `shadcn@4.12.0 init` で scaffold | ローカルのみ | `bun install && bun run build` が通る |
| 2 | ルート `package.json` (workspaces) + `.gitignore` | ローカルのみ | `bun install` で workspaces 解決 |
| 3 | `design/CLAUDE.md` + `design/DESIGN.md` (placeholder) | ローカルのみ | 手作業確認 |
| 4 | `packages/{tokens,contracts,harness}` スタブ | ローカルのみ | `bun install` |
| 5 | `git init` → 初回 commit（docs/research 含む）| 取り消し可能 | `git log` |
| 6 | **`gh repo create hellboy-1101/claude-design` → push** | **外部・取消可能だが履歴残る** | リモート確認 |
| 7 | **Vercel link → `vercel --prod` → URL 取得** | **外部・公開** | URL アクセス |

### パッケージマネージャ
- 候補: `bun` または `pnpm`
- ユーザー global rule (`coding-standards.md`): "Node.js: bun または pnpm（npm直接より推奨）"
- 暫定: **bun**（速度・ユーザー他プロジェクトでの実績）
- ⚠️ shadcn CLI が bun workspaces を正しく解決するか不明 → Phase 0 で動作確認

### コミット署名
- ユーザー global `~/.claude/settings.json` で `Co-Authored-By` 等の attribution は無効化済み
- コミットメッセージは plain text（rules/git-workflow.md 準拠）

---

## 6. 採用しないもの（明示・Grok 推奨に従う）

| 避ける | 理由 |
|---|---|
| HSL-only SSOT | OKLCH + Tailwind v4 が 2026 標準 |
| DESIGN.md 手書きメンテ | drift 必至（melta UI が実証） |
| Figma-only DS | agent は machine-readable contracts を読めない |
| Web 単一で Style Dictionary のみ | Terrazzo 2.4.0 の方が Tailwind v4 出力が直接的 |
| harness 無しの Claude Design 単独運用 | code と visual の乖離 |

---

## 7. 主要参照（Codex が読むべき順序）

1. `docs/research/2026-06-29-grok-claude-design-research.md` — Grok WebSearch 正本（出典付き、推奨構成）
2. `docs/research/2026-06-29-grok-claude-design-prompt.md` — 再調査用プロンプト雛形
3. 本ファイル — 採用方針＋裏取り結果のまとめ

---

## 8. Codex への具体的な確認質問（Bullet 化）

以下のうち**1つでも "No" / "再検討要" があれば指摘してください**。全て "問題なし" なら "承認" と書いてください。

1. **3層ハーネス方針**（DESIGN.md + contracts JSON + TS harness/CI）は、2026 年時点の AI ネイティブ DS のベストプラクティスとして妥当か？
2. **DTCG 2025.10 + Terrazzo 2.4.0 を primary**、Style Dictionary 5.5.0 を secondary という選択は妥当か？（特に DTCG 2025.10 のツール対応状況）
3. **Tailwind v4.3.1 + shadcn 4.12.0 + React 19 + Vite 8.1.0** の組合せに既知の互換性問題は無いか？（Vite 8 は特に新しい）
4. **モノレポ構成**（`apps/web` + `packages/{tokens,contracts,harness}`）は将来 npm 公開・他リポ再利用を見据えて妥当か？
5. **Phase 0 タスク順序**: ローカル scaffold → git init → GitHub 作成 → Vercel デプロイ という順序に、**やり直しコストの高い不可逆判断**が混入していないか？
6. **OKLCH 採用**: 2026 年時点で OKLCH を SSOT にすることの将来性（ブラウザ互換・他ツールチェーン互換）に懸念は無いか？
7. **`design/` 配下の DESIGN.md / CLAUDE.md** と、プロジェクトルートの CLAUDE.md（Claude Code が自動読込する場所）を**分けるべきか統合すべきか**？
8. **Impeccable CLI 3.1.0** を Phase 3 まで保留する判断は妥当か？それとも Phase 0 で同時セットアップすべきか？
9. **採用しないもの**（§6）に**追加すべき項目**はあるか？（例: tailwind-merge / class-variance-authority / radix-ui 等の方針）
10. **盲点監査**: 上記に列挙されていないが、Phase 0 着手前に判断しておくべき重要事項はあるか？

---

## 9. 既知の制約・前提（Codex への補足）

- ユーザー global rules に従い、品質ゲートとして **ビルド検証 / ランタイム検証 / テスト実行 / バグ自動修正ループ** が必須（[quality-enforcement.md](../../../.claude/rules/quality-enforcement.md)）
- **テストアップ系の指示が来たら push で止めず Vercel 本番デプロイ + URL 返却**が必須（[test-up-policy.md](../../../.claude/rules/test-up-policy.md)）
- 1セッションで新規ファイルは**最大2つ目安**（[dev-process-guard.md](../../../.claude/rules/dev-process-guard.md)）— shadcn init による生成ファイル群は scaffold として例外扱い
- 検証は「似ているか」でなく「収まっているか(fit)」を原寸で（[verification-and-premise-audit.md](../../../.claude/rules/verification-and-premise-audit.md)）

---

## 10. レビュー後の流れ

- Codex が "承認" → そのまま Phase 0 を順次実行
- Codex が "再検討要" 指摘 → 対応 or 反論を明示してから Phase 0 着手
- Codex が **Impeccable / 別構成** を強く推奨 → ユーザーに再判断を仰ぐ

---

## 11. Codex レビュー結果と対応（v2 確定 — 2026-06-29）

**Codex 総合判定**: 再検討要（10項目中 Yes:4 / No:2 / Caveat:4）
**判定後の私の対応**: 1項目は反論、6項目は受入修正、3項目はそのまま採用継続

### 11.1 反論する指摘

#### 項目2 (No判定) — @terrazzo/cli のバージョン
Codex は「npm の `@terrazzo/cli` ページは `0.6.3` を示している」と主張したが、これは誤り。前提監査として 2026-06-29 23:18 時点で再検証:

| 検証手段 | 結果 |
|---|---|
| `npm view @terrazzo/cli dist-tags` | `latest: '2.4.0'` |
| `curl https://registry.npmjs.org/@terrazzo/cli` 直叩き | `dist-tags.latest = "2.4.0"` |
| バージョン履歴の連続性 | `2.0.3 → 2.1.0 → 2.2.0 → 2.3.0 → 2.4.0` |
| npmjs.com SPA HTML 内 `0.6.3` 文字列 | **出現せず** |

→ **@terrazzo/cli 2.4.0 を採用継続**。Codex の 0.6.3 主張は古いキャッシュ or ハルシネーションの可能性が高い。再発防止として、本ドキュメントで **「npm registry 直叩き（`registry.npmjs.org/<pkg>`）を一次ソースとする」** を明記。

### 11.2 受入修正する指摘

#### 項目3 (Caveat) — Vite 版数の文書間不一致
本体は Vite **8.1.0** を採用し、研究文書 (`docs/research/2026-06-29-grok-claude-design-research.md:248`) の "Vite 6.x" 記述を上書きする。

**確定**: `vite@8.1.0`。研究文書は Grok の初期主張であり、`npm view vite version` で実在最新 8.1.0 を確認済み。研究文書側は「historical record」として保存し編集しない（Grok 調査時点の記録として価値があるため）。

#### 項目4 & 項目10 — `packages/ui` の追加（最重要）

§4 のディレクトリ構成を更新:

```
claude-design/
├── apps/
│   └── web/                    # showcase / playground 専用（実装はここに置かない）
├── packages/
│   ├── ui/                     # 【追加】publishable な実コンポーネント実装
│   │   ├── src/components/     # Button, Card, ... (DS 本体)
│   │   ├── package.json        # publishConfig 付き、将来 npm 公開
│   │   └── README.md
│   ├── tokens/                 # DTCG JSON SSOT
│   ├── contracts/              # rules.json + components/*.json
│   └── harness/                # TS 検証スクリプト
├── design/
│   ├── DESIGN.md               # 自動生成・手編集禁止
│   └── CLAUDE.md               # DS 読込規約 (Quick/Standard/Full) のみ
├── CLAUDE.md                   # 【明文化】Claude Code 自動読込起点・repo 全体運用
├── docs/
└── ...
```

**境界ルール**:
- `apps/web` は `packages/ui` に依存して**表示するだけ**。コンポーネント実装を `apps/web/src/components/` に置くことを禁止
- `packages/ui` は将来 `@hellboy-1101/claude-design-ui` として npm 公開を想定

#### 項目6 (Caveat) — ブラウザサポート境界の明文化
§1 プロジェクト概要に追記する確定事項:

> **対象ブラウザ**: modern browsers のみ（Chrome / Edge / Safari / Firefox の各最新2バージョン）。OKLCH / CSS `@property` / Tailwind v4 の `@theme` は legacy ブラウザ非対応のため、IE11 / 旧 Safari / 旧 Android 系は **明示的に対象外**。

#### 項目7 (Caveat) — CLAUDE.md 二系統の責務分離

| ファイル | 役割 | 読込タイミング |
|---|---|---|
| **ルート `/CLAUDE.md`** | repo 全体運用 / package manager / Phase 進行 / `design/CLAUDE.md` への導線 | Claude Code セッション開始時に**自動読込** |
| **`design/CLAUDE.md`** | DS 読込規約のみ — Quick/Standard/Full 三段階 + 対象 contract の指定方法 | DS 関連タスク時に明示参照 |

ルート CLAUDE.md は最小限（100行以内）に保ち、詳細は `design/CLAUDE.md` と `docs/` に分散する（ユーザー global rule "CLAUDE.md は100行以内の目次に留める" 準拠）。

#### 項目9 (Yes + 追加3項目) — 「採用しないもの」追加
§6 に以下3項目を追加:

| 追加で避ける | 理由 |
|---|---|
| **DTCG 以外の第二トークン正本を作る** | tokens の SSOT は `packages/tokens/*.json` 一択。他に CSS 変数を手書きしたり SCSS マップを別に持ったりしない |
| **生成物（DESIGN.md / contracts derived files）への手修正** | drift の原因になる。必ず source を直して再生成する |
| **`apps/web` 内に再利用前提コンポーネントを置く** | showcase の専用ロジックのみに限定。再利用される可能性があれば必ず `packages/ui` に移す |

なお Codex は「tailwind-merge / CVA / radix-ui の一律 blacklist は不要」と明言。これは採用候補として残す（shadcn 4.x がこれらを使う設計）。

#### 項目10 (No → 部分対応) — package manager 確定
**確定**: **pnpm**（ユーザー承認済み）。理由:
- shadcn 4.x monorepo 公式ガイドが pnpm ベース
- DTCG ツール群（Terrazzo / Style Dictionary）との互換性実績
- workspace プロトコル成熟度

ルート `package.json` に `"packageManager": "pnpm@<latest>"` を明記、`bun` / `npm` での install を CI でブロック。

### 11.3 そのまま採用継続（Codex も Yes 判定）

- 項目1: 3層ハーネス方針 → そのまま
- 項目5: Phase 0 タスク順序 → そのまま
- 項目8: Impeccable Phase 3 まで保留 → そのまま

---

## 12. v2 反映後の Phase 0 タスク順序（修正版）

| # | タスク | v1 からの変更 |
|---|---|---|
| 0 | **【新】ルート `CLAUDE.md` 配置**（責務: repo 運用・design への導線）| 追加 |
| 1 | `apps/web` を shadcn 4.12.0 init（pnpm 指定） | PM 確定 |
| 2 | ルート `package.json` (pnpm workspaces) + `pnpm-workspace.yaml` + `.gitignore` | pnpm 化 |
| 3 | `design/CLAUDE.md` + `design/DESIGN.md` (placeholder) | 責務を DS 読込規約のみに |
| 4 | `packages/{ui,tokens,contracts,harness}` スタブ | **`ui` 追加** |
| 5 | `git init` → 初回 commit（docs/research + docs/review 含む） | docs/review も含める |
| 6 | `gh repo create hellboy-1101/claude-design` → push | 変更なし |
| 7 | Vercel link → `--prod` → URL 取得 | apps/web を root directory に指定 |

タスクを TaskCreate に反映する（#0 追加、#4 に packages/ui を含める）。

---

## 13. v2 確定 — Phase 0 着手判断

上記対応により、Codex の "再検討要" 指摘のうち:
- 1件（@terrazzo バージョン）は反論で押し切り
- 6件は本ドキュメントで仕様凍結
- 3件は元から Yes 判定

→ **Phase 0 着手可能な状態**。ただし Phase 0 #0 として「ルート CLAUDE.md 配置」を新規追加し、その後の #1〜#7 を順次実行する。

---

## 14. Codex 再レビュー（v2→v3）— 2026-06-29

**v2 を Codex 再レビューに掛けた結果、新たに 7 件の指摘**。前回指摘の解消はOK（A:Yes / B:Yes / E:Yes / F:Yes）だが、別観点で構造的問題が露見した。

### 14.1 統合指摘リスト（2セッションから集約）

| ID | 種別 | 指摘 | 妥当性 |
|---|---|---|---|
| **α** | 必須 | `apps/web` 単独 init → workspace 後付けの順序は、shadcn 公式 `init -t vite --monorepo`（root 1発で apps/web + packages/ui + workspace 一体生成）と噛み合わない | ✅ 一次裏取り済（CLI help で `--monorepo` 確認） |
| **β** | 必須 | Phase 0 で `design/DESIGN.md (placeholder)` を人手作成すると、自分で宣言した「DESIGN.md 手書き禁止／派生物手修正禁止」を初回コミットで破る | ✅ 妥当（自己矛盾） |
| **γ** | 推奨 | `design/CLAUDE.md` は subdirectory なので Claude Code 起動時自動読込対象外。`@design/CLAUDE.md` import か Quick ルール root 持ち上げが必要 | ✅ 妥当（公式仕様） |
| **C** | 必須 | `apps/web` の実装を全面禁止する書き方は shadcn 公式 monorepo 想定とズレ。`components.json` / package exports / tsconfig path alias など monorepo 成立条件も抜け | ✅ 妥当 |
| **D** | 推奨 | ブラウザ境界が prose だけで `browserslist` フィールドや検証マトリクスに未反映 | ✅ 妥当 |
| **G** | 必須 | `"pnpm@<latest>"` は固定値でなく再現性なし。**この環境に pnpm 自体が未導入**。CI enforcement も未実装 | ✅ 一次裏取り済（npm view で pnpm@11.9.0 確認、`which pnpm` で未導入確認） |
| **H** | 必須 | 新盲点: monorepo 実装条件と pnpm 導入手順・固定版・即時 enforcement の文書化なし | ✅ 妥当（α+G の総合） |

スタック選定そのものには Codex も赤信号無しと明言（react 19.2.7 / vite 8.1.0 / shadcn 4.12.0 / @terrazzo 2.4.0 / style-dictionary 5.5.0 は両者一致）。

### 14.2 v3 修正方針

#### α 対応 — Phase 0 ブートストラップを 1 発化

Phase 0 の旧 #1+#2+#4(ui 部分) を統合し、**root で `pnpm dlx shadcn@4.12.0 init -t vite --monorepo --base radix -y` を実行**して apps/web + packages/ui + workspace を一体生成する。

旧構成: 単独 init → workspace 後付け（CLI の monorepo 理解を捨てる）
新構成: monorepo init で CLI に **`components.json`/exports/alias を全部書かせる**

#### β 対応 — DESIGN.md は Phase 0 で作らない

- `design/` ディレクトリは作る
- `design/DESIGN.md` は **Phase 1 で `pnpm design:export` 自動生成**まで存在させない
- 代わりに `design/README.md` に「DESIGN.md は Phase 1 で自動生成される」を明記
- `design/CLAUDE.md` は規約文書（派生物ではない）なので手書きで OK

#### γ + 11.2-項目7 上書き — root /CLAUDE.md で制約を効かせる

公式仕様上、Claude Code 起動時自動読込は **root `./CLAUDE.md` のみ**。subdirectory は on-demand。

**root /CLAUDE.md の構造**（雛形・100行以内）:

```md
# claude-design

AIネイティブ・デザインシステム。3層ハーネス（DESIGN.md + contracts + CI）で
generic SaaS 顔を構造的に抑制する。

## 不変ルール (Quick・常時適用)
- SSOT は `packages/tokens/*.json` (DTCG 2025.10) のみ
- 色は OKLCH 必須（modern browsers のみ対応、IE/旧 Safari は対象外）
- `DESIGN.md` / derived contracts は手編集禁止（必ず source を直して再生成）
- 再利用前提コンポは `packages/ui`、apps/web 固有の組み合わせは `apps/web/src` OK
- package manager は `pnpm@11.9.0` 固定（npm / bun 禁止・CI で enforce）

## DS 詳細（読込モード）
@design/CLAUDE.md

## ドキュメント目次
- `docs/research/` — Grok 調査正本（変更不可・historical record）
- `docs/review/` — 設計レビュー記録（Codex 含む）
```

`@<path>` 構文で `design/CLAUDE.md` を**起動時にインライン展開**させる。これで初回セッションから DS 規約が効く。

#### C 対応 — apps/web 境界ルール再定義

| 旧（v2 で書きすぎ） | 新（v3 確定） |
|---|---|
| ❌ apps/web 内に再利用前提コンポを置く禁止 | apps/web 内に**再利用前提**コンポを置く禁止 |
| （実装は全部 packages/ui） | **app 固有の組み合わせ**（page, layout, route 等）は `apps/web/src/` で OK |
| | 判断基準: 「他の app から import する可能性があるか」が Yes なら packages/ui |
| | shadcn の追加コンポは `pnpm dlx shadcn add <name>` で **`packages/ui` 側に追加**（monorepo init 後の `components.json` に従う） |

#### D 対応 — browserslist と検証マトリクスの明文化

root `package.json` に追加:
```json
"browserslist": [
  "last 2 Chrome versions",
  "last 2 Edge versions",
  "last 2 Firefox versions",
  "last 2 Safari versions",
  "not dead",
  "not ie 11"
]
```

検証マトリクス（CI で Playwright を Phase 3 で導入時に有効化）:
- Chrome latest, Safari latest, Firefox latest（最低限）
- iOS Safari latest, Android Chrome latest（モバイル）

#### G 対応 — pnpm 具体版固定 + 即時導入

- 実在最新: **pnpm@11.9.0**（`npm view pnpm version` で確認、2026-06-29 時点）
- 現環境に pnpm 未導入 → Phase 0 #0 で `corepack enable && corepack prepare pnpm@11.9.0 --activate` を実行
- root `package.json` に `"packageManager": "pnpm@11.9.0"` を明記
- CI: `engines.packageManager` 強制 + `pre-commit` で `which pnpm` チェック（Phase 3 で）

#### H 対応 — monorepo 成立条件の検証ステップを Phase 0 に追加

shadcn `init -t vite --monorepo` で生成される想定物（要 Phase 0 検証）:
- `pnpm-workspace.yaml` (apps/*, packages/*)
- root `package.json` (packageManager, workspaces)
- `apps/web/package.json` (依存に `@workspace/ui` 等)
- `packages/ui/package.json` (exports, peer deps)
- `components.json` (root or packages/ui — registries 設定)
- `tsconfig.base.json` + 各 `tsconfig.json` (path aliases)

Phase 0 タスクとして **これらの存在と整合性を確認**するステップを追加。

### 14.3 v3 確定 — Phase 0 タスク順序（10ステップ）

| # | タスク | 不可逆性 | 検証 |
|---|---|---|---|
| 1 | `corepack enable && corepack prepare pnpm@11.9.0 --activate` → `pnpm -v` で確認 | ローカル（取消可）| `pnpm -v == 11.9.0` |
| 2 | repo root で `pnpm dlx shadcn@4.12.0 init -t vite --monorepo --base radix -y -n claude-design` | ローカル | apps/web + packages/ui + workspace 生成 |
| 3 | monorepo 成立条件検証（components.json/exports/tsconfig path/workspace 解決）| ローカル | `pnpm install && pnpm -r ls` |
| 4 | root /CLAUDE.md 配置（Quick ルール + `@design/CLAUDE.md` import） | ローカル | Claude Code が次セッションで読込 |
| 5 | `design/CLAUDE.md` + `design/README.md` 配置（DESIGN.md は作らない） | ローカル | 手作業確認 |
| 6 | `packages/{tokens,contracts,harness}` スタブ追加（ui は shadcn init で生成済） | ローカル | `pnpm install` で解決 |
| 7 | root `package.json` に `browserslist` 追加 + `engines.packageManager` 強制 | ローカル | `pnpm install` 通過 |
| 8 | `git init` → 初回 commit（docs/research + docs/review 含む） | 取消可 | `git log` |
| 9 | `gh repo create hellboy-1101/claude-design --public --source=. --remote=origin --push` | **外部・要確認** | リモート確認 |
| 10 | Vercel link (root: `apps/web`) → `vercel --prod --yes` → URL 取得 | **外部・公開** | URL アクセス |

### 14.4 三度目の Codex レビューに送るための要点

v3 では以下が変わっている:
- §14 で v2 v.s. 再 Codex の指摘 7 件全てに対応案を明記
- Phase 0 が 10 ステップに再編、shadcn monorepo init が中心軸
- DESIGN.md placeholder 廃止
- root CLAUDE.md が Quick 直書き + `@design/CLAUDE.md` import
- pnpm@11.9.0 固定
- browserslist + engines.packageManager で enforcement

これで「α/β/γ/C/D/G/H 全て解消」を Codex に確認してもらい、承認を得てから Phase 0 着手する。

---

## 15. v3 確定 — Phase 0 着手判断（次工程）

→ v3 の承認は **Codex 三度目レビュー待ち**。承認後、§14.3 のタスク順で順次実行。

以上 (v3)。

---

## 16. Codex 三度目レビュー結果と v3.1 軽微修正（2026-06-29）

### 16.1 三度目レビューの判定

| 項目 | 判定 | コメント |
|---|---|---|
| α monorepo init 1発化 | ✅ Yes | 解消（公式導線と一致） |
| β DESIGN.md placeholder 廃止 | ✅ Yes | 解消（自己矛盾なし） |
| γ root CLAUDE.md auto-load | ✅ Yes | 解消（@path import 公式仕様と整合） |
| C apps/web 境界ルール | ✅ Yes | 解消（shadcn 公式想定と一致） |
| D browserslist 明文化 | ✅ Yes | 解消 |
| G pnpm 具体版固定 | ⚠️ Caveat | Corepack 手順が pnpm 公式推奨と細部不一致、`engines.packageManager` enforcement の根拠が弱い |
| H monorepo 成立条件検証 | ⚠️ Caveat | `tsconfig.base.json` 前提は過剰（shadcn 公式は components.json/alias/exports 重視） |
| I 新盲点監査 | ❌ No | **pnpm 11 は Node.js 22+ 前提**なのに事前確認が無い |
| J 一次裏取り正確性 | ⚠️ Caveat | shadcn / pnpm 版数 OK、Corepack 手順は完全一致ではない |

**総合: Yes:5 / Caveat:3 / No:1**

### 16.2 前提監査ループ警告

[verification-and-premise-audit.md](../../../.claude/rules/verification-and-premise-audit.md) B-1 シグナル監視:
- 1回目: 構造的指摘 → v2 で解消
- 2回目: 別の構造的指摘 → v3 で解消
- 3回目: **精度 Caveat に変質**（指摘の質が「構造誤り」→「もっと精度を上げろ」へ）

→ 同一指摘の反復ではないが、無限精度要求ループに入る兆候。**ここで止めて Phase 0 に進む判断**は妥当。残り指摘は Phase 0 実行時の手当てで対応する。

### 16.3 v3.1 修正内容（最小）

#### I 対応（最重要）— Node.js 22+ 事前確認を Phase 0 #1 に格上げ

新 Phase 0 #1: `node -v` で **v22.0.0 以上**を確認。未満の場合は nvm 等で更新を促し、Phase 0 を中断する。pnpm 11 系は公式に Node.js 22+ 必須。

#### G 対応（軽微）— Corepack 手順の補足

旧 v3 Phase 0 #1（→ v3.1 では #2 に繰下げ）の手順は Node.js 公式の Corepack 手順に従ったもので動作するが、pnpm 公式は別途 `corepack enable pnpm && corepack use pnpm@11.9.0` を推奨。**どちらも有効**だが、Phase 0 実行時に `pnpm -v` で `11.9.0` が出れば成功とする。

`engines.packageManager` は Node.js 公式機能で Corepack が読み取る。v3.1 では「Phase 0 #7 で root package.json に書く、CI enforcement は Phase 3 で導入」と明確化。

#### H 対応（軽微）— tsconfig.base.json 前提の緩和

旧 v3 §14.2 H 対応の検証項目に `tsconfig.base.json` を必須として挙げたが、shadcn init の実生成物に依存。**実態確認に変更**:
- 必須確認: `components.json` registries / `packages/ui/package.json` exports / `pnpm-workspace.yaml` の解決
- 任意確認: `tsconfig.base.json` がある場合のみ path alias を確認、無ければ `apps/web/tsconfig.json` 単体で確認

### 16.4 v3.1 確定 — Phase 0 タスク順序（11 ステップ）

| # | タスク | 不可逆性 |
|---|---|---|
| **1** | **`node -v` で v22+ を確認**（未満なら nvm 等で更新・Phase 0 中断）| ローカル |
| 2 | `corepack enable && corepack prepare pnpm@11.9.0 --activate` → `pnpm -v` で確認 | ローカル |
| 3 | repo root で `pnpm dlx shadcn@4.12.0 init -t vite --monorepo --base radix -y -n claude-design` | ローカル |
| 4 | monorepo 成立条件検証（components.json/exports/workspace 解決。tsconfig.base.json は実態に応じて） | ローカル |
| 5 | root /CLAUDE.md 配置（Quick ルール + `@design/CLAUDE.md` import） | ローカル |
| 6 | `design/CLAUDE.md` + `design/README.md` 配置（DESIGN.md は作らない）| ローカル |
| 7 | `packages/{tokens,contracts,harness}` スタブ追加（ui は init で生成済）| ローカル |
| 8 | root `package.json` に `browserslist` + `engines.packageManager: pnpm@11.9.0` | ローカル |
| 9 | `git init` → 初回 commit | 取消可 |
| 10 | `gh repo create hellboy-1101/claude-design` → push | **外部・要確認** |
| 11 | Vercel link (root: `apps/web`) → `vercel --prod --yes` → URL 取得 | **外部・公開** |

### 16.5 v3.1 確定 — Codex 再々々レビュー無し、Phase 0 着手判断

ユーザー判断（2026-06-29）により、v3.1 軽微修正で確定。**Codex 四度目レビューは行わない**（精度要求ループ回避）。残り Caveat は Phase 0 実行中の検証で対処する。

以上 (v3.1)。
