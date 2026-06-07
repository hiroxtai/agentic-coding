---
description: 'セッション終了：今回の作業を引き継ぎファイルに蒸留する'
agent: 'agent'
---

このセッションの成果を引き継ぎファイルに反映してください。**ソースコードは変更しないこと。**

1. `memory-bank/progress.md` を更新（現在の状態・次の一手を書き換え、「最近の変更」に日付つきで1行追記）。
2. 重要な技術/設計判断があれば `memory-bank/decisions/` に新しいADRを追記
   （次の連番、既存ADRをテンプレートに、過去のADRは編集せず supersede）。
3. 仕様/構成が変わったら該当の `specs/<NNNN-slug>/` と `memory-bank/architecture.md` を更新。
4. 更新した引き継ぎファイルの**差分（diff）を提示**してください。
