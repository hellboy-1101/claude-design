# @workspace/contracts

AI agent が読む machine-readable な DS 契約。

## 構成（Phase 1〜2 で実装）

| ファイル | 内容 | 生成元 |
|---|---|---|
| `tokens.json` | flattened tokens reference (agent 向け) | `@workspace/tokens` から auto-generate |
| `rules.json` | `{ id, severity, detector, message }[]` | 手書き (規約) |
| `components/<name>.json` | variant / size / a11y / allowed rules | 手書き (component ごと) |

## rules.json のフォーマット

```json
{
  "rules": [
    {
      "id": "no-hardcoded-color",
      "severity": "error",
      "detector": "regex:#[0-9a-fA-F]{6}",
      "message": "Use var(--color-*) token instead of hex color"
    }
  ]
}
```

LLM 主観の指摘ではなく、**deterministic な detector** で判定できる形に揃える（Impeccable 流）。

## 不変ルール

- `tokens.json` は **手編集禁止**（@workspace/tokens から再生成）
- `rules.json` の id は kebab-case
- `components/*.json` は 1 component 1 ファイル
