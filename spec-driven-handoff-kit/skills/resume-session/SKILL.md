---
name: resume-session
description: >-
  Resume work on this repository by reading the session-handoff files and
  reconstructing the current state. Trigger at the start of a work session, or when
  the user says "resume", "where were we", "continue", or "catch up".
---

# resume-session — 引き継ぎを読んで再開する

このリポジトリは `memory-bank/` でセッション間引き継ぎを管理しています。次の手順で再開します。

1. `memory-bank/progress.md`、`memory-bank/architecture.md`、`memory-bank/decisions/` の最新、
   および進行中があれば該当の `specs/<NNNN-slug>/`（spec/plan/tasks）を読む。
2. **現在地**と**次にやるべき最重要の一手**を 3〜5 個の箇条書きで要約する。
3. 要約を**実際のコードと照合**し（progress.md が指すファイルを開く）、メモと実態の食い違いを指摘する。
4. **コードを変更する前に、ユーザーの確認を待つ。**
