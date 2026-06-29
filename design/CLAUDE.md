# Design System 読込規約

DS 関連タスクで AI が読み込む規約。3 段階の読込モードで context 効率と精度を両立する。

## 読込モード

### Quick（常時・root /CLAUDE.md から import 済み）
- `design/DESIGN.md` のブランド原則と禁止事項のみ
- 小規模 UI 変更（色変更、spacing 調整、既存 component の variant 追加）に使用
- token 値の正確な確認は不要、規約の確認だけで済むケース

### Standard（DS タスク開始時に明示参照）
- Quick + 対象 component の `packages/contracts/components/<name>.json`
- 既存 component の修正、新規 variant の追加に使用
- 関連 token と rule だけを参照

### Full（新規 component 設計時のみ）
- Standard + `packages/contracts/rules.json` + `packages/contracts/tokens.json` 全部
- 新規 component を設計する時、または DS 全体に影響する判断をする時
- token 全体像と rule 全体像を把握する必要があるケース

## 不変ルール（再掲）

[root /CLAUDE.md](../CLAUDE.md) の「不変ルール (Quick)」セクションを参照。違反は CI でブロックされる。

## PR 手順（DS 関連）

1. tokens を変更する場合: `packages/tokens/*.json` を直して `pnpm design:export`（Phase 1 で実装）で `DESIGN.md` / `contracts/tokens.json` を再生成
2. component を追加する場合: `packages/ui/src/components/` に実装 + `packages/contracts/components/<name>.json` に契約を記述
3. rule を追加する場合: `packages/contracts/rules.json` に id + severity + detector + message を記述（LLM 主観に依存しない）
4. PR 前: `pnpm typecheck && pnpm lint && pnpm build` が green
5. PR で `packages/harness` の CI が通ること

## 禁止事項

- `design/DESIGN.md` の手編集（auto-generate 物）
- `packages/contracts/*.json` 内の derived field の手編集（source を直す）
- `apps/web/src/` への再利用前提コンポ実装（必ず `packages/ui` に置く）
- token 値の hardcode（必ず `var(--token-name)` 経由）
- 第二トークン正本の作成（CSS 変数手書き / SCSS マップ / Tailwind config 直書き等）
