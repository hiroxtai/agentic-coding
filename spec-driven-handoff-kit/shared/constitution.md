# Project Constitution（プロジェクト憲法）

<!-- Spec Kit の憲法。配置先: .specify/memory/constitution.md -->
<!-- Spec Kit を使う場合は `/speckit.constitution` で生成・更新してもよい。これはその雛形。 -->
<!-- ここに書くのは「交渉不可能な原則」。全フェーズ（specify/plan/tasks/implement）がこれに従う。 -->

## 原則（Principles）

1. **仕様が真実の源**：実装の前に spec を確定する。コードと spec が食い違ったら spec に合わせるか、
   spec を更新してから実装する。
2. **小さく検証可能に**：タスクは単独で実装・テストできる粒度に割る。各タスクは検証で締める。
3. **決定は記録する**：重要な技術/設計判断は ADR（`memory-bank/decisions/`）に残す。追記のみ・supersede。
4. **引き継ぎを前提に書く**：次のセッション（別エージェントでも）が現在地を復元できるよう、
   `memory-bank/` を最新に保つ。

## 技術的制約（Constraints / 各自記入）

- 言語/スタック: <例: TypeScript / Node.js>
- 必須: <例: すべての公開APIにテストを付ける>
- 禁止: <例: 新規の外部依存を無断追加しない>

## 品質ゲート（Quality gates / 各自記入）

- マージ前に: <例: lint・typecheck・test がすべて green>
- レビュー: <例: 重要変更は敵対的レビュー（別セッション/サブエージェント）を通す>

## 改定（Amendment）

この憲法の変更は ADR として記録する（例: `decisions/00NN-amend-constitution.md`）。
