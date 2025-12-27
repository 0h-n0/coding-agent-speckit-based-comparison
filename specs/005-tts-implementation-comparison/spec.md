# Feature Specification: TTS Implementation Comparison

**Feature Branch**: `005-tts-implementation-comparison`
**Created**: 2025-12-27
**Status**: Draft
**Input**: User description: "それぞれのsubmoduleのspec/*tts*にかんする実装を比較してください。また、SPECの比較とその実装の比較もスクリプトを確認してください"

## Overview

本機能は、3つのコーディングエージェント（Codex, Gemini, Claude）が生成したTTSサービス（Style-Bert-VITS2による日本語音声合成）の仕様書と実装コードを比較分析するドキュメントを作成します。STT比較およびLLM比較と同様の構造で、仕様定義の違いと実装品質の差異を客観的に評価します。

## Comparison Baseline

| Agent | Spec Path | Feature ID | Spec FR Count |
|-------|-----------|------------|---------------|
| Codex | `specs/001-tts-api/spec.md` | 001-tts-api | 7 |
| Gemini | `specs/004-tts-japanese-api/spec.md` | 004-tts-japanese-api | 10 |
| Claude | `specs/004-tts-api/spec.md` | 004-tts-api | 7 |

## User Scenarios & Testing *(mandatory)*

### User Story 1 - 仕様書の比較閲覧 (Priority: P1)

ユーザーは、各エージェントが同じ要件（Style-Bert-VITS2を使用したTTS API）に対してどのような仕様書を作成したかを一覧で比較できます。

**Why this priority**: 仕様書の比較がすべての分析の基盤となります。

**Independent Test**: TTS比較ページにアクセスし、3エージェントの仕様書の機能要件が表形式で比較できることを確認できます。

**Acceptance Scenarios**:

1. **Given** ドキュメントサイトにアクセスしている, **When** TTS比較ページを開く, **Then** 3エージェントの機能要件比較表が表示される
2. **Given** 仕様比較表を閲覧している, **When** 各エージェントの詳細を確認する, **Then** ストリーミング対応、音声パラメータ設定、認証方式などの差異が明確に表示される

---

### User Story 2 - 実装コードの比較閲覧 (Priority: P1)

ユーザーは、各エージェントの実装コードの構造、行数、APIエンドポイント設計、エラーハンドリング方式を比較できます。

**Why this priority**: 仕様と実装の両面を比較することで、エージェントの総合的な能力を評価できます。

**Independent Test**: 実装比較セクションにアクセスし、コード構造、行数、APIパスの比較表が表示されることを確認できます。

**Acceptance Scenarios**:

1. **Given** TTS比較ページを閲覧している, **When** 実装比較セクションを確認する, **Then** API実装行数、サービス層行数、テスト行数が比較表示される
2. **Given** 実装比較表を閲覧している, **When** APIエンドポイント設計を確認する, **Then** パス設計、ストリーミング対応、認証対応の違いが明確に表示される

---

### User Story 3 - 仕様と実装の整合性分析 (Priority: P2)

ユーザーは、各エージェントが仕様書で定義した要件をどの程度実装に反映しているかを確認できます。

**Why this priority**: 仕様遵守度の評価により、エージェントの信頼性を判断できます。

**Independent Test**: 整合性分析セクションにアクセスし、各FRの実装状況（実装済み/部分的/未実装）が表示されることを確認できます。

**Acceptance Scenarios**:

1. **Given** TTS比較ページを閲覧している, **When** 整合性分析セクションを確認する, **Then** 各エージェントのFR達成率がパーセンテージで表示される
2. **Given** 整合性分析を確認している, **When** 個別のFRを確認する, **Then** 実装状況と対応するコード箇所への参照が表示される

---

### Edge Cases

