# @workspace/tokens

DTCG Format Module 2025.10 準拠の design tokens SSOT。

## 構成（Phase 1 で実装）

| ファイル | 内容 |
|---|---|
| `color.light.json` | Light mode カラー (OKLCH) |
| `color.dark.json` | Dark mode カラー (OKLCH) |
| `typography.json` | font-family / font-size / line-height / weight |
| `spacing.json` | spacing scale |
| `radius.json` | border-radius scale |
| `motion.json` | duration / easing |

## 不変ルール

- **DTCG 2025.10 形式のみ**（手書き CSS 変数や SCSS マップを並走させない）
- **色は OKLCH 必須**（HSL / RGB は禁止）
- **semantic token は decision token から alias**（`--color-primary` は `--color-blue-500` を参照する形）

## Phase 1 で追加する script

```bash
pnpm tokens:build       # Terrazzo 2.4.0 で tokens.css と Tailwind @theme を生成
pnpm tokens:export      # DESIGN.md と contracts/tokens.json を再生成
```
