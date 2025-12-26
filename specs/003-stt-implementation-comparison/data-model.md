# Data Model: STT Implementation Comparison

**Feature**: 003-stt-implementation-comparison
**Date**: 2025-12-26

## Overview

このデータモデルはSTT実装比較ドキュメントの構造を定義する。
実際のデータベースやAPIは存在せず、MkDocsドキュメントのコンテンツ構造を表す。

## Entities

### AgentSTTSpec

各エージェントのSTT仕様書データを表す。

| Field | Type | Description |
|-------|------|-------------|
| agent_id | string | エージェント識別子 (codex, gemini, claude) |
| spec_path | string | 仕様書ファイルパス |
| feature_id | string | フィーチャーID (例: 001-stt-api) |
| commit_hash | string | 参照コミットハッシュ |
| functional_requirements | FunctionalRequirement[] | 機能要件リスト |
| success_criteria | SuccessCriterion[] | 成功基準リスト |
| user_stories | UserStory[] | ユーザーストーリーリスト |

### FunctionalRequirement

個々の機能要件を表す。

| Field | Type | Description |
|-------|------|-------------|
| id | string | 要件ID (例: FR-001) |
| description | string | 要件説明 |
| category | string | カテゴリ (file_upload, streaming, auth, etc.) |

### SuccessCriterion

個々の成功基準を表す。

| Field | Type | Description |
|-------|------|-------------|
| id | string | 基準ID (例: SC-001) |
| description | string | 基準説明 |
| metric_type | string | メトリクスタイプ (latency, accuracy, concurrency, etc.) |
| target_value | string | 目標値 (例: "5秒", "10%") |

### AgentSTTImpl

各エージェントのSTT実装データを表す。

| Field | Type | Description |
|-------|------|-------------|
| agent_id | string | エージェント識別子 |
| api_file_path | string | APIエンドポイント実装ファイルパス |
| service_file_path | string | サービス層ファイルパス |
| total_lines | integer | 総行数 |
| test_lines | integer | テストコード行数 |
| endpoints | Endpoint[] | エンドポイント一覧 |
| error_handling | ErrorHandlingPattern | エラー処理パターン |
| websocket_impl | WebSocketPattern | WebSocket実装パターン |

### Endpoint

個々のAPIエンドポイントを表す。

| Field | Type | Description |
|-------|------|-------------|
| method | string | HTTPメソッド (POST, GET, WS) |
| path | string | エンドポイントパス |
| description | string | エンドポイント説明 |
| parameters | string[] | パラメータ一覧 |
| response_model | string | レスポンスモデル名 |

### ErrorHandlingPattern

エラーハンドリングパターンを表す。

| Field | Type | Description |
|-------|------|-------------|
| response_format | string | レスポンス形式 |
| error_code_type | string | エラーコード定義方式 (string, enum) |
| has_details | boolean | 詳細情報の有無 |
| has_logging | boolean | ログ出力の有無 |

### WebSocketPattern

WebSocket実装パターンを表す。

| Field | Type | Description |
|-------|------|-------------|
| protocol | string | プロトコル (binary, json, mixed) |
| termination_signal | string | 終了シグナル方式 |
| has_partial_results | boolean | 中間結果の有無 |
| buffer_strategy | string | バッファリング戦略 |

### AlignmentResult

仕様-実装整合性チェック結果を表す。

| Field | Type | Description |
|-------|------|-------------|
| agent_id | string | エージェント識別子 |
| requirement_id | string | 要件ID |
| status | string | 状態 (implemented, partial, not_implemented) |
| evidence | string | エビデンス（コード参照など） |
| notes | string | 備考 |

### ComparisonSummary

全体比較サマリーを表す。

| Field | Type | Description |
|-------|------|-------------|
| comparison_date | date | 比較実施日 |
| agents | string[] | 比較対象エージェント一覧 |
| spec_comparison | CategoryScore[] | 仕様書比較スコア |
| impl_comparison | CategoryScore[] | 実装比較スコア |
| alignment_scores | AlignmentScore[] | 仕様遵守度スコア |

### CategoryScore

カテゴリ別スコアを表す。

| Field | Type | Description |
|-------|------|-------------|
| category | string | カテゴリ名 |
| codex_score | string | Codexスコア (優/良/可/不足) |
| gemini_score | string | Geminiスコア |
| claude_score | string | Claudeスコア |

### AlignmentScore

仕様遵守度スコアを表す。

| Field | Type | Description |
|-------|------|-------------|
| agent_id | string | エージェント識別子 |
| total_requirements | integer | 総要件数 |
| implemented | integer | 実装済み数 |
| partial | integer | 部分実装数 |
| not_implemented | integer | 未実装数 |
| alignment_rate | float | 達成率 (%) |

## Relationships

```text
AgentSTTSpec
  └── contains → FunctionalRequirement[]
  └── contains → SuccessCriterion[]
  └── contains → UserStory[]

AgentSTTImpl
  └── contains → Endpoint[]
  └── contains → ErrorHandlingPattern
  └── contains → WebSocketPattern

AlignmentResult
  └── references → AgentSTTSpec.agent_id
  └── references → FunctionalRequirement.id

ComparisonSummary
  └── aggregates → CategoryScore[]
  └── aggregates → AlignmentScore[]
```

## Notes

- このデータモデルは静的ドキュメントの論理構造を表す
- 実際のデータはMarkdownファイル内のテーブルとして表現される
- 比較データは手動（Claude Codeによる分析）で生成される
