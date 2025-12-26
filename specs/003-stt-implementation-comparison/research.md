# Research: STT Implementation Comparison

**Feature**: 003-stt-implementation-comparison
**Date**: 2025-12-26

## Executive Summary

3つのコーディングエージェント（Codex, Gemini, Claude）のSTT実装を比較分析した結果をまとめる。
各エージェントの仕様書定義と実装コードを照合し、設計アプローチの違いと実装品質を評価する。

## Comparison Baseline

| Agent | Commit | Repository | Date |
|-------|--------|------------|------|
| Codex | [d77c272](https://github.com/0h-n0/local-voice-assistant-codex/commit/d77c272739e9116c9d220a46e8e14ea547ef5ea2) | local-voice-assistant-codex | 2025-12-26 |
| Gemini | [361573e](https://github.com/0h-n0/local-voice-assistant-gemini/commit/361573e99e5a4d6f4440eb83f90845b9e56258c7) | local-voice-assistant-gemini | 2025-12-26 |
| Claude | [aa6d1e3](https://github.com/0h-n0/local-voice-assistant-claude/commit/aa6d1e374d1088224811491e2430afed2bb3f7e2) | local-voice-assistant-claude | 2025-12-26 |

## Specification Comparison

### STT Spec Locations

| Agent | Spec Path | Spec Feature ID |
|-------|-----------|-----------------|
| Codex | `specs/001-stt-api/spec.md` | 001-stt-api |
| Gemini | `specs/002-stt-japanese-api/spec.md` | 002-stt-japanese-api |
| Claude | `specs/002-stt-api/spec.md` | 002-stt-api |

### Functional Requirements Comparison

| 要件カテゴリ | Codex | Gemini | Claude |
|--------------|-------|--------|--------|
| ファイル変換エンドポイント | ✅ FR-001 | ✅ FR-001 | ✅ FR-001 |
| サポート形式 | WAV, FLAC, MP3 (FR-009) | WAV, MP3 (FR-002) | WAV, MP3, FLAC, OGG (FR-001) |
| WebSocketストリーミング | ✅ FR-002 | ✅ FR-004 | ✅ FR-006 |
| 認証要件 | ❌ なし (FR-008) | ✅ API Key (FR-009) | ❌ なし (CORS制限) |
| ファイルサイズ制限 | 60秒 (FR-006) | 50MB (FR-008) | 100MB (FR-005) |
| ヘルスチェック | ✅ FR-004 | ❌ 未指定 | ✅ FR-007 |
| エラーハンドリング | ✅ FR-005 | ✅ FR-007 | ✅ FR-009 |
| Rate Limiting | ❌ 未指定 | ✅ FR-012 | ❌ 未指定 |
| Observability (Logging/Metrics) | ❌ 未指定 | ✅ FR-010, FR-011 | ❌ 未指定 |
| 機能要件数 | 9件 | 12件 | 9件 |

### Success Criteria Comparison

| 基準 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| レイテンシ目標 | 10秒/10秒音声 (SC-002) | 5秒/30秒音声 (SC-001) | 5秒/10秒音声 (SC-001) |
| ストリーミング遅延 | 未指定 | 500ms (SC-002) | 500ms (SC-004) |
| 精度目標 (WER/CER) | 未指定 | WER 10-15% (SC-001, SC-002) | CER 10% (SC-005) |
| 同時リクエスト | 未指定 | 5件 (SC-003) | 10件 (SC-003) |
| 長時間ストリーミング | 未指定 | 1時間 (SC-004) | 未指定 |
| 統合時間 | 30分 (SC-004) | 未指定 | 未指定 |

## Implementation Comparison

### Code Structure

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| API実装ファイル | `backend/app/api/stt.py` | `backend/src/api/v1/endpoints/stt.py` | `backend/src/api/stt.py` |
| サービス層 | `app/services/transcription.py` | `src/core/stt_processor.py` | `src/services/stt_service.py` |
| 総行数 (API+Service) | 113行 | 153行 | 661行 |
| テスト行数 | 29行 | 259行 | 384行 |
| **合計行数** | **142行** | **412行** | **1045行** |

### API Endpoint Design

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| ファイル変換パス | `POST /stt/file` | `POST /api/v1/transcribe/file` | `POST /api/stt/transcribe` |
| ストリーミングパス | `WS /stt/stream` | `WS /api/v1/transcribe/stream` | `WS /api/stt/stream` |
| ステータスパス | ❌ なし | ❌ なし | `GET /api/stt/status` |
| APIプレフィックス | なし | `/api/v1` | `/api/stt` |
| バージョニング | ❌ なし | ✅ v1 | ❌ なし |

### Error Handling Pattern

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| エラーレスポンス形式 | `ErrorResponse(code, message)` | `HTTPException(status_code, detail)` | `ErrorResponse(error_code, message, details)` |
| エラーコード定義 | 文字列 (`unsupported_audio_format`) | 文字列 (detail内) | Enum (`ErrorCode.UNSUPPORTED_FORMAT`) |
| 詳細情報 | ❌ なし | ❌ なし | ✅ details オブジェクト |
| ログ出力 | ❌ なし | ❌ 未確認 | ✅ 構造化ログ |

### WebSocket Implementation

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| プロトコル | バイナリ + JSON制御 | バイナリのみ | バイナリ (PCM) |
| 終了シグナル | `{"event": "end"}` | クライアント切断 | クライアント切断 |
| 中間結果 | ❌ なし | ✅ `is_final` フラグ | ✅ `partial`/`final` type |
| バッファリング | フレームリスト | async generator | `AudioBuffer` クラス |

### Test Coverage

| 項目 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| テストファイル数 | 2 | 2 | 4 |
| テストケース概算 | 2-3件 | 6-8件 | 10-15件 |
| テストタイプ | 基本的な統合テスト | モック + 統合テスト | コントラクト + ユニットテスト |
| API Keyテスト | ❌ | ✅ | ❌ (認証なし) |
| ファイルサイズテスト | ❌ | ✅ | ✅ |
| Rate Limitテスト | ❌ | ✅ (モック) | ❌ (Rate Limitなし) |

## Spec-to-Implementation Alignment

### Codex Alignment

| FR | 要件 | 実装状況 | エビデンス |
|----|------|----------|------------|
| FR-001 | 音声ファイル受付 | ✅ 実装済み | `transcribe_file()` 関数 |
| FR-002 | WebSocketストリーミング | ✅ 実装済み | `stt_stream()` 関数 |
| FR-003 | フォーマット検証 | ⚠️ 部分的 | `audio_format` パラメータで受け取るが検証は簡易 |
| FR-004 | ヘルスエンドポイント | ❌ 未実装 | STT APIにはなし（別エンドポイントか要確認） |
| FR-005 | エラーレスポンス | ✅ 実装済み | `ErrorResponse` スキーマ |
| FR-006 | 60秒制限 | ❌ 未実装 | 制限ロジックなし |
| FR-007 | プレーンテキスト返却 | ✅ 実装済み | `TranscriptionResult.text` |
| FR-008 | 認証なし | ✅ 実装済み | 認証コードなし |
| FR-009 | WAV/FLAC/MP3サポート | ⚠️ 部分的 | 形式は受け取るが実際の変換はサービス依存 |

**達成率**: 6/9 (67%) + 2部分的

### Gemini Alignment

| FR | 要件 | 実装状況 | エビデンス |
|----|------|----------|------------|
| FR-001 | /transcribe/file エンドポイント | ✅ 実装済み | `transcribe_file()` 関数 |
| FR-002 | WAV/MP3サポート | ✅ 実装済み | `content_type` 検証 |
| FR-003 | JSON返却 | ✅ 実装済み | `TranscriptionResult` モデル |
| FR-004 | /transcribe/stream エンドポイント | ✅ 実装済み | `transcribe_stream()` 関数 |
| FR-005 | タイムスタンプ付き結果 | ✅ 実装済み | `start_timestamp`, `end_timestamp` |
| FR-006 | reazonspeech-nemo-v2使用 | ⚠️ 部分的 | モック実装、実際のモデルは未確認 |
| FR-007 | エラーハンドリング | ✅ 実装済み | `HTTPException` 使用 |
| FR-008 | 50MB制限 | ✅ 実装済み | `MAX_FILE_SIZE_BYTES` 定数 |
| FR-009 | API Key認証 | ✅ 実装済み | `get_api_key` 依存性 |
| FR-010 | 構造化ログ | ⚠️ 部分的 | ログ設定は存在するが詳細未確認 |
| FR-011 | Prometheusメトリクス | ⚠️ 部分的 | `middlewares/metrics.py` 存在 |
| FR-012 | Rate Limiting | ✅ 実装済み | `rate_limit_dependency` |

**達成率**: 9/12 (75%) + 3部分的

### Claude Alignment

| FR | 要件 | 実装状況 | エビデンス |
|----|------|----------|------------|
| FR-001 | 音声ファイル変換 (WAV/MP3/FLAC/OGG) | ✅ 実装済み | `validate_audio_format()` 関数 |
| FR-002 | reazonspeech-nemo-v2使用 | ⚠️ 部分的 | サービス層で実装（モック可能性あり） |
| FR-003 | テキスト+処理時間+長さ返却 | ✅ 実装済み | `TranscriptionResponse` モデル |
| FR-004 | 16kHzリサンプリング | ⚠️ 部分的 | サービス層で処理 |
| FR-005 | 100MB制限 | ✅ 実装済み | `MAX_FILE_SIZE` 定数 |
| FR-006 | WebSocketストリーミング | ✅ 実装済み | `stream_transcribe()` 関数 |
| FR-007 | ステータスエンドポイント | ✅ 実装済み | `get_status()` 関数 |
| FR-008 | ローカル処理のみ | ✅ 実装済み | 外部API呼び出しなし |
| FR-009 | 構造化エラーレスポンス | ✅ 実装済み | `ErrorResponse` + `ErrorCode` Enum |

**達成率**: 7/9 (78%) + 2部分的

## Overall Evaluation

### Implementation Scores

| カテゴリ | Codex | Gemini | Claude |
|----------|-------|--------|--------|
| コード量 | 142行 (最小) | 412行 (中間) | 1045行 (最大) |
| テスト充実度 | 可 | 良 | 優 |
| エラー処理 | 可 | 良 | 優 |
| API設計 | 可 | 良 | 優 |
| 仕様遵守度 | 67% | 75% | 78% |
| **総合評価** | **可** | **良** | **優** |

### Key Findings

**Codex**:
- 最もシンプルで軽量な実装
- テストカバレッジが最小限
- 仕様に定義された制限（60秒）が未実装

**Gemini**:
- API Key認証とRate Limitingを実装
- Prometheusメトリクスの基盤あり
- APIバージョニング（v1）を採用

**Claude**:
- 最も包括的な実装（コード量3倍以上）
- 構造化ログと詳細なエラーハンドリング
- ステータスAPIでモデル状態を監視可能
- コントラクトテストとユニットテストの分離

## Conclusions

1. **実装品質**: Claude > Gemini > Codex
2. **仕様遵守度**: Claude (78%) > Gemini (75%) > Codex (67%)
3. **運用機能**: Gemini (認証+Rate Limit+Metrics) > Claude (ステータス+ログ) > Codex (最小限)
4. **テスト充実度**: Claude > Gemini > Codex

各エージェントは異なるアプローチで実装しており、Codexは最小限の実装、Geminiは運用機能重視、Claudeは品質と保守性重視という特徴がある。
