# Agentic Coding ナレッジベース（2026年6月）

LLM・AIエージェントを活用したコーディング開発手法の調査資料集です（図解付き・外部依存なしの
静的 HTML 6本 ＋ 構築キット2式）。`index.html` がハブページで、各資料への入口になっています。

🔗 **公開URL（Pages 有効化後）:** `https://hiroxtai.github.io/agentic-coding/`

---

## ローカルで確認する

ブラウザで `index.html` をそのまま開くだけで閲覧できます。
簡易サーバーで確認する場合:

```bash
python3 -m http.server 8000
# → http://localhost:8000/ を開く
```

## GitHub Pages で公開する手順

### 1. GitHub に空のリポジトリを作成
GitHub で新規リポジトリ（例: `agentic-coding-guide`）を **Public** で作成します。
※ Private リポジトリで Pages を使うには GitHub Pro 等のプランが必要です。

### 2. ローカルから push する
このフォルダで以下を実行します（`<username>` と `<repo>` は置き換え）:

```bash
git remote add origin https://github.com/<username>/<repo>.git
git push -u origin main
```

### 3. Pages を有効化する
リポジトリの **Settings → Pages** を開き、次を設定して **Save**:

- **Source**: `Deploy from a branch`
- **Branch**: `main` / `(root)`

数十秒〜数分後、`https://<username>.github.io/<repo>/` で公開されます。

## 更新方法

内容を編集したら、コミットして push するだけで自動的に再デプロイされます:

```bash
git add -A
git commit -m "Update report"
git push
```

## ファイル構成

| ファイル / フォルダ | 役割 |
| --- | --- |
| `index.html` | **ハブページ**（5資料の一覧・入口） |
| `coding-agent-methods-2026.html` | ① コーディングエージェント開発手法ガイド（総論） |
| `github-spec-kit-guide.html` | ② GitHub Spec Kit 詳解（仕様駆動） |
| `session-handoff-guide.html` | ③ セッション間引き継ぎ 詳解（汎用） |
| `github-copilot-handoff-guide.html` | ④ GitHub Copilot 引き継ぎ 詳解 |
| `unified-workflow-guide.html` | ⑤ 統合ワークフロー構築手順書（集大成） |
| `hands-on-walkthrough-guide.html` | ⑥ 実践ハンズオン（パスワードリセットの実演） |
| `spec-driven-handoff-kit/` | 統合構成の実ファイル一式（Copilot / Claude / Codex 横断）。手順は `SETUP.md` |
| `copilot-handoff-starter/` | Copilot 向け引き継ぎスターターキット。手順は `SETUP.md` |
| `.nojekyll` / `.gitignore` / `README.md` | Pages 設定 ／ 除外 ／ このファイル |

---

### 補足
- 初回コミットの author 情報はローカル設定（`git config user.name / user.email`）です。
  変更したい場合は `git commit --amend --reset-author` で付け替えられます。
- 公開リポジトリではコミットに含まれるメールアドレスが公開されます。
  これを避けたい場合は GitHub の noreply アドレスの利用を検討してください。
