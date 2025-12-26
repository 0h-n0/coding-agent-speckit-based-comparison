# Comparison Baseline

このドキュメントでは、コーディングエージェント比較の前提条件と実験環境を定義します。

## Agent Environment

各エージェントの利用環境とプランは以下の通りです。

| Agent | Plan | Model | Release Date | CLI Version | Notes |
|-------|------|-------|--------------|-------------|-------|
| Codex | Plus プラン | gpt-5.2-codex | 2025-12-18 | 0.77.0 | OpenAI Codex CLI |
| Gemini | 無料枠 (Preview) | Gemini 3 Flash | 2025-12-17 | 0.22.2 | Auto モデル選択 |
| Claude Code | MAX $100 プラン | Opus 4.5 | 2025-11-24 | 2.0.76 | Claude Code CLI |

### CLI Installation

```bash
# Codex CLI
npm i -g @openai/codex

# Gemini CLI
npm i -g @google/gemini-cli

# Claude Code
npm i -g @anthropic-ai/claude-code
```

### Model References

- [Introducing GPT-5.2-Codex | OpenAI](https://openai.com/index/introducing-gpt-5-2-codex/)
- [Introducing Gemini 3 Flash | Google](https://blog.google/products/gemini/gemini-3-flash/)
- [Introducing Claude Opus 4.5 | Anthropic](https://www.anthropic.com/news/claude-opus-4-5)

### CLI References

- [@openai/codex - npm](https://www.npmjs.com/package/@openai/codex)
- [@google/gemini-cli - npm](https://www.npmjs.com/package/@google/gemini-cli)
- [@anthropic-ai/claude-code - npm](https://www.npmjs.com/package/@anthropic-ai/claude-code)

## Common Workflow

全エージェントで同一のSpeckit開発ワークフローを使用します。

```text
specify → clarify → plan → tasks → implement
```

### Workflow Details

| Step | Command | Description |
|------|---------|-------------|
| 1 | `/speckit.specify` | 機能仕様の定義 |
| 2 | `/speckit.clarify` | 曖昧な点の明確化（レコメンドされた選択肢を選択） |
| 3 | `/speckit.plan` | 実装計画の策定 |
| 4 | `/speckit.tasks` | タスク分解 |
| 5 | `/speckit.implement` | 実装の実行 |

### Claude Code Specific

Claude Codeのみ、`implement`後にデフォルトで`review`コマンドを実行します。

```text
specify → clarify → plan → tasks → implement → review
```

## Common Constitution

全エージェントに対して以下の共通Constitutionを適用します。

### Technology Stack

| Category | Technology |
|----------|------------|
| Package Manager | uv |
| Type Checking | pydantic |
| Linter | ruff |
| Frontend | React / Next.js (TypeScript) |
| Backend | FastAPI |

### Development Workflow

| Phase | Action |
|-------|--------|
| 機能作成前 | フィーチャーブランチを作成 |
| 機能作成後 | README を更新 |
| コミット後 | Pull Request を作成 |
| マージ前 | レビューを実施 |

## Feature Commands

全エージェントに対して以下の10機能を同一の指示で実装させます。

### F-1: プロジェクト初期構成

```text
speckit.specify "ローカル音声アシスタントのバックエンドとフロントエンドの基本プロジェクト構成を作成したい。FastAPI + React (または Next.js) 構成。"
speckit.clarify
speckit.plan
speckit.tasks
speckit.implement
```

### F-2: STT（ReazonSpeech NeMo v2）

```text
speckit.specify "reazon-research/reazonspeech-nemo-v2 を用いて音声ファイルまたはマイク入力から日本語テキストを生成する STT API を実装したい。"
speckit.clarify
speckit.plan
speckit.tasks
speckit.implement
```

### F-3: LLM（OpenAI gpt-5-mini）

```text
speckit.specify "OpenAI の gpt-5-mini に問い合わせてテキスト応答を生成する LLM サービスを実装したい。"
speckit.clarify
speckit.plan
speckit.tasks
speckit.implement
```

### F-4: TTS（Style-Bert-VITS2）

```text
speckit.specify "Style-Bert-VITS2 を用いて日本語テキストを自然音声に変換する TTS API を実装したい。"
speckit.clarify
speckit.plan
speckit.tasks
speckit.implement
```

### F-5: 会話オーケストレーター

```text
speckit.specify "STT → LLM → TTS を順番に実行し、1つの音声対話を完結させるオーケストレーターを実装したい。"
speckit.clarify
speckit.plan
speckit.tasks
speckit.implement
```

### F-6: 会話履歴管理

```text
speckit.specify "ユーザーとアシスタントの会話履歴を保存・取得できるストレージ機能を実装したい。SQLiteを想定。"
speckit.clarify
speckit.plan
speckit.tasks
speckit.implement
```

### F-7: フロントエンド（ChatGPT風UI）

```text
speckit.specify "ChatGPTのように会話履歴が表示され、音声入力と音声出力ができるWeb UIを実装したい。"
speckit.clarify
speckit.plan
speckit.tasks
speckit.implement
```

### F-8: リアルタイム通信（WebSocket）

```text
speckit.specify "音声入力・文字起こし・応答・音声再生をリアルタイムで表示できるように WebSocket 通信を追加したい。"
speckit.clarify
speckit.plan
speckit.tasks
speckit.implement
```

### F-9: 設定管理

```text
speckit.specify "APIキー、モデルパス、音声デバイスなどを設定ファイルまたは .env で管理できるようにしたい。"
speckit.clarify
speckit.plan
speckit.tasks
speckit.implement
```

### F-10: ローカル起動統合

```text
speckit.specify "ローカルで1コマンドで全サービスが起動するように docker-compose または make コマンドを用意したい。"
speckit.clarify
speckit.plan
speckit.tasks
speckit.implement
```

## Feature Summary

| ID | Feature | Category |
|----|---------|----------|
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

## Evaluation Criteria

各機能の実装結果は以下の観点で評価します。

| Criteria | Description |
|----------|-------------|
| 仕様遵守度 | 仕様書で定義した機能要件の達成率 |
| コード品質 | エラー処理、ログ出力、テストカバレッジ |
| 設計品質 | API設計、モジュール分離、拡張性 |
| ドキュメント | README、コメント、API仕様書 |

---

*Last Updated: 2025-12-26*
