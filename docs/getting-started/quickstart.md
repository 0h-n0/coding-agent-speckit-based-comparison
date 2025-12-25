# クイックスタート

このガイドでは、Coding Agent Comparison を使い始めるための最初のステップを説明します。

## 概要

Coding Agent Comparison は、異なる AI コーディングエージェントが生成する
ドキュメントの品質を比較・評価するためのフレームワークです。

## 基本的な使い方

### 1. プロジェクトのセットアップ

まず、[インストール](installation.md) の手順に従ってプロジェクトをセットアップしてください。

### 2. サブモジュールの確認

各エージェントの実装を確認します：

```bash
# サブモジュールの最新化
git submodule update --remote --merge

# 各エージェントのディレクトリを確認
ls submodules/
```

### 3. ドキュメントの閲覧

ローカルでドキュメントを確認するには：

```bash
mkdocs serve
```

ブラウザで `http://127.0.0.1:8000` を開きます。

### 4. エージェント別ドキュメントの確認

各エージェントの詳細は以下のページで確認できます：

- [Codex](../agents/codex/index.md) - OpenAI のコード生成モデル
- [Gemini](../agents/gemini/index.md) - Google のマルチモーダル AI
- [Claude](../agents/claude/index.md) - Anthropic の会話型 AI

## ドキュメント構成

```
docs/
├── index.md              # ホームページ
├── getting-started/      # 入門ガイド
├── features/             # 機能説明
├── agents/               # エージェント別ドキュメント
│   ├── codex/
│   ├── gemini/
│   └── claude/
├── methodology/          # 評価方法論
├── development/          # 開発者向け情報
└── reference/            # リファレンス
```

## 次のステップ

- [機能](../features/index.md) - プロジェクトの機能を詳しく見る
- [評価基準](../methodology/evaluation-criteria.md) - 評価方法を理解する
- [貢献ガイド](../development/contributing.md) - プロジェクトに貢献する
