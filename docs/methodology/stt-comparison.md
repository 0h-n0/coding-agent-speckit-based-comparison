# STT Implementation Comparison

このドキュメントでは、3つのコーディングエージェント（Codex, Gemini, Claude）のSTT（Speech-to-Text）機能について、仕様書と実装の両面から比較分析を行います。

## Overview

各エージェントは同じ音声認識機能を実装することを求められましたが、仕様の定義方法と実装アプローチには顕著な違いがあります。この比較を通じて、各エージェントの設計思想と開発能力を客観的に評価します。

## Comparison Baseline

| Agent | Commit | Repository | Date |
|-------|--------|------------|------|
| Codex | [d77c272](https://github.com/0h-n0/local-voice-assistant-codex/commit/d77c272739e9116c9d220a46e8e14ea547ef5ea2) | local-voice-assistant-codex | 2025-12-26 |
| Gemini | [361573e](https://github.com/0h-n0/local-voice-assistant-gemini/commit/361573e99e5a4d6f4440eb83f90845b9e56258c7) | local-voice-assistant-gemini | 2025-12-26 |
| Claude | [aa6d1e3](https://github.com/0h-n0/local-voice-assistant-claude/commit/aa6d1e374d1088224811491e2430afed2bb3f7e2) | local-voice-assistant-claude | 2025-12-26 |

### STT Spec Locations

| Agent | Spec Path | Spec Feature ID |
|-------|-----------|-----------------|
| Codex | `specs/001-stt-api/spec.md` | 001-stt-api |
| Gemini | `specs/002-stt-japanese-api/spec.md` | 002-stt-japanese-api |
| Claude | `specs/002-stt-api/spec.md` | 002-stt-api |

---

## Specification Comparison

各エージェントが定義したSTT仕様書を比較します。機能要件（FR）と成功基準（SC）の違いを分析します。

### Functional Requirements Comparison

| 要件カテゴリ | Codex | Gemini | Claude |
|--------------|-------|--------|--------|
| ファイル変換エンドポイント | :white_check_mark: FR-001 | :white_check_mark: FR-001 | :white_check_mark: FR-001 |
| サポート形式 | WAV, FLAC, MP3 (FR-009) | WAV, MP3 (FR-002) | WAV, MP3, FLAC, OGG (FR-001) |
| WebSocketストリーミング | :white_check_mark: FR-002 | :white_check_mark: FR-004 | :white_check_mark: FR-006 |
| 認証要件 | :x: なし (FR-008) | :white_check_mark: API Key (FR-009) | :x: なし (CORS制限) |
| ファイルサイズ制限 | 60秒 (FR-006) | 50MB (FR-008) | 100MB (FR-005) |
| ヘルスチェック | :white_check_mark: FR-004 | :x: 未指定 | :white_check_mark: FR-007 |
| エラーハンドリング | :white_check_mark: FR-005 | :white_check_mark: FR-007 | :white_check_mark: FR-009 |
| Rate Limiting | :x: 未指定 | :white_check_mark: FR-012 | :x: 未指定 |
| Observability (Logging/Metrics) | :x: 未指定 | :white_check_mark: FR-010, FR-011 | :x: 未指定 |
| **機能要件数** | **9件** | **12件** | **9件** |

!!! note "Geminiの特徴"
    Geminiは3エージェント中最も多くの機能要件（12件）を定義しており、認証、Rate Limiting、Observabilityなど運用面の要件が充実しています。

### Success Criteria Comparison

| 基準 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| レイテンシ目標 | 10秒/10秒音声 (SC-002) | 5秒/30秒音声 (SC-001) | 5秒/10秒音声 (SC-001) |
| ストリーミング遅延 | 未指定 | 500ms (SC-002) | 500ms (SC-004) |
| 精度目標 (WER/CER) | 未指定 | WER 10-15% (SC-001, SC-002) | CER 10% (SC-005) |
| 同時リクエスト | 未指定 | 5件 (SC-003) | 10件 (SC-003) |
| 長時間ストリーミング | 未指定 | 1時間 (SC-004) | 未指定 |
| 統合時間 | 30分 (SC-004) | 未指定 | 未指定 |

!!! info "パフォーマンス目標の違い"
    GeminiとClaudeはストリーミング遅延500msを明確に定義していますが、Codexは未指定です。一方、Codexのみが開発者向けの「統合時間30分」という基準を設けています。

詳細な分析については各エージェントのページを参照してください：

- [Codex STT Analysis](../agents/codex/stt-analysis.md)
- [Gemini STT Analysis](../agents/gemini/stt-analysis.md)
- [Claude STT Analysis](../agents/claude/stt-analysis.md)

---

## Implementation Comparison

各エージェントのSTT実装コードを比較します。ファイル構造、APIエンドポイント設計、エラー処理パターン、WebSocket実装、テストカバレッジの観点から分析します。

### Code Structure

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| API実装ファイル | `backend/app/api/stt.py` | `backend/src/api/v1/endpoints/stt.py` | `backend/src/api/stt.py` |
| サービス層 | `app/services/transcription.py` | `src/core/stt_processor.py` | `src/services/stt_service.py` |
| 総行数 (API+Service) | 113行 | 153行 | 661行 |
| テスト行数 | 29行 | 259行 | 384行 |
| **合計行数** | **142行** | **412行** | **1045行** |

!!! info "コード量の違い"
    Claudeの実装はCodexの約7倍、Geminiの約2.5倍のコード量があります。これは構造化ログ、詳細なエラーハンドリング、包括的なテストコードが含まれているためです。

### API Endpoint Design

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| ファイル変換パス | `POST /stt/file` | `POST /api/v1/transcribe/file` | `POST /api/stt/transcribe` |
| ストリーミングパス | `WS /stt/stream` | `WS /api/v1/transcribe/stream` | `WS /api/stt/stream` |
| ステータスパス | :x: なし | :x: なし | `GET /api/stt/status` |
| APIプレフィックス | なし | `/api/v1` | `/api/stt` |
| バージョニング | :x: なし | :white_check_mark: v1 | :x: なし |

!!! note "APIバージョニング"
    Geminiのみが明示的なAPIバージョニング（/api/v1）を採用しています。Claudeはステータスエンドポイントを提供し、モデルの状態を監視可能にしています。

### Error Handling Pattern

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| エラーレスポンス形式 | `ErrorResponse(code, message)` | `HTTPException(status_code, detail)` | `ErrorResponse(error_code, message, details)` |
| エラーコード定義 | 文字列 (`unsupported_audio_format`) | 文字列 (detail内) | Enum (`ErrorCode.UNSUPPORTED_FORMAT`) |
| 詳細情報 | :x: なし | :x: なし | :white_check_mark: details オブジェクト |
| ログ出力 | :x: なし | :x: 未確認 | :white_check_mark: 構造化ログ |

### WebSocket Implementation

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| プロトコル | バイナリ + JSON制御 | バイナリのみ | バイナリ (PCM) |
| 終了シグナル | `{"event": "end"}` | クライアント切断 | クライアント切断 |
| 中間結果 | :x: なし | :white_check_mark: `is_final` フラグ | :white_check_mark: `partial`/`final` type |
| バッファリング | フレームリスト | async generator | `AudioBuffer` クラス |

!!! tip "中間結果のサポート"
    GeminiとClaudeはリアルタイム音声認識で中間結果を返却できます。Codexは最終結果のみを返却します。

### Test Coverage

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| テストファイル数 | 2 | 2 | 4 |
| テストケース概算 | 2-3件 | 6-8件 | 10-15件 |
| テストタイプ | 基本的な統合テスト | モック + 統合テスト | コントラクト + ユニットテスト |
| API Keyテスト | :x: | :white_check_mark: | :x: (認証なし) |
| ファイルサイズテスト | :x: | :white_check_mark: | :white_check_mark: |
| Rate Limitテスト | :x: | :white_check_mark: (モック) | :x: (Rate Limitなし) |

---

## Spec-to-Implementation Alignment

各エージェントが仕様書で定義した要件をどの程度忠実に実装しているかを評価します。

### Codex Alignment

| FR | 要件 | 実装状況 | エビデンス |
|----|------|----------|------------|
| FR-001 | 音声ファイル受付 | :white_check_mark: 実装済み | `transcribe_file()` 関数 |
| FR-002 | WebSocketストリーミング | :white_check_mark: 実装済み | `stt_stream()` 関数 |
| FR-003 | フォーマット検証 | :warning: 部分的 | `audio_format` パラメータで受け取るが検証は簡易 |
| FR-004 | ヘルスエンドポイント | :x: 未実装 | STT APIにはなし |
| FR-005 | エラーレスポンス | :white_check_mark: 実装済み | `ErrorResponse` スキーマ |
| FR-006 | 60秒制限 | :x: 未実装 | 制限ロジックなし |
| FR-007 | プレーンテキスト返却 | :white_check_mark: 実装済み | `TranscriptionResult.text` |
| FR-008 | 認証なし | :white_check_mark: 実装済み | 認証コードなし |
| FR-009 | WAV/FLAC/MP3サポート | :warning: 部分的 | 形式は受け取るが実際の変換はサービス依存 |

**達成率**: 6/9 (67%) + 2部分的

### Gemini Alignment

| FR | 要件 | 実装状況 | エビデンス |
|----|------|----------|------------|
| FR-001 | /transcribe/file エンドポイント | :white_check_mark: 実装済み | `transcribe_file()` 関数 |
| FR-002 | WAV/MP3サポート | :white_check_mark: 実装済み | `content_type` 検証 |
| FR-003 | JSON返却 | :white_check_mark: 実装済み | `TranscriptionResult` モデル |
| FR-004 | /transcribe/stream エンドポイント | :white_check_mark: 実装済み | `transcribe_stream()` 関数 |
| FR-005 | タイムスタンプ付き結果 | :white_check_mark: 実装済み | `start_timestamp`, `end_timestamp` |
| FR-006 | reazonspeech-nemo-v2使用 | :warning: 部分的 | モック実装、実際のモデルは未確認 |
| FR-007 | エラーハンドリング | :white_check_mark: 実装済み | `HTTPException` 使用 |
| FR-008 | 50MB制限 | :white_check_mark: 実装済み | `MAX_FILE_SIZE_BYTES` 定数 |
| FR-009 | API Key認証 | :white_check_mark: 実装済み | `get_api_key` 依存性 |
| FR-010 | 構造化ログ | :warning: 部分的 | ログ設定は存在するが詳細未確認 |
| FR-011 | Prometheusメトリクス | :warning: 部分的 | `middlewares/metrics.py` 存在 |
| FR-012 | Rate Limiting | :white_check_mark: 実装済み | `rate_limit_dependency` |

**達成率**: 9/12 (75%) + 3部分的

### Claude Alignment

| FR | 要件 | 実装状況 | エビデンス |
|----|------|----------|------------|
| FR-001 | 音声ファイル変換 (WAV/MP3/FLAC/OGG) | :white_check_mark: 実装済み | `validate_audio_format()` 関数 |
| FR-002 | reazonspeech-nemo-v2使用 | :warning: 部分的 | サービス層で実装（モック可能性あり） |
| FR-003 | テキスト+処理時間+長さ返却 | :white_check_mark: 実装済み | `TranscriptionResponse` モデル |
| FR-004 | 16kHzリサンプリング | :warning: 部分的 | サービス層で処理 |
| FR-005 | 100MB制限 | :white_check_mark: 実装済み | `MAX_FILE_SIZE` 定数 |
| FR-006 | WebSocketストリーミング | :white_check_mark: 実装済み | `stream_transcribe()` 関数 |
| FR-007 | ステータスエンドポイント | :white_check_mark: 実装済み | `get_status()` 関数 |
| FR-008 | ローカル処理のみ | :white_check_mark: 実装済み | 外部API呼び出しなし |
| FR-009 | 構造化エラーレスポンス | :white_check_mark: 実装済み | `ErrorResponse` + `ErrorCode` Enum |

**達成率**: 7/9 (78%) + 2部分的

### Overall Evaluation

| カテゴリ | Codex | Gemini | Claude |
|----------|-------|--------|--------|
| コード量 | 142行 (最小) | 412行 (中間) | 1045行 (最大) |
| テスト充実度 | 可 | 良 | 優 |
| エラー処理 | 可 | 良 | 優 |
| API設計 | 可 | 良 | 優 |
| 仕様遵守度 | 67% | 75% | 78% |
| **総合評価** | **可** | **良** | **優** |

---

## Key Findings and Conclusions

### Key Findings

#### Codex

- 最もシンプルで軽量な実装（142行）
- テストカバレッジが最小限
- 仕様に定義された制限（60秒）が未実装
- 開発速度を重視した設計

#### Gemini

- API Key認証とRate Limitingを実装
- Prometheusメトリクスの基盤あり
- APIバージョニング（v1）を採用
- 運用機能を重視した設計

#### Claude

- 最も包括的な実装（1045行、コード量3倍以上）
- 構造化ログと詳細なエラーハンドリング
- ステータスAPIでモデル状態を監視可能
- コントラクトテストとユニットテストの分離
- 品質と保守性を重視した設計

### Conclusions

1. **実装品質**: Claude > Gemini > Codex
2. **仕様遵守度**: Claude (78%) > Gemini (75%) > Codex (67%)
3. **運用機能**: Gemini (認証+Rate Limit+Metrics) > Claude (ステータス+ログ) > Codex (最小限)
4. **テスト充実度**: Claude > Gemini > Codex

各エージェントは異なるアプローチで実装しており：

- **Codex**: 最小限の実装で素早くデリバリー
- **Gemini**: 運用機能を重視した本番運用志向
- **Claude**: 品質と保守性を重視した堅牢な設計

---

*Comparison Date: 2025-12-26*
*Feature ID: 003-stt-implementation-comparison*
