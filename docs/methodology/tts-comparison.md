# TTS Implementation Comparison

このドキュメントでは、3つのコーディングエージェント（Codex, Gemini, Claude）のTTS（Text-to-Speech）機能について、仕様書と実装の両面から比較分析を行います。

!!! info "前提条件"
    この比較の実験環境、共通ワークフロー、Constitution については [Comparison Baseline](comparison-baseline.md) を参照してください。

## Overview

各エージェントは同じ音声合成機能（Style-Bert-VITS2を使用した日本語TTS API）を実装することを求められましたが、仕様の定義方法と実装アプローチには顕著な違いがあります。この比較を通じて、各エージェントの設計思想と開発能力を客観的に評価します。

## Comparison Baseline

| Agent | Commit | Repository | Date |
|-------|--------|------------|------|
| Codex | [c40b09d](https://github.com/0h-n0/local-voice-assistant-codex/commit/c40b09dfb388af7bec45a520640b7bc3f782c161) | local-voice-assistant-codex | 2025-12-27 |
| Gemini | [8f8d9de](https://github.com/0h-n0/local-voice-assistant-gemini/commit/8f8d9de33be7d7a05fdf4de507aa18ee16ff5bec) | local-voice-assistant-gemini | 2025-12-27 |
| Claude | [21bd10c](https://github.com/0h-n0/local-voice-assistant-claude/commit/21bd10cbb263a11ab80203c9c33167d8e3b9534e) | local-voice-assistant-claude | 2025-12-27 |

### TTS Spec Locations

| Agent | Spec Path | Spec Feature ID |
|-------|-----------|-----------------|
| Codex | `specs/001-tts-api/spec.md` | 001-tts-api |
| Gemini | `specs/004-tts-japanese-api/spec.md` | 004-tts-japanese-api |
| Claude | `specs/004-tts-api/spec.md` | 004-tts-api |

---

## Specification Comparison

各エージェントが定義したTTS仕様書を比較します。機能要件（FR）と成功基準（SC）の違いを分析します。

### Functional Requirements Comparison

| 要件カテゴリ | Codex | Gemini | Claude |
|--------------|-------|--------|--------|
| テキスト受付エンドポイント | ✅ FR-001 | ✅ FR-001 | ✅ FR-001 |
| 音声フォーマット | WAV (FR-007) | WAV (FR-002) | WAV (FR-002) |
| ストリーミング対応 | ❌ なし | ✅ FR-006 | ❌ なし |
| 音声スタイル選択 | ✅ FR-002 | ✅ FR-003, FR-004 | ❌ 対象外 |
| 話速パラメータ | ❌ なし | ✅ FR-005 | ✅ FR-003 |
| テキスト長制限 | 1,000文字 (FR-006) | 500文字 (FR-007) | 5,000文字 (FR-004) |
| 認証要件 | ❌ なし | ✅ API Key (FR-008) | ❌ なし |
| ヘルスチェック | ✅ FR-004 | ❌ なし | ✅ FR-005 |
| エラーハンドリング | ✅ FR-003, FR-005 | ✅ FR-007 | ✅ FR-004 |
| ログ/メトリクス | ❌ なし | ✅ FR-009, FR-010 | ✅ FR-006 |
| 同時リクエスト制限 | ❌ なし | ❌ なし | ✅ FR-007 |
| **機能要件数** | **7件** | **10件** | **7件** |

!!! note "Geminiの特徴"
    Geminiは3エージェント中最も多くの機能要件（10件）を定義しており、ストリーミング対応、認証、Observabilityなど運用面の要件が充実しています。

### Feature Support Matrix

| 機能 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| 基本音声合成 | ✅ | ✅ | ✅ |
| 音声スタイル/感情選択 | ✅ | ✅ | ❌ |
| 話速調整 | ❌ | ✅ | ✅ |
| ピッチ調整 | ❌ | ✅ | ❌ |
| ストリーミング出力 | ❌ | ✅ | ❌ |
| API認証 | ❌ | ✅ | ❌ |
| ヘルスチェック | ✅ | ❌ | ✅ |
| Prometheusメトリクス | ❌ | ✅ | ❌ |
| 処理時間追跡 | ❌ | ❌ | ✅ |
| 同時リクエスト制限 | ❌ | ❌ | ✅ |

!!! info "設計思想の違い"
    - **Codex**: シンプルで即座に利用可能（認証なし、基本機能重視）
    - **Gemini**: 機能豊富（ストリーミング、認証、Observability）
    - **Claude**: 運用安定性重視（同時リクエスト制限、処理時間追跡）

### Success Criteria Comparison

| 基準 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| レイテンシ目標 | 5秒/1文 (SC-002) | 2秒/20文字 (SC-001) | 10秒/100文字 (SC-001) |
| 成功率 | 95% (SC-001) | 未指定 | 未指定 |
| 同時リクエスト | 未指定 | 2件 (SC-003) | 100件連続 (SC-003) |
| 音質基準 | 未指定 | 44.1kHz (SC-002) | 聞き取り可能 (SC-002) |
| スタイル数 | 未指定 | 10種類 (SC-004) | 未指定 |
| エラー応答時間 | 未指定 | 未指定 | 1秒以内 (SC-004) |
| 統合時間 | 30分 (SC-004) | 未指定 | 未指定 |

詳細な分析については各エージェントのページを参照してください：

- [Codex TTS Analysis](../agents/codex/tts-analysis.md)
- [Gemini TTS Analysis](../agents/gemini/tts-analysis.md)
- [Claude TTS Analysis](../agents/claude/tts-analysis.md)

---

## Implementation Comparison

各エージェントが生成した実装コードを比較します。アーキテクチャ、コード品質、機能実装の違いを分析します。

### Architecture Comparison

| アーキテクチャ要素 | Codex | Gemini | Claude |
|-------------------|-------|--------|--------|
| フレームワーク | FastAPI | FastAPI | FastAPI |
| レイヤー構造 | 2層（API + Engine） | 3層（API + Synthesizer + ModelManager） | 3層（API + Service + Model） |
| 非同期処理 | 同期処理 | asyncio + ThreadPoolExecutor | asyncio + ThreadPoolExecutor |
| 依存性注入 | なし | FastAPI Depends | FastAPI Depends |
| エラーハンドリング | JSONResponse直接返却 | HTTPException | カスタムエラーレスポンス関数 |

### Code Quality Metrics

| メトリクス | Codex | Gemini | Claude |
|-----------|-------|--------|--------|
| APIエンドポイントコード行数 | 38行 | 45行 | 162行 |
| サービス層コード行数 | 22行 | 122行 | 236行 |
| 型ヒント使用 | 部分的 | 完全 | 完全 |
| ドキュメント文字列 | なし | あり | あり |
| ロギング実装 | なし | あり（Prometheus統合） | あり（構造化ログ） |

### Implementation Features

| 実装機能 | Codex | Gemini | Claude |
|---------|-------|--------|--------|
| Style-Bert-VITS2統合 | スタブのみ | 完全実装 | 完全実装 |
| モデル読み込み | なし | ModelManager経由 | 非同期load_model |
| 同時実行制御 | なし | Semaphore(2) | Semaphore(configurable) |
| 音声パラメータ | voice | speed, pitch, style, style_weight | speed |
| ストリーミング | なし | 文単位分割ストリーミング | なし |
| メトリクス収集 | なし | Prometheus Counter/Histogram | X-Processing-Time ヘッダー |
| ヘルスチェック | 未確認 | なし（仕様で未定義） | /api/tts/status エンドポイント |

### Streaming Implementation (Gemini Only)

Geminiのみがストリーミング出力を実装しています：

```python
# Gemini: 文単位での分割ストリーミング
async def synthesize_stream(self, request: TTSRequest) -> AsyncGenerator[bytes, None]:
    sentences = self._split_sentences(request.text)  # 。！？\n で分割
    for segment in sentences:
        sr, pcm_data = await self._infer_pcm(segment, request)
        yield pcm_data
```

!!! note "ストリーミングの利点"
    - 最初の音声チャンクが早く返却される（Time-to-First-Byte短縮）
    - 長文テキストでもユーザー体験が向上
    - メモリ使用量の最適化

### Error Handling Comparison

| エラー種別 | Codex | Gemini | Claude |
|-----------|-------|--------|--------|
| 空テキスト | ValueError → 400 | ValueError → 400 | TTSErrorCode.EMPTY_TEXT → 400 |
| テキスト長超過 | ValueError → 400 | ValueError → 400 | TTSErrorCode.TEXT_TOO_LONG → 400 |
| 未対応ボイス | ValueError → 400 | N/A | N/A |
| モデル未読込 | N/A | Exception → 500 | TTSErrorCode.MODEL_NOT_LOADED → 503 |
| 同時リクエスト超過 | N/A | N/A | TTSErrorCode.SERVICE_BUSY → 503 |
| 合成失敗 | ValueError → 500 | Exception → 500 | TTSErrorCode.SYNTHESIS_FAILED → 500 |

!!! info "Claude のエラーハンドリング"
    Claude は最も詳細なエラーコード体系を実装しており、クライアントがエラーの種類を識別しやすい設計になっています。また、503 ステータスコードを適切に使用してサービス一時不可状態を表現しています。

詳細な実装分析については各エージェントのページを参照してください：

- [Codex TTS Analysis](../agents/codex/tts-analysis.md)
- [Gemini TTS Analysis](../agents/gemini/tts-analysis.md)
- [Claude TTS Analysis](../agents/claude/tts-analysis.md)

---

## Alignment Analysis

各エージェントの仕様書と実装の整合性を分析します。

### Spec-Implementation Alignment Matrix

| 仕様要件 | Codex Spec | Codex Impl | Gemini Spec | Gemini Impl | Claude Spec | Claude Impl |
|---------|------------|------------|-------------|-------------|-------------|-------------|
| 日本語テキスト受付 | ✅ | ⚠️ スタブ | ✅ | ✅ | ✅ | ✅ |
| WAV形式出力 | ✅ | ⚠️ ヘッダのみ | ✅ | ✅ | ✅ | ✅ |
| 音声スタイル選択 | ✅ | ⚠️ 検証のみ | ✅ | ✅ | ❌ | ❌ |
| 話速パラメータ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ |
| テキスト長制限 | ✅ (1000) | ✅ | ✅ (500) | ✅ | ✅ (5000) | ✅ |
| ヘルスチェック | ✅ | ⚠️ 未確認 | ❌ | ❌ | ✅ | ✅ |
| ストリーミング | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ |
| API認証 | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ |
| Prometheusメトリクス | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ |
| 同時リクエスト制限 | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ |

!!! warning "Alignment Legend"
    - ✅ 仕様通り実装済み / 仕様と実装が一致
    - ⚠️ 部分的実装 / 仕様を満たしていない
    - ❌ 仕様なし / 未実装

### Alignment Score Summary

| Agent | 仕様定義 | 実装完了 | 整合率 | 追加実装 |
|-------|---------|---------|--------|---------|
| Codex | 7件 | 2件 | 29% | 0件 |
| Gemini | 10件 | 9件 | 90% | 1件 (同時制限) |
| Claude | 7件 | 7件 | 100% | 0件 |

!!! note "整合率の解釈"
    - **Codex (29%)**: スタブ実装のため、仕様の大部分が未実現
    - **Gemini (90%)**: ヘルスチェック以外は完全実装、仕様外の同時制限も追加
    - **Claude (100%)**: 仕様通りの完全実装

### Gap Analysis by Agent

#### Codex Gaps

| Gap | 仕様 | 実装状況 | 影響 |
|-----|------|---------|------|
| Style-Bert-VITS2統合 | FR-001 | スタブのみ | 致命的 |
| 実際の音声生成 | FR-007 | b"RIFF"のみ | 致命的 |
| ヘルスチェック | FR-004 | 未確認 | 中程度 |

#### Gemini Gaps

| Gap | 仕様 | 実装状況 | 影響 |
|-----|------|---------|------|
| ヘルスチェック | 仕様なし | 未実装 | 低（仕様で未定義） |

#### Claude Gaps

| Gap | 仕様 | 実装状況 | 影響 |
|-----|------|---------|------|
| なし | - | 完全実装 | - |

### Design Philosophy Comparison

| 観点 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| 仕様の詳細度 | 基本的 | 包括的 | バランス型 |
| 実装の完成度 | プロトタイプ | 本番レベル | 本番レベル |
| 拡張性設計 | 低 | 高 | 中 |
| 運用考慮 | 低 | 高 | 高 |
| Out of Scope 明記 | なし | 部分的 | 明確 |

!!! info "設計思想の特徴"
    - **Codex**: 迅速なプロトタイピング重視。仕様は簡潔だが実装は最小限
    - **Gemini**: 機能豊富で運用重視。ストリーミング、認証、メトリクスを標準装備
    - **Claude**: 仕様と実装の一貫性重視。Out of Scope を明確に定義し、定義した機能は完全実装

詳細な整合性分析については各エージェントのページを参照してください：

- [Codex TTS Analysis](../agents/codex/tts-analysis.md)
- [Gemini TTS Analysis](../agents/gemini/tts-analysis.md)
- [Claude TTS Analysis](../agents/claude/tts-analysis.md)

---

## Conclusion

### Summary

3つのエージェントによるTTS API実装を比較した結果、以下の特徴が明らかになりました：

| 評価軸 | 最優秀 | 理由 |
|-------|--------|------|
| 仕様の包括性 | Gemini | 10件の機能要件、ストリーミング・認証・Observability対応 |
| 実装の完成度 | Claude / Gemini | Style-Bert-VITS2完全統合、本番運用可能な品質 |
| 仕様・実装整合性 | Claude | 100%整合率、Out of Scope明確化 |
| 運用考慮 | Gemini | Prometheusメトリクス、API認証、ストリーミング |
| シンプルさ | Codex | 60行程度の最小実装 |

### Recommendations

1. **本番運用を目指す場合**: Gemini または Claude の実装を参考に
2. **プロトタイピングの場合**: Codex のシンプルな構造を参考に
3. **仕様書作成の参考**: Claude の Out of Scope 明記と Gemini の詳細な機能要件定義

### Next Steps

- STT (Speech-to-Text) 機能との統合分析
- LLM サービスとの連携評価
- エンドツーエンドの音声アシスタントワークフロー検証

---