- 特定エージェントのサブモジュールが未初期化の場合はどうなるか？→ 「データなし」として表示し、利用可能なエージェントのみ比較
- 仕様書のフォーマットがエージェント間で異なる場合はどうなるか？→ 共通の比較項目を定義し、該当項目がない場合は「未定義」と表示

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: ドキュメントは各エージェントのTTS仕様書の機能要件を比較する表を含まなければならない
- **FR-002**: ドキュメントは各エージェントのTTS実装コードの構造と行数を比較する表を含まなければならない
- **FR-003**: ドキュメントは各エージェントのAPIエンドポイント設計を比較しなければならない
- **FR-004**: ドキュメントは各エージェントのエラーハンドリング方式を比較しなければならない
- **FR-005**: ドキュメントはストリーミング対応の有無を明示しなければならない
- **FR-006**: ドキュメントは音声パラメータ（話速、ピッチ等）の設定可否を比較しなければならない
- **FR-007**: ドキュメントは各FRの実装状況を評価し、達成率を算出しなければならない
- **FR-008**: ドキュメントはテストカバレッジ（行数とテストタイプ）を比較しなければならない
- **FR-009**: 各エージェント専用の詳細分析ページを作成しなければならない
- **FR-010**: MkDocsナビゲーションにTTS分析ページへのリンクを追加しなければならない
- **FR-011**: ドキュメントは既存のSTT比較・LLM比較と一貫したフォーマットで作成しなければならない

### Key Entities

- **TTSSpec**: 各エージェントのTTS仕様書。機能要件、成功基準、キーエンティティを含む
- **TTSImplementation**: 各エージェントのTTS実装コード。API層、サービス層、テストを含む
- **ComparisonResult**: 仕様と実装の比較結果。達成率、未実装項目、品質評価を含む

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: ユーザーは3エージェントのTTS仕様を1ページで比較できる
- **SC-002**: ユーザーは各エージェントのコード行数と構造の違いを即座に把握できる
- **SC-003**: ユーザーは各エージェントの仕様遵守率（パーセンテージ）を確認できる
- **SC-004**: ドキュメントはSTT比較・LLM比較と同じ構造を持ち、一貫性のある分析を提供する
- **SC-005**: 各エージェントの詳細分析ページからメイン比較ページへ戻るリンクが機能する

## Assumptions

- 各サブモジュールは初期化済みで、specs/*tts*ディレクトリとbackend/実装コードが存在する
- 比較対象のコミットは現在のHEADとする
- 仕様書はSpeckit形式で記述されている
- 実装はFastAPI + Pythonで行われている

## Out of Scope

- 実際のTTS音声品質のテスト（モデルへのリクエスト送信）
- パフォーマンスベンチマーク（レイテンシ測定）
- セキュリティ監査
- コードのリファクタリング提案

## Research Summary

### Codex TTS Implementation

| 項目 | 値 |
|------|-----|
| Spec Feature ID | 001-tts-api |
| FR Count | 7 |
| Implementation Lines | 66 |
| Test Lines | 33 |
| Total Lines | 99 |
| Streaming | No |
| Voice Selection | Yes (voice parameter) |
| API Path | `POST /tts/synthesize` |
| Authentication | No |
| Health Endpoint | Referenced (FR-004) |

### Gemini TTS Implementation

| 項目 | 値 |
|------|-----|
| Spec Feature ID | 004-tts-japanese-api |
| FR Count | 10 |
| Implementation Lines | 134 |
| Test Lines | 0 |
| Total Lines | 134 |
| Streaming | Yes (StreamingResponse) |
| Voice Selection | Yes (model_id, style) |
| API Path | `POST /api/v1/tts/synthesize` |
| Authentication | Yes (API Key) |
| Models Endpoint | `GET /api/v1/tts/models` |

### Claude TTS Implementation

| 項目 | 値 |
|------|-----|
| Spec Feature ID | 004-tts-api |
| FR Count | 7 |
| Implementation Lines | 503 |
| Test Lines | 364 |
| Total Lines | 867 |
| Streaming | No |
| Voice Selection | No (single voice) |
| API Path | `POST /api/tts/synthesize` |
| Authentication | No |
| Status Endpoint | `GET /api/tts/status` |
