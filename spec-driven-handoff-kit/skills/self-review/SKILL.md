---
name: self-review
description: >-
  Review the current diff adversarially before opening a pull request, then fix the
  findings. Use after implementing a task and before creating a PR, or when the user
  says "self review", "review my changes", or "pre-PR review".
---

# self-review — PR前の自己レビュー

PRを出す前に、いまの差分を「別人のレビュアー」として批評し、指摘を直します。
自分が書いた理屈に引きずられないよう、**差分と基準だけ**を新鮮な視点で評価します。

## 手順

1. 変更差分（`git diff` / ステージ済み）と、関連する仕様 `specs/<NNNN>/spec.md`・`REVIEW.md`・
   `AGENTS.md` を読む。
2. 差分を次の観点で評価する:
   - **正しさ**: ロジック誤り・エッジケース・競合・例外処理・後方互換性。
   - **セキュリティ**: 入力検証、認可、PII/シークレット混入、インジェクション。
   - **仕様適合**: spec の受け入れ条件を満たすか。`REVIEW.md` / `AGENTS.md` のルール違反はないか。
   - **テスト**: 受け入れ条件に対応するテストがあるか。
3. 指摘を**重大度（Important / Nit）付き**で列挙する。
4. **Important は必ず修正**、Nit は任意。修正後、テスト/リント/ビルドを実行して緑を確認し、
   証拠（実行コマンドと結果）を示す。
5. 仕様や決定に影響する修正をしたら、`memory-bank/` の更新を提案する。

## やらないこと

- 正しさに無関係なスタイル指摘で過剰設計しない（Important と仕様/REVIEW 違反に集中）。
- レビューと同じセッションの「自分びいき」に陥らない。可能なら別セッション/サブエージェントで実行する。
