# 仕様駆動 × インタビュー深掘り × ADR引き継ぎ ― 統合構築キット（SETUP）

GitHub Copilot / Claude Code / OpenAI Codex の **3エージェント横断**で、どのプロジェクト・
どのストーリー/タスクでも同じ流れで開発できる統合環境を作ります。設計の核は次の一文です。

> **AGENTS.md を全エージェント共通の“背骨”にし、Spec Kit はエージェント別に導入、
> grill-me は移植可能な Agent Skill として共有、ADR＋memory-bank で引き継ぐ。**

まずこの SETUP.md を通読し、続いて「2. 初期構築」を実施してください。

---

## 0. 全体像

### 構成の考え方：共有コア ＋ エージアダプタ

```
┌──────────────────────────── 共有コア（ツール非依存・git管理）────────────────────────────┐
│  AGENTS.md                  … 全エージェントが読む背骨（ワークフロー＋規約）                │
│  .specify/memory/constitution.md … プロジェクト憲法（Spec Kit）                            │
│  specs/<NNNN-slug>/{spec,plan,tasks}.md … 仕様駆動の成果物                                  │
│  memory-bank/{progress,architecture}.md, decisions/ … セッション引き継ぎ（ADR）            │
└───────────────────────────────────────────────────────────────────────────────────────────┘
        ▲                         ▲                          ▲
        │ 読む/書く                │ 読む/書く                 │ 読む/書く
┌───────┴───────┐        ┌────────┴────────┐        ┌────────┴────────┐
│ GitHub Copilot │        │  Claude Code     │        │  OpenAI Codex    │
│ .github/        │        │ .claude/skills/  │        │ Codex skills +   │
│  copilot-instr. │        │ AGENTS.md(+CLAUDE)│       │ AGENTS.md 連鎖    │
│  prompts/*.md   │        │ AskUserQuestion  │        │ memories(自動)   │
│ Copilot Memory  │        │ memory(自動)     │        │ resume           │
└─────────────────┘        └──────────────────┘        └──────────────────┘
```

- **共有コア**はどのエージェントからも同じファイルとして見える。エージェントを切り替えても
  spec / ADR / progress は共有される。
- **アダプタ**は各ツールの「コマンド/スキルの置き場所」の違いを吸収するだけ。

### キットのフォルダ（このキット内）

```
spec-driven-handoff-kit/
├─ SETUP.md                      ← このファイル
├─ shared/                       ← リポジトリのルートにコピーする共有コア
│   ├─ AGENTS.md
│   ├─ constitution.md           → .specify/memory/constitution.md に置く
│   ├─ memory-bank/{progress,architecture}.md, decisions/{0001,0002}.md
│   └─ specs/0001-example-feature/spec.md   ← spec.md の書き方の見本
├─ skills/                       ← 移植可能な Agent Skill（Claude Code / Codex 用）
│   ├─ grill-me/SKILL.md
│   ├─ resume-session/SKILL.md
│   └─ update-handoff/SKILL.md
└─ copilot/                      ← Copilot 用アダプタ
    ├─ copilot-instructions.md   → .github/copilot-instructions.md に置く
    └─ prompts/{grill-me,resume-session,update-handoff}.prompt.md → .github/prompts/ に置く
```

---

## 1. 前提（インストール）

| 対象 | 導入 |
| --- | --- |
| Spec Kit (`specify` CLI) | `uvx --from git+https://github.com/github/spec-kit.git specify --help` |
| GitHub Copilot | VS Code 拡張。Settings→Copilot で **Memory を ON** |
| Claude Code | CLI を導入（`/skills` が使えること） |
| OpenAI Codex | Codex CLI を導入。`codex` が動くこと |
| grill-me（任意の公式版） | Matt Pocock の Agent Skill。本キットにも汎用版を同梱 |

---

## 2. プロジェクトの初期構築（ワンタイム）

### 2.1 共有コアを置く

このキットの `shared/` の中身を、あなたのリポジトリのルートにコピーします。

```bash
cp -r shared/AGENTS.md shared/memory-bank shared/specs <your-repo>/
mkdir -p <your-repo>/.specify/memory && cp shared/constitution.md <your-repo>/.specify/memory/constitution.md
```

### 2.2 Spec Kit をエージェント別に導入

各エージェント向けに、Spec Kit のコマンド群（`/speckit.*`）を導入します（**既存リポジトリにも追加可**）。

