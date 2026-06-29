# @workspace/harness

DS の検証ハーネス。tokens / contracts の正しさと、生成物 (DESIGN.md / derived tokens.json) の drift を CI で検出する。

## 実装予定 (Phase 1〜3)

| script | 役割 | Phase |
|---|---|---|
| `design:export` | `@workspace/tokens` → `design/DESIGN.md` + `@workspace/contracts/tokens.json` 再生成 | Phase 1 |
| `design:check` | 生成物が手編集されていないか drift 検出 | Phase 3 |
| `design:validate` | tokens / contracts の JSON Schema 検証 | Phase 3 |
| `design:a11y` | Playwright + axe-core で a11y テスト | Phase 3 |
| `design:lint` | rules.json の detector を実行 (Impeccable 互換) | Phase 3 |

## CI ゲート (Phase 3)

```yaml
# .github/workflows/design-check.yml
- pnpm design:check       # drift 検出
- pnpm design:validate    # schema 検証
- pnpm design:a11y        # a11y テスト
- pnpm design:lint        # ルール違反検出
```

## 依存予定

- `@terrazzo/cli@2.4.0` + `@terrazzo/parser@2.4.0` (tokens → CSS variables)
- `style-dictionary@5.5.0` (multi-platform 出力が必要になった場合)
- `ajv` (JSON Schema validation)
- `@playwright/test` + `axe-core` (a11y)
