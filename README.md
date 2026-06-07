# Agentic Coding 開発手法ガイド（2026年6月）

LLM・AIエージェントを活用したコーディング開発手法を、Anthropic 公式情報を中心に
体系的にまとめた単一ページのレポート（図解付き・外部依存なしの静的 HTML）です。

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

| ファイル | 役割 |
| --- | --- |
| `index.html` | レポート本体（単一ファイル・外部依存なし） |
| `.nojekyll` | GitHub Pages の Jekyll 処理を無効化（全ファイルをそのまま配信） |
| `.gitignore` | OS・エディタの不要ファイルを除外 |
| `README.md` | このファイル |

---

### 補足
- 初回コミットの author 情報はローカル設定（`git config user.name / user.email`）です。
  変更したい場合は `git commit --amend --reset-author` で付け替えられます。
- 公開リポジトリではコミットに含まれるメールアドレスが公開されます。
  これを避けたい場合は GitHub の noreply アドレスの利用を検討してください。
