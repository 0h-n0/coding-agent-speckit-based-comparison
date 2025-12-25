# Coding Agent Comparison

コーディングエージェントを比較・評価するためのフレームワークです。

## ドキュメント

📚 **[ドキュメントサイト](https://0h-n0.github.io/coding-agent-speckit-based-comparison/)**

## 概要

異なるAIコーディングエージェントが生成するドキュメントの品質を比較・評価します。

### 対応エージェント

| エージェント | 提供元 | 説明 |
|-------------|--------|------|
| Codex | OpenAI | コード生成に特化したモデル |
| Gemini | Google | マルチモーダルAIモデル |
| Claude | Anthropic | 会話型AIアシスタント |

## クイックスタート

```bash
# リポジトリのクローン
git clone https://github.com/0h-n0/coding-agent-speckit-based-comparison.git
cd coding-agent-speckit-based-comparison

# サブモジュールの初期化
git submodule update --init --recursive

# ドキュメントのローカルプレビュー
pip install mkdocs-material
mkdocs serve
```

ブラウザで http://127.0.0.1:8000 を開いてください。

## プロジェクト構成

```
.
├── docs/                # MkDocsドキュメント
├── submodules/          # エージェント実装（サブモジュール）
│   ├── codex/
│   ├── gemini/
│   └── claude/
├── specs/               # 機能仕様
├── .specify/            # Speckit設定
└── mkdocs.yml           # MkDocs設定
```

## ライセンス

MIT License
