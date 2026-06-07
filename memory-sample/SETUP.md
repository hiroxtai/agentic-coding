# GitHub Copilot セッション間引き継ぎ ― 構築キット（SETUP）

このキットを自分のリポジトリにコピーすれば、**「自動の Copilot Memory」＋「確定の引き継ぎファイル」**
の二本立て環境がすぐ作れます。まずこの SETUP.md を読み、続いて各ファイルをコピーしてください。

---

## 0. 何を作るのか（ファイル構成）

```text
あなたのリポジトリ/
├─ AGENTS.md                      ← エージェントへの指示の“本体”（必ず守らせる規約＋引き継ぎ手順）
├─ .github/
│   ├─ copilot-instructions.md    ← Copilot が毎回読む。中身は AGENTS.md を参照するだけ
│   ├─ instructions/
│   │   └─ typescript.instructions.md   ← パス別の指示（任意・モノレポ向けの例）
│   └─ prompts/
│       ├─ resume-session.prompt.md     ← /resume-session で呼ぶ「再開」コマンド
│       └─ update-handoff.prompt.md     ← /update-handoff で呼ぶ「引き継ぎ更新」コマンド
└─ memory-bank/                   ← 引き継ぎの“記憶”。人もエージェントも読み書きする
    ├─ progress.md                ← 心臓部：現在地・次の一手・最近の変更
    ├─ architecture.md            ← 変わりにくい“地図”
    └─ decisions/
        └─ 0001-record-architecture-decisions.md   ← ADR（決定記録）兼テンプレート
```

役割分担を一言で：

| 層 | 何 | 性質 |
| --- | --- | --- |
| 自動の記憶 | **Copilot Memory**（設定でON） | 自動・リポジトリ単位・約28日で失効 |
| 確定の規約 | `AGENTS.md` ＋ `.github/copilot-instructions.md` | 手動・恒久・git管理・必ず適用 |
| 確定の引き継ぎ | `memory-bank/`（progress / architecture / decisions） | 人が意図的に残す。git で永続 |
| 定型操作 | `.github/prompts/*.prompt.md` | `/コマンド` で呼ぶ |

---

## 1. 導入手順（5ステップ）

### ステップ0：Copilot Memory を有効化（自動の土台）

GitHub の **Settings → Copilot** で **Memory** をオンにします（Pro/Pro+ は既定ON）。
これだけで、規約・アーキ・横断依存をエージェントが自動学習し、次セッションへ引き継ぎ始めます。
※ Memory はリポジトリ単位・約28日で失効・公開プレビュー。**確定情報は必ず下記ファイルに**置きます。

### ステップ1：ファイルをコピー

このキットの `AGENTS.md` / `.github/` / `memory-bank/` を、**あなたのリポジトリのルート**に
そのままコピーします。

### ステップ2：プレースホルダを埋める

`AGENTS.md` の `<...>` を自分のプロジェクトに合わせて記入します（プロジェクト概要、ビルド/テスト
コマンド、コード規約、やってはいけないこと）。ここが**最も重要**です。`memory-bank/progress.md` と
`architecture.md` の `<...>` も現状で埋めます（最初はざっくりでOK。育てていく）。

### ステップ3：コミット

```bash
git add AGENTS.md .github memory-bank
git commit -m "Add Copilot session-handoff system"
```

これでチーム全員（と全エージェント）に共有されます。

### ステップ4：動作確認

VS Code の Copilot Chat（Agent mode）で `/resume-session` と打ち、エージェントが
`memory-bank/` を読んで現在地を要約すれば成功です。

---

## 2. 日々の使い方（運用サイクル）

毎セッションを次の循環で回します。**開始＝読む / 終了＝書く** を徹底するのがコツです。

```text
① 開始   →  /resume-session   （引き継ぎを読んで現在地を復元・確認）
② 作業   →  ふつうに指示して実装（重要な判断が出たらADRを追記させる）
③ 終了   →  /update-handoff   （今回の作業を progress.md / decisions に蒸留）
④ 次回   →  新しいセッションで ① に戻る（記憶はファイルに残っている）
```

### そのまま使えるプロンプト例

**A. セッション開始（再開）** — チャットにこう打つ：

```text
/resume-session
```

スラッシュコマンドを使わない場合は、同じことを普通の文で：

```text
memory-bank/progress.md と architecture.md、decisions/ の最新を読んで、
現在地と次の一手を3〜5個に要約して。コードと照合して食い違いがあれば指摘して。
変更する前に確認を取って。
```

**B. 作業中に決定を記録させる**：

```text
いま決めた「<決定内容>」を memory-bank/decisions/ に新しいADRとして追記して。
既存のADRをテンプレートにして、過去のADRは編集しないで。
```

**C. セッション終了（引き継ぎ更新）** — チャットにこう打つ：

```text
/update-handoff
```

または普通の文で：

```text
今回やったことを memory-bank/progress.md に反映して（現在の状態・次の一手・最近の変更に1行追記）。
重要な判断があれば decisions/ にADRを追記。アーキが変われば architecture.md も更新。
ソースコードは変えず、引き継ぎファイルの差分だけ見せて。
```

**D. クラウドの coding agent（Issue→PR）に任せるとき** — Issue 本文や指示にこう書く：

```text
着手前に AGENTS.md と memory-bank/progress.md・architecture.md を読むこと。
実装後、memory-bank/progress.md を更新し、重要な判断は decisions/ にADRとして残すこと。
```

---

## 3. 注意点

- **Copilot Memory は揮発的**（自動・約28日失効・プレビュー）。恒久的に守らせたい規約・決定は
  必ず `AGENTS.md` と `memory-bank/` に書く。両者は競合せず補完関係。
- **クラウドの coding agent は MCP が tools 限定**（resources/prompts 非対応、リモートOAuth不可）。
  恒久情報はリポジトリのファイルに置くこと。
- `AGENTS.md` / `copilot-instructions.md` は**短く保つ**。肥大化すると重要な指示が埋もれて無視される。
  「この行を消すとエージェントが間違えるか？」がNoなら削る。
- 複数リポジトリ/ツールをまたいで記憶を共有したい場合のみ、Agent mode に
  **OpenMemory MCP** などの外部メモリを追加で重ねる（応用）。

---

## 4. なぜこの構成なのか（設計意図）

- **AGENTS.md を本体**にし、`copilot-instructions.md` はそれを参照するだけにすることで、
  二重管理を避けつつ、Copilot 以外のツール（Claude Code 等）でも同じ規約が効く。
- **memory-bank をファイルで持つ**ことで、Memory（自動・失効あり）と違い、
  重要な引き継ぎが git に永続し、人もレビューできる。
- **ADR を append-only** にすることで、「進化 vs 上書き」問題（決定の経緯が消える）を防ぐ。
- **prompt files でコマンド化**することで、「開始＝読む / 終了＝書く」が形骸化せず毎回実行される。
