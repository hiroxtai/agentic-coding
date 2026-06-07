<!-- 配置先: .github/copilot-instructions.md（GitHub Copilot が毎タスク自動で読む） -->
<!-- 実体は AGENTS.md に集約し、ここからは参照だけ（二重管理を避ける）。 -->

# Copilot への指示

このリポジトリのエージェント向け指示は、リポジトリ直下の **`AGENTS.md`** に集約されています。
**すべてのタスクで `AGENTS.md` に必ず従ってください。** とくに以下を厳守：

- 標準ワークフロー（深掘り→仕様→計画→分解→実装→記録→引き継ぎ）。
- セッション開始時に `memory-bank/progress.md` と `architecture.md` を読む。
- 重要判断は `memory-bank/decisions/` にADR追記。終了時に `progress.md` を更新。
- 要件が曖昧なら、実装前に `/grill-me` で詰める。
