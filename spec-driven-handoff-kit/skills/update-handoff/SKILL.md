---
name: update-handoff
description: >-
  Distill the current session into the repository's handoff files before ending.
  Trigger at the end of a work session, or when the user says "wrap up", "update
  handoff", "save progress", or "hand off".
---

# update-handoff — セッションを引き継ぎファイルに蒸留する

このセッションの成果を引き継ぎファイルに反映します。**この手順ではソースコードを変更しないこと。**

1. `memory-bank/progress.md` を更新：「現在の状態」「次の一手」を書き換え、「最近の変更」に
   日付つきで1行追記（plan / done / 触ったファイル / next を簡潔に）。
2. 重要な技術・設計判断があれば `memory-bank/decisions/` に**新しいADR**を追記
   （次の連番、既存ADRをテンプレートに、過去のADRは編集せず supersede）。
3. 仕様や構成が変わったら該当の `specs/<NNNN-slug>/`（spec/plan/tasks）と
   `memory-bank/architecture.md` を更新。
4. 最後に、更新した**引き継ぎファイルの差分（diff）を提示**してレビュー可能にする。
