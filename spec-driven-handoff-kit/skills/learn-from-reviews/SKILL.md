---
name: learn-from-reviews
description: >-
  Harvest review comments from recent merged pull requests, find recurring feedback
  patterns, and turn them into persistent review rules so the same issues don't
  recur. Use periodically (e.g. weekly) or when the user says "learn from reviews"
  or "update review rules".
---

# learn-from-reviews — レビュー指摘をルール化する

最近のレビュー指摘の傾向を学習し、恒久ルールに変換します。**同じ指摘を再発させない**のが目的です。

## 手順

1. 最近マージされた自分のPRのレビューコメントを集める（GitHub なら `gh` を使う）:
   - `gh pr list --state merged --author @me --limit 20 --json number,title`
   - 各PRについて `gh pr view <number> --comments`（または `gh api` でレビューコメント取得）。
2. コメントを分類し、**2回以上繰り返されているパターン**を抽出する
   （命名、エラー処理、テスト不足、ログのPII、N+1クエリ、など）。
3. それぞれを**短い命令形のルール**に言い換える（例:「ログにメールアドレスを出さない」）。
4. 追記先を提案する（採用は人間が判断）:
   - レビュー基準 → `REVIEW.md`（Claude）/ `.github/instructions/*.instructions.md`・
     `.github/copilot-instructions.md`（Copilot）の「常にチェック」欄。
   - 一般規約 → `AGENTS.md` の「やってはいけないこと」。
   - 重要な方針転換 → `memory-bank/decisions/` に ADR。
5. 追記案の**差分を提示**し、承認後に反映する。**コードは変更しない**。

## やらないこと

- 個別PR固有の些細な指摘はルール化しない。**繰り返し・一般化できるもの**だけを昇格させる。
- ルールファイルを肥大化させない（重複・陳腐化したルールは整理を提案する）。