```bash
# 使うエージェントごとに1回ずつ（同じリポジトリで併用可）
specify init . --integration copilot
specify init . --integration claude
specify init . --integration codex
# 版により --ai の場合あり。`specify init --help` で確認。非対話時の既定は copilot。
```

これで `/speckit.constitution` `/speckit.specify` `/speckit.plan` `/speckit.tasks` `/speckit.implement`
が各エージェントで使えるようになります（Copilot は `.github/prompts/`、Claude/Codex は skills として導入）。

### 2.3 スキル/プロンプトをエージェント別に配置

| ファイル | GitHub Copilot | Claude Code | OpenAI Codex |
| --- | --- | --- | --- |
| 背骨 | `AGENTS.md`（ルート）＋ `.github/copilot-instructions.md` | `AGENTS.md`（＋任意で `CLAUDE.md`） | `AGENTS.md`（連鎖で読む） |
| grill-me | `.github/prompts/grill-me.prompt.md` | `.claude/skills/grill-me/SKILL.md` | Codex の skills ディレクトリに `grill-me/SKILL.md` |
| resume-session | `.github/prompts/resume-session.prompt.md` | `.claude/skills/resume-session/SKILL.md` | 同上 |
| update-handoff | `.github/prompts/update-handoff.prompt.md` | `.claude/skills/update-handoff/SKILL.md` | 同上 |

```bash
# Copilot 用
mkdir -p <your-repo>/.github/prompts
cp copilot/copilot-instructions.md <your-repo>/.github/copilot-instructions.md
cp copilot/prompts/*.prompt.md      <your-repo>/.github/prompts/

# Claude Code 用（スキル）
mkdir -p <your-repo>/.claude/skills
cp -r skills/* <your-repo>/.claude/skills/

# Codex 用（スキル）… Codex のスキル配置先にコピー（`codex` のドキュメント/`--help` で確認）
# 例: プロジェクト内のスキルディレクトリ、または ~/.codex 配下
```

`CLAUDE.md` を使う場合は1行だけの中継で十分：

```markdown
# CLAUDE.md
このリポジトリのエージェント指示は AGENTS.md に集約。必ず AGENTS.md に従うこと。
```

### 2.4 中身を記入する（最重要）

- `AGENTS.md` … プロジェクト概要・ビルド/テストコマンド・コード規約の `<...>` を埋める。
- `.specify/memory/constitution.md` … 技術的制約・品質ゲートを記入（`/speckit.constitution` で生成も可）。
- `memory-bank/progress.md` / `architecture.md` … 現状をざっくり記入（最初は粗くてよい。育てる）。
- コミットして共有：`git add AGENTS.md .github .claude .specify memory-bank specs && git commit -m "Add unified spec-driven handoff setup"`

---

## 3. 毎タスクの手順（どのストーリー/タスクでも同じ）

新しいストーリー/タスクは、毎回この8ステップで進めます。**開始＝読む / 終了＝書く** が要。

```
(0) 再開    → 引き継ぎを読む
(1) 深掘り  → grill-me で要件・制約・前提・代替案を洗い出す
(2) 仕様    → spec.md を確定（何を・なぜ）
(3) 計画    → plan.md（スタック・構成・制約）。必要なら plan も grill
(4) 分解    → tasks.md（小さく検証可能な単位）
(5) 実装    → tasks を1つずつ。各タスクで検証
(6) 記録    → 重要判断は ADR、progress を更新
(7) 引き継ぎ→ update-handoff（差分を提示）
```

### コマンド対応表（同じ流れ、呼び方だけ違う）

| ステップ | GitHub Copilot | Claude Code | OpenAI Codex |
| --- | --- | --- | --- |
| (0) 再開 | `/resume-session` | `/resume-session`（または「resume」） | スキル起動 or「resume」、`codex resume` |
| (1) 深掘り | `/grill-me` | `/grill-me`（または AskUserQuestion で自動インタビュー） | grill-me スキル起動 |
| (2) 仕様 | `/speckit.specify` | `/speckit.specify` | `/speckit.specify` |
| (3) 計画 | `/speckit.plan` | `/speckit.plan` | `/speckit.plan` |
| (4) 分解 | `/speckit.tasks` | `/speckit.tasks` | `/speckit.tasks` |
| (5) 実装 | `/speckit.implement` | `/speckit.implement` | `/speckit.implement` |
| (6) 記録 | 「この決定をADRに追記して」 | 同左 | 同左 |
| (7) 引き継ぎ | `/update-handoff` | `/update-handoff` | update-handoff スキル |

