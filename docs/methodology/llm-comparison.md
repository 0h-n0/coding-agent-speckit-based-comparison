# LLM Implementation Comparison

このドキュメントでは、3つのコーディングエージェント（Codex, Gemini, Claude）のLLM（Large Language Model）サービス機能について、仕様書と実装の両面から比較分析を行います。

!!! info "前提条件"
    この比較の実験環境、共通ワークフロー、Constitution については [Comparison Baseline](comparison-baseline.md) を参照してください。

## Overview

各エージェントは同じLLMサービス機能（OpenAI gpt-5-miniを使用したテキスト生成API）を実装することを求められましたが、仕様の定義方法と実装アプローチには顕著な違いがあります。この比較を通じて、各エージェントの設計思想と開発能力を客観的に評価します。

## Comparison Baseline

| Agent | Commit | Repository | Date |
|-------|--------|------------|------|
| Codex | [d77c272](https://github.com/0h-n0/local-voice-assistant-codex/commit/d77c272739e9116c9d220a46e8e14ea547ef5ea2) | local-voice-assistant-codex | 2025-12-26 |
| Gemini | [361573e](https://github.com/0h-n0/local-voice-assistant-gemini/commit/361573e99e5a4d6f4440eb83f90845b9e56258c7) | local-voice-assistant-gemini | 2025-12-26 |
| Claude | [aa6d1e3](https://github.com/0h-n0/local-voice-assistant-claude/commit/aa6d1e374d1088224811491e2430afed2bb3f7e2) | local-voice-assistant-claude | 2025-12-26 |

### LLM Spec Locations

| Agent | Spec Path | Spec Feature ID |
|-------|-----------|-----------------|
| Codex | `specs/001-llm-service/spec.md` | 001-llm-service |
| Gemini | `specs/003-llm-service/spec.md` | 003-llm-service |
| Claude | `specs/003-llm-text-service/spec.md` | 003-llm-text-service |

---

## Specification Comparison

各エージェントが定義したLLM仕様書を比較します。機能要件（FR）と成功基準（SC）の違いを分析します。

### Functional Requirements Comparison

| 要件カテゴリ | Codex | Gemini | Claude |
|--------------|-------|--------|--------|
| チャット/補完エンドポイント | ✅ FR-001 | ✅ FR-001 | ✅ FR-001 |
| OpenAI gpt-5-mini使用 | ✅ FR-002 | ✅ FR-002 | ✅ FR-002 |
| ストリーミング (SSE) | ❌ なし | ✅ FR-003 | ❌ なし |
| チャット履歴管理 | ❌ なし | ✅ FR-004 | ✅ FR-006 |
| パラメータ設定 (temp, max_tokens) | ❌ なし | ✅ FR-005 | ❌ なし |
| ステータスエンドポイント | ❌ なし | ❌ なし | ✅ FR-005 |
| 会話クリア機能 | ❌ なし | ❌ なし | ✅ FR-008 |
| 同時リクエスト制限 | ❌ なし | ❌ なし | ✅ FR-004 |
| 固定システムプロンプト | ❌ なし | ❌ なし | ✅ FR-003 |
| APIバージョニング | ❌ なし | ✅ v1 | ❌ なし |
| エラーハンドリング | ✅ FR-007 | ✅ FR-007 | ✅ FR-007 |
| **機能要件数** | **8件** | **8件** | **9件** |

!!! note "Claudeの特徴"
    Claudeは3エージェント中最も多くの機能要件（9件）を定義しており、会話管理、同時リクエスト制限、ステータス監視など運用面の要件が充実しています。

### Success Criteria Comparison

| 基準 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| レスポンスタイム目標 | 未指定 | 5秒 (SC-001) | 3秒 (SC-001) |
| ストリーミング開始時間 | 未指定 | 1秒 (SC-002) | 未指定 |
| 履歴保持ターン数 | 未指定 | 10ターン (SC-003) | 5ターン (SC-002) |
| 同時リクエスト数 | 未指定 | 未指定 | 5件 (SC-003) |
| API可用性 | 未指定 | 99.9% (SC-004) | 未指定 |

!!! info "パフォーマンス目標の違い"
    GeminiとClaudeはレスポンスタイム目標を明確に定義していますが、Codexは未指定です。Geminiのみがストリーミング開始時間とAPI可用性を定義しています。

詳細な分析については各エージェントのページを参照してください：

- [Codex LLM Analysis](../agents/codex/llm-analysis.md)
- [Gemini LLM Analysis](../agents/gemini/llm-analysis.md)
- [Claude LLM Analysis](../agents/claude/llm-analysis.md)

---

## Implementation Comparison

各エージェントのLLM実装コードを比較します。ファイル構造、APIエンドポイント設計、エラー処理パターン、チャット履歴実装、テストカバレッジの観点から分析します。

### Code Structure

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| API実装ファイル | `backend/app/api/llm.py` | `backend/src/api/v1/endpoints/llm.py` | `backend/src/api/llm.py` |
| サービス層 | `app/services/llm_client.py` | `src/core/llm/service.py` | `src/services/llm_service.py` |
| 実装行数 | 91行 | 174行 | 678行 |
| テスト行数 | 51行 | 119行 | 615行 |
| **合計行数** | **142行** | **293行** | **1293行** |

!!! info "コード量の違い"
    Claudeの実装はCodexの約9倍、Geminiの約4.4倍のコード量があります。これはチャット履歴管理、詳細なエラーハンドリング、包括的なテストコードが含まれているためです。

### API Endpoint Design

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| チャットパス | `POST /llm/complete` | `POST /api/v1/llm/chat` | `POST /api/llm/chat` |
| ストリーミングパス | ❌ なし | `POST /api/v1/llm/chat` (SSE) | ❌ なし |
| ステータスパス | ❌ なし | ❌ なし | `GET /api/llm/status` |
| 会話クリアパス | ❌ なし | ❌ なし | `DELETE /api/llm/conversation/{id}` |
| APIプレフィックス | なし | `/api/v1` | `/api/llm` |
| バージョニング | ❌ なし | ✅ v1 | ❌ なし |

!!! note "APIバージョニング"
    Geminiのみが明示的なAPIバージョニング（/api/v1）を採用しています。Claudeはステータスエンドポイントと会話クリア機能を提供しています。

### Error Handling Pattern

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| エラーレスポンス形式 | `HTTPException` | `HTTPException(status_code, detail)` | `ErrorResponse(error_code, message, details)` |
| エラーコード定義 | HTTP status のみ | HTTP status + detail | Enum (`ErrorCode`) |
| 詳細情報 | ❌ なし | 文字列のみ | ✅ details オブジェクト |
| ログ出力 | ❌ なし | ❌ 未確認 | ✅ 構造化ログ |

### Chat History Implementation

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| 履歴管理 | ❌ なし | ✅ メッセージリスト | ✅ インメモリキャッシュ |
| 永続化 | ❌ なし | ❌ なし | ❌ なし (TTL) |
| ターン制限 | N/A | 10ターン | 5ターン (TTL: 30分) |
| 会話ID | ❌ なし | ✅ あり | ✅ あり |
| クリア機能 | ❌ なし | ❌ なし | ✅ あり |

### Test Coverage

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| テストファイル数 | 1 | 2 | 4 |
| テストケース概算 | 3-5件 | 6-8件 | 15-20件 |
| テストタイプ | 基本的な統合テスト | モック + 統合テスト | コントラクト + ユニットテスト |
| ストリーミングテスト | N/A | ✅ | N/A |
| エラーハンドリングテスト | ⚠️ 部分的 | ✅ | ✅ 詳細 |
| 履歴管理テスト | N/A | ✅ | ✅ 詳細 |

---

## Spec-to-Implementation Alignment

各エージェントが仕様書で定義した要件をどの程度忠実に実装しているかを評価します。

### Codex Alignment

| FR | 要件 | 実装状況 | エビデンス |
|----|------|----------|------------|
| FR-001 | チャット補完エンドポイント | ✅ 実装済み | `complete()` 関数 |
| FR-002 | OpenAI gpt-5-mini使用 | ✅ 実装済み | `llm_client.py` でモデル指定 |
| FR-003 | プロンプト受け取り | ✅ 実装済み | リクエストボディ |
| FR-004 | テキスト応答返却 | ✅ 実装済み | レスポンスモデル |
| FR-005 | 環境変数からAPIキー | ✅ 実装済み | `OPENAI_API_KEY` |
| FR-006 | タイムアウト処理 | ⚠️ 部分的 | デフォルト設定のみ |
| FR-007 | エラーハンドリング | ✅ 実装済み | HTTPException |
| FR-008 | JSON形式応答 | ✅ 実装済み | FastAPI自動変換 |

**達成率**: 7/8 (88%) + 1部分的

### Gemini Alignment

| FR | 要件 | 実装状況 | エビデンス |
|----|------|----------|------------|
| FR-001 | チャットエンドポイント | ✅ 実装済み | `chat()` 関数 |
| FR-002 | OpenAI gpt-5-mini使用 | ✅ 実装済み | `service.py` でモデル指定 |
| FR-003 | SSEストリーミング | ✅ 実装済み | `StreamingResponse` |
| FR-004 | チャット履歴管理 | ✅ 実装済み | メッセージリスト管理 |
| FR-005 | パラメータ設定 | ✅ 実装済み | temperature, max_tokens |
| FR-006 | エラーハンドリング | ✅ 実装済み | HTTPException + 詳細 |
| FR-007 | JSON応答 | ✅ 実装済み | レスポンスモデル |
| FR-008 | APIバージョニング | ✅ 実装済み | `/api/v1` プレフィックス |

**達成率**: 8/8 (100%)

### Claude Alignment

| FR | 要件 | 実装状況 | エビデンス |
|----|------|----------|------------|
| FR-001 | チャットエンドポイント | ✅ 実装済み | `chat()` 関数 |
| FR-002 | OpenAI gpt-5-mini使用 | ✅ 実装済み | `llm_service.py` でモデル指定 |
| FR-003 | 固定システムプロンプト | ✅ 実装済み | `SYSTEM_PROMPT` 定数 |
| FR-004 | 同時リクエスト制限 | ✅ 実装済み | セマフォ実装 |
| FR-005 | ステータスエンドポイント | ✅ 実装済み | `get_status()` 関数 |
| FR-006 | チャット履歴管理 | ✅ 実装済み | TTLキャッシュ |
| FR-007 | エラーハンドリング | ✅ 実装済み | ErrorResponse + ErrorCode |
| FR-008 | 会話クリア機能 | ✅ 実装済み | DELETE エンドポイント |
| FR-009 | JSON形式応答 | ✅ 実装済み | レスポンスモデル |

**達成率**: 9/9 (100%)

### Overall Evaluation

| カテゴリ | Codex | Gemini | Claude |
|----------|-------|--------|--------|
| コード量 | 142行 (最小) | 293行 (中間) | 1293行 (最大) |
| テスト充実度 | 可 | 良 | 優 |
| エラー処理 | 可 | 良 | 優 |
| API設計 | 可 | 良 | 優 |
| 仕様遵守度 | 88% | 100% | 100% |
| **総合評価** | **可** | **良** | **優** |

---

## Key Findings and Conclusions

### Key Findings

#### Codex

- 最もシンプルで軽量な実装（142行）
- 基本的なチャット機能のみ実装
- ストリーミングとチャット履歴は未対応
- 迅速なプロトタイピングに適した設計

#### Gemini

- SSEストリーミングを完全実装
- チャット履歴管理とパラメータ設定をサポート
- APIバージョニング（v1）を採用
- 運用機能とユーザビリティを重視した設計

#### Claude

- 最も包括的な実装（1293行、コード量9倍以上）
- 構造化ログと詳細なエラーハンドリング
- ステータスAPIでサービス状態を監視可能
- 同時リクエスト制限で安定性を確保
- 会話クリア機能でメモリ管理をサポート
- 品質と保守性を重視した設計

### Conclusions

1. **実装品質**: Claude > Gemini > Codex
2. **仕様遵守度**: Gemini (100%) = Claude (100%) > Codex (88%)
3. **機能充実度**: Claude (会話管理+監視) > Gemini (ストリーミング+パラメータ) > Codex (基本機能)
4. **テスト充実度**: Claude > Gemini > Codex

各エージェントは異なるアプローチで実装しており：

- **Codex**: 最小限の実装で素早くデリバリー
- **Gemini**: ストリーミングと柔軟なパラメータ設定を重視
- **Claude**: 品質、監視機能、保守性を重視した堅牢な設計

---

*Comparison Date: 2025-12-26*
*Feature ID: 004-llm-implementation-comparison*
