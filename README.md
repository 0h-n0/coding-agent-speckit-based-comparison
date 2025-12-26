# Coding Agent Comparison

コーディングエージェントを比較・評価するためのフレームワークです。

## ドキュメント

📚 **[ドキュメントサイト](https://0h-n0.github.io/coding-agent-speckit-based-comparison/)**

## 概要

異なるAIコーディングエージェントが生成するドキュメントの品質を比較・評価します。

### 対応エージェント

| エージェント | 提供元 | プラン | モデル | リリース日 |
|-------------|--------|--------|--------|------------|
| Codex | OpenAI | Plus プラン | gpt-5.2-codex | 2025-12-18 |
| Gemini | Google | 無料枠 (Preview) | Gemini 3 Flash (Auto) | 2025-12-17 |
| Claude | Anthropic | MAX $100 プラン | Opus 4.5 | 2025-11-24 |

### 比較機能

| ID | 機能 | カテゴリ |
|----|------|----------|
| F-1 | プロジェクト初期構成 | Setup |
| F-2 | STT (ReazonSpeech NeMo v2) | Core |
| F-3 | LLM (OpenAI gpt-5-mini) | Core |
| F-4 | TTS (Style-Bert-VITS2) | Core |
| F-5 | 会話オーケストレーター | Integration |
| F-6 | 会話履歴管理 | Data |
| F-7 | フロントエンド (ChatGPT風UI) | UI |
| F-8 | リアルタイム通信 (WebSocket) | Communication |
| F-9 | 設定管理 | Configuration |
| F-10 | ローカル起動統合 | DevOps |

### 共通Constitution

全エージェントに対して以下の共通設定を適用:

- **パッケージ管理**: uv
- **型チェック**: pydantic
- **リンター**: ruff
- **フロントエンド**: React / Next.js (TypeScript)
- **バックエンド**: FastAPI

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