### そのまま使えるプロンプト（スラッシュを使わない場合）

**深掘り（grill）:**
```text
このタスクの要件と設計を grill して。1問ずつ、推奨回答を添えて。直前の答えを掘り切ってから次へ。
具体的アクションが可能になったら要約して specs/<NNNN-slug>/spec.md の下書きにして。
```

**仕様→計画→分解（Spec Kit を使わない素の指示でも可）:**
```text
合意した内容で specs/<NNNN-slug>/spec.md を確定して（何を・なぜ・受け入れ条件・スコープ外・E2E検証）。
次に plan.md（スタック・構成・制約）、続いて tasks.md（単独で実装・検証できる小タスク）に分解して。
```

**引き継ぎ更新:**
```text
今回の作業を memory-bank/progress.md に反映（現在地・次の一手・最近の変更に1行）。
重要判断は decisions/ にADR追記。仕様変更は specs/ を更新。ソースは変えず差分だけ見せて。
```

---

## 4. セッション間引き継ぎの詳細

- **確定情報はファイルに**：spec / ADR / progress / architecture。git にある限り永続し、人もレビュー可能。
- **ネイティブ記憶は補助**：Copilot Memory（自動・約28日失効）、Codex memories（`~/.codex/memories/`・
  `memory_summary.md` を次回が最初に読む）、Claude のメモリ。便利だが**失効・確率的**なので、
  重要事項は必ずファイル側に置く。
- **ADR は追記のみ**：決定の変更は過去ADRを消さず supersede（「進化 vs 上書き」問題を回避）。
- **別エージェントへの引き継ぎ**も同じ：相手も AGENTS.md と memory-bank/ を読むので、Copilot で詰めた
  spec を Claude Code で実装、といった横断が成立する。

---

## 5. エージェント別チートシート

**GitHub Copilot（VS Code, agent mode）**
- 背骨: `AGENTS.md` ＋ `.github/copilot-instructions.md`。コマンド: `.github/prompts/*.prompt.md`（`/名前`）。
- ネイティブ: Copilot Memory（設定でON）。クラウド coding agent は MCP が tools 限定。

**Claude Code**
- 背骨: `AGENTS.md`（＋任意 `CLAUDE.md`）。スキル: `.claude/skills/<name>/SKILL.md`（`/名前` で起動）。
- ネイティブ: AskUserQuestion で自動インタビュー、サブエージェントで敵対的レビュー、hook で自動化。

**OpenAI Codex**
- 背骨: `AGENTS.md`（ルートから cwd まで連鎖、`~/.codex/AGENTS.md` が全体）。スキル＋ `memories`。
- セッション継続: `codex resume`。`project_doc_max_bytes` で読み込み量を調整可。

---

## 6. 注意点・既存ツール

- **小さく始める**：いきなり全部入れず、まず `AGENTS.md` ＋ `memory-bank/` ＋ grill-me から。Spec Kit と
  3エージェント併用は後から足せる。
- **指示は短く**：AGENTS.md / constitution が肥大化すると重要指示が埋もれる。「消すと間違えるか？」で取捨。
- **ターンキー代替**：自前構築の代わりに、クロスエージェントSDDハーネス
  [cc-sdd](https://github.com/gotalab/cc-sdd)（Agent Skills で Claude Code/Codex/Copilot ほかに対応）を
  土台にする手もある。本キットの考え方（共有コア＋アダプタ）はそのまま応用できる。
- **コマンド名/配置は版で変わる**：`specify init --help`、各エージェントのスキル/プロンプトの
  最新ドキュメントで確認すること。

---

## 7. 出典

- GitHub Spec Kit … <https://github.com/github/spec-kit> / Microsoft for Developers 解説
- grill-me（Matt Pocock）… <https://www.aihero.dev/my-grill-me-skill-has-gone-viral> / Agent Skills 形式
- OpenAI Codex（AGENTS.md / memories）… <https://developers.openai.com/codex/guides/agents-md> ほか
- GitHub Copilot（AGENTS.md / instructions / Memory）… GitHub Docs
- cc-sdd（クロスエージェントSDDハーネス）… <https://github.com/gotalab/cc-sdd>
