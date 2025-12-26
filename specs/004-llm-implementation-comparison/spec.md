# Feature Specification: LLM Implementation Comparison

**Feature Branch**: `004-llm-implementation-comparison`
**Created**: 2025-12-26
**Status**: Draft
**Input**: User description: "それぞれのsubmoduleのspec/*llm*にかんする実装を比較してください。また、SPECの比較とその実装の比較もスクリプトを確認してください"

## Overview

本機能は、3つのコーディングエージェント（Codex, Gemini, Claude）が生成したLLMサービス（OpenAI gpt-5-mini連携）の仕様書と実装コードを比較分析するドキュメントを作成します。STT比較（003-stt-implementation-comparison）と同様の構造で、仕様定義の違いと実装品質の差異を客観的に評価します。

## Comparison Baseline

| Agent | Spec Path | Feature ID | Spec FR Count |
|-------|-----------|------------|---------------|
| Codex | `specs/001-llm-service/spec.md` | 001-llm-service | 8 |
| Gemini | `specs/003-llm-service/spec.md` | 003-llm-service | 8 |
| Claude | `specs/003-llm-text-service/spec.md` | 003-llm-text-service | 9 |

## User Scenarios & Testing *(mandatory)*

### User Story 1 - 仕様書の比較閲覧 (Priority: P1)

ユーザーは、各エージェントが同じ要件（gpt-5-miniを使用したLLMサービス）に対してどのような仕様書を作成したかを一覧で比較できます。

**Why this priority**: 仕様書の比較がすべての分析の基盤となります。

**Independent Test**: LLM比較ページにアクセスし、3エージェントの仕様書の機能要件が表形式で比較できることを確認できます。

**Acceptance Scenarios**:

1. **Given** ドキュメントサイトにアクセスしている, **When** LLM比較ページを開く, **Then** 3エージェントの機能要件比較表が表示される
2. **Given** 仕様比較表を閲覧している, **When** 各エージェントの詳細を確認する, **Then** チャット履歴サポート、ストリーミング対応、パラメータ設定などの差異が明確に表示される

---

### User Story 2 - 実装コードの比較閲覧 (Priority: P1)

ユーザーは、各エージェントの実装コードの構造、行数、APIエンドポイント設計、エラーハンドリング方式を比較できます。

**Why this priority**: 仕様と実装の両面を比較することで、エージェントの総合的な能力を評価できます。

**Independent Test**: 実装比較セクションにアクセスし、コード構造、行数、APIパスの比較表が表示されることを確認できます。

**Acceptance Scenarios**:

1. **Given** LLM比較ページを閲覧している, **When** 実装比較セクションを確認する, **Then** API実装行数、サービス層行数、テスト行数が比較表示される
2. **Given** 実装比較表を閲覧している, **When** APIエンドポイント設計を確認する, **Then** パス設計、バージョニング、ストリーミング対応の違いが明確に表示される

---

### User Story 3 - 仕様と実装の整合性分析 (Priority: P2)

ユーザーは、各エージェントが仕様書で定義した要件をどの程度実装に反映しているかを確認できます。

**Why this priority**: 仕様遵守度の評価により、エージェントの信頼性を判断できます。

**Independent Test**: 整合性分析セクションにアクセスし、各FRの実装状況（実装済み/部分的/未実装）が表示されることを確認できます。

**Acceptance Scenarios**:

1. **Given** LLM比較ページを閲覧している, **When** 整合性分析セクションを確認する, **Then** 各エージェントのFR達成率がパーセンテージで表示される
2. **Given** 整合性分析を確認している, **When** 個別のFRを確認する, **Then** 実装状況と対応するコード箇所への参照が表示される

---

### Edge Cases

- 特定エージェントのサブモジュールが未初期化の場合はどうなるか？→ 「データなし」として表示し、利用可能なエージェントのみ比較
- 仕様書のフォーマットがエージェント間で異なる場合はどうなるか？→ 共通の比較項目を定義し、該当項目がない場合は「未定義」と表示

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: ドキュメントは各エージェントのLLM仕様書の機能要件を比較する表を含まなければならない
- **FR-002**: ドキュメントは各エージェントのLLM実装コードの構造と行数を比較する表を含まなければならない
- **FR-003**: ドキュメントは各エージェントのAPIエンドポイント設計を比較しなければならない
- **FR-004**: ドキュメントは各エージェントのエラーハンドリング方式を比較しなければならない
- **FR-005**: ドキュメントはストリーミング（SSE）対応の有無を明示しなければならない
- **FR-006**: ドキュメントはチャット履歴管理の実装有無を比較しなければならない
- **FR-007**: ドキュメントは各FRの実装状況を評価し、達成率を算出しなければならない
- **FR-008**: ドキュメントはテストカバレッジ（行数とテストタイプ）を比較しなければならない
- **FR-009**: 各エージェント専用の詳細分析ページを作成しなければならない
- **FR-010**: MkDocsナビゲーションにLLM分析ページへのリンクを追加しなければならない
- **FR-011**: ドキュメントは既存のSTT比較と一貫したフォーマットで作成しなければならない

### Key Entities

- **LLMSpec**: 各エージェントのLLM仕様書。機能要件、成功基準、キーエンティティを含む
- **LLMImplementation**: 各エージェントのLLM実装コード。API層、サービス層、テストを含む
- **ComparisonResult**: 仕様と実装の比較結果。達成率、未実装項目、品質評価を含む

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: ユーザーは3エージェントのLLM仕様を1ページで比較できる
- **SC-002**: ユーザーは各エージェントのコード行数と構造の違いを即座に把握できる
- **SC-003**: ユーザーは各エージェントの仕様遵守率（パーセンテージ）を確認できる
- **SC-004**: ドキュメントはSTT比較と同じ構造を持ち、一貫性のある分析を提供する
- **SC-005**: 各エージェントの詳細分析ページからメイン比較ページへ戻るリンクが機能する

## Assumptions

- 各サブモジュールは初期化済みで、specs/*llm*ディレクトリとbackend/実装コードが存在する
- 比較対象のコミットは現在のHEADとする
- 仕様書はSpeckit形式で記述されている
- 実装はFastAPI + Pythonで行われている

## Out of Scope

- 実際のLLM応答品質のテスト（モデルへのリクエスト送信）
- パフォーマンスベンチマーク（レイテンシ測定）
- セキュリティ監査
- コードのリファクタリング提案

## Research Summary

### Codex LLM Implementation

| 項目 | 値 |
|------|-----|
| Spec Feature ID | 001-llm-service |
| FR Count | 8 |
| Implementation Lines | 91 |
| Test Lines | 51 |
| Total Lines | 142 |
| Streaming | No |
| Chat History | No |
| API Path | `POST /llm/complete` |

### Gemini LLM Implementation

| 項目 | 値 |
|------|-----|
| Spec Feature ID | 003-llm-service |
| FR Count | 8 |
| Implementation Lines | 174 |
| Test Lines | 119 |
| Total Lines | 293 |
| Streaming | Yes (SSE) |
| Chat History | Yes (message list) |
| API Path | `POST /api/v1/llm/chat` |

### Claude LLM Implementation

| 項目 | 値 |
|------|-----|
| Spec Feature ID | 003-llm-text-service |
| FR Count | 9 |
| Implementation Lines | 678 |
| Test Lines | 615 |
| Total Lines | 1293 |
| Streaming | No |
| Chat History | Yes (in-memory cache with TTL) |
| API Path | `POST /api/llm/chat` |
