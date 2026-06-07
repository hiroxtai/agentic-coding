---
applyTo: '**/*.ts,**/*.tsx'
---

# TypeScript 向けの指示（パス別）

このファイルは `.ts` / `.tsx` を編集するときだけ自動適用されます（モノレポや言語別運用の例）。

- ES Modules（`import` / `export`）を使う。CommonJS（`require`）は使わない。
- 名前付きエクスポートを優先する。
- `any` を避け、型を明示する。
- 変更後は `npm run lint` と `npm test` を実行し、通ることを確認してから完了とする。
