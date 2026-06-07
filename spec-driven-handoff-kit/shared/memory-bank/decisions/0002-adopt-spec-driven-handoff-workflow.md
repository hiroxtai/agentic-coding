# 2. 仕様駆動＋インタビュー＋ADR引き継ぎの統合ワークフローを採用する

- ステータス: accepted
- 日付: 2026-06-07
- 置換: なし

## 背景（Context）
要件の曖昧さ、実装の手戻り、セッション/エージェント間での文脈喪失を減らしたい。
対象エージェントは GitHub Copilot・Claude Code・OpenAI Codex の3種。

## 決定（Decision）
次を統合した運用を採用する。

1. **インタビュー深掘り（grill-me）** で要件・制約・前提・代替案を実装前に洗い出す。
2. **Spec Kit** による仕様駆動（constitution → specify → plan → tasks → implement）。
3. **ADR ＋ memory-bank** によるセッション間引き継ぎ。
4. 共通の背骨として **AGENTS.md** を全エージェントが読む（ツール横断標準）。

## 結果（Consequences）
- どのプロジェクト・どのタスクでも同じ流れで進められる。
- エージェントを切り替えても（Copilot↔Claude Code↔Codex）同じ spec/ADR/progress を共有できる。
- 各ツール固有の導入（コマンド/スキルの置き場所）は SETUP.md に従う。
