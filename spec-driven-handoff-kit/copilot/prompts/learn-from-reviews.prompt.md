---
description: 'マージ済みPRのレビュー指摘を学習し、レビュー観点ルールに追記する'
agent: 'agent'
---

最近のPRレビュー指摘の傾向を学習し、再発防止のルールに変換してください。

1. 最近マージされた自分のPRのレビューコメントを `gh` で集める:
   - `gh pr list --state merged --author @me --limit 20 --json number,title`
   - 各PRについて `gh pr view <number> --comments`。
2. **2回以上繰り返されている指摘パターン**を抽出し、短い命令形のルールに言い換える。
3. 追記先を提案:
   - レビュー観点 → `.github/copilot-instructions.md` または `.github/instructions/*.instructions.md`
   - 一般規約 → `AGENTS.md` の「やってはいけないこと」
   - 重要方針 → `memory-bank/decisions/` の ADR
4. 追記案の差分を提示し、承認後に反映（**コードは変更しない**）。

個別の些細な指摘はルール化しない。繰り返し・一般化できるものだけを昇格させること。
