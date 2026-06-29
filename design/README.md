# design/

Design System ハーネスの中核。

## ファイル

| ファイル | 役割 | 状態 |
|---|---|---|
| `CLAUDE.md` | DS 読込規約 (Quick/Standard/Full) | 手書き OK（規約文書） |
| `DESIGN.md` | ブランド原則 + Quick Reference (token 全体) | **手編集禁止**・**Phase 1 で auto-generate** |

## なぜ DESIGN.md がまだ存在しないか

Phase 0 の時点で `DESIGN.md` を手書きで配置すると、自分で宣言した「DESIGN.md は手書き禁止 / 派生物への手修正禁止」ルールを初回コミットで破ることになる。

そのため `DESIGN.md` は **Phase 1 で `packages/harness/export-designmd.ts` が `packages/tokens/*.json` から auto-generate** するまで作成しない。

## Phase 1 で実装予定

```bash
pnpm design:export    # tokens → DESIGN.md + contracts/tokens.json を再生成
pnpm design:check     # drift 検出 (生成物が手で変更されていないか)
```

詳細は [docs/review/2026-06-29-codex-review-request-phase0.md](../docs/review/2026-06-29-codex-review-request-phase0.md) §14.2 (β 対応) を参照。
