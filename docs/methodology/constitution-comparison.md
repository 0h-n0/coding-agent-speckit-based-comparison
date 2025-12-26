# 憲法比較分析

3つのコーディングエージェント（Codex, Gemini, Claude）の憲法を比較分析します。

!!! info "言語について / Language Note"
    このドキュメントは日本語で記載されています。技術用語は英語のまま使用しています。

    This document is written in Japanese. Technical terms are kept in English for clarity.

!!! info "前提条件"
    この比較の実験環境、共通ワークフロー、Constitution については [Comparison Baseline](comparison-baseline.md) を参照してください。

## 比較ベースライン

この比較は以下のコミット時点の憲法に基づいています。

| Agent | Version | Repository | Commit | Date |
|-------|---------|------------|--------|------|
| Codex | 1.1.0 | [local-voice-assistant-codex](https://github.com/0h-n0/local-voice-assistant-codex) | [5c2031e](https://github.com/0h-n0/local-voice-assistant-codex/commit/5c2031e347e42f8eefde569db2d1ad513c0a5564) | 2025-12-25 |
| Gemini | 1.2.0 | [local-voice-assistant-gemini](https://github.com/0h-n0/local-voice-assistant-gemini) | [5f9064f](https://github.com/0h-n0/local-voice-assistant-gemini/commit/5f9064f672641534adee80dd9fcb3a60c9b48527) | 2025-12-25 |
| Claude | 1.1.0 | [local-voice-assistant-claude](https://github.com/0h-n0/local-voice-assistant-claude) | [7a68ec6](https://github.com/0h-n0/local-voice-assistant-claude/commit/7a68ec67e66b30620dfcbe462698f8b311dabc8b) | 2025-12-25 |

## 基本原則比較

各エージェントの憲法で定義されている基本原則を比較します。

| 原則 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| プライバシー/セキュリティ | ✅ Principle I (Local-First Privacy) | ⚠️ Security section (簡潔) | ✅ Principle I (Privacy-First, NON-NEGOTIABLE) |
| パフォーマンス | ❌ 未指定 | ❌ 未指定 | ✅ Principle II (Performance-Conscious: <500ms) |
| シンプリシティ/YAGNI | ❌ 未指定 | ✅ Principle VII (Simplicity/YAGNI) | ✅ Principle III (Simplicity-First) |
| テスト駆動開発 | ✅ Principle III (Reliability) | ✅ Principle III (Test-First) | ✅ Principle IV (TDD, NON-NEGOTIABLE) |
| モジュラリティ | ❌ 未指定 | ✅ Principle I (Library-First) | ✅ Principle V (Modular Architecture) |
| オブザーバビリティ | ✅ Principle IV | ✅ Principle V | ✅ Principle VI |
| セマンティックバージョニング | ✅ Principle V | ✅ Principle VI | ⚠️ Amendment process で暗示 |
| スクリプタブルインターフェース | ✅ Principle II | ✅ Principle II (CLI Interface) | ❌ 未指定 |

### 主な発見

- **Claude**: 最も詳細な原則を持ち、明示的な NON-NEGOTIABLE マーカーを使用
- **Gemini**: ユニークな Library-First アプローチを採用
- **Codex**: Local-First Privacy と Scriptable Interfaces を重視
- **Claude**: 具体的なパフォーマンス目標（<500ms レイテンシ）を含む

## 総合評価サマリー

| カテゴリ | Codex | Gemini | Claude |
|----------|-------|--------|--------|
| 基本原則 | 良 | 良 | 優 |
| 技術スタック | 良 | 良 | 優 |
| 開発ワークフロー | 可 | 良 | 優 |
| 品質基準 | 可 | 良 | 優 |
| ガバナンス | 良 | 可 | 優 |
| **総合** | **良** | **良** | **優** |

!!! info "評価基準"
    - **優 (Excellent)**: 要素が包括的な詳細、理由、例とともに存在
    - **良 (Good)**: 要素が適切な詳細とともに存在
    - **可 (Acceptable)**: 要素は言及されているが詳細が不足
    - **不足 (Insufficient)**: 要素が欠落または重大な不完全性

## 技術スタック比較

各エージェントの憲法で指定されている技術スタックを比較します。

| 要素 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| フロントエンドフレームワーク | ✅ React/Next + TypeScript | ✅ Next.js + React + TypeScript | ✅ Next.js + React + TypeScript |
| バックエンドフレームワーク | ✅ FastAPI | ✅ FastAPI | ✅ FastAPI |
| パッケージマネージャー | ✅ uv | ✅ uv | ✅ uv |
| 型バリデーション | ✅ Pydantic | ✅ Pydantic | ✅ Pydantic v2 |
| Linting | ✅ Ruff | ✅ Ruff | ✅ Ruff |
| 型チェック | ❌ 未指定 | ❌ 未指定 | ✅ mypy (strict) |
| テストフレームワーク | ❌ 未指定 | ❌ 未指定 | ✅ pytest, Jest + RTL |
| プロジェクト構造 | ❌ 未記載 | ❌ 未記載 | ✅ 詳細なディレクトリツリー |

### 主な発見

- 3つのエージェントすべてが同じコアスタックを指定（React/Next, FastAPI, uv, Pydantic, Ruff）
- Claude が最も包括的なスタック仕様を提供（mypy strict モードを含む）
- Claude は詳細なプロジェクト構造テンプレートを含む
- Codex と Gemini はテストフレームワークの仕様が欠落

## 開発ワークフロー比較

各エージェントの開発ワークフロー規定を比較します。

| 要素 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| ブランチ戦略 | ✅ フィーチャーブランチ必須 | ✅ ブランチ作成必須 | ✅ フィーチャーブランチ (NON-NEGOTIABLE) |
| PR 要件 | ✅ 必須 | ✅ レビュー付き必須 | ✅ コンプライアンスチェックリスト付き必須 |
| コードレビュー | ⚠️ 言及あり（詳細なし） | ✅ 必須（1+ 承認者） | ✅ 詳細な要件あり |
| README 更新 | ✅ 必須 | ✅ 必須 | ✅ 必須 |
| コミットフォーマット | ❌ 未指定 | ❌ 未指定 | ✅ Conventional commits |
| マージ戦略 | ❌ 未指定 | ❌ 未指定 | ✅ Squash merge 推奨 |

### 主な発見

- Claude が最も詳細なワークフローを持ち、明示的な NON-NEGOTIABLE 指定あり
- 全エージェントがフィーチャーブランチと PR ワークフローを必須
- Claude がコミットメッセージフォーマットとマージ戦略を指定
- Codex と Gemini はコミットフォーマットのガイダンスが欠落

## 品質基準比較

各エージェントの品質基準を比較します。

| 要素 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| テスト要件 | ✅ 重要パスの自動テスト | ✅ TDD 必須 | ✅ TDD (NON-NEGOTIABLE), 3種類のテスト |
| Linting ルール | ✅ Ruff 強制 | ✅ Ruff 必須 | ✅ Ruff 警告ゼロ |
| 型チェック | ❌ 未指定 | ✅ Pydantic（ランタイム） | ✅ mypy strict + Pydantic |
| カバレッジ要件 | ❌ 未指定 | ❌ 未指定 | ✅ カバレッジ低下不可 |
| フロントエンド品質 | ❌ 未指定 | ❌ 未指定 | ✅ ESLint, Prettier, tsc |
| 統合テスト | ⚠️ 言及あり | ✅ Principle IV | ✅ 必須 |
| コントラクトテスト | ❌ 未指定 | ❌ 未指定 | ✅ 必須 |

### 主な発見

- Claude が最も包括的な品質基準を提供
- Claude は3種類のテスト（unit, integration, contract）を指定
- Claude はフロントエンド固有の品質要件を含む
- Codex と Gemini はフロントエンド品質仕様が欠落

## ガバナンス比較

各エージェントのガバナンス構造を比較します。

| 要素 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| 修正プロセス | ✅ 書面による提案必須 | ✅ PR ベースの修正 | ✅ 詳細な5ステッププロセス |
| バージョニングポリシー | ✅ MAJOR/MINOR/PATCH 定義 | ❌ 未指定 | ✅ MAJOR/MINOR/PATCH 定義 |
| コンプライアンスレビュー | ✅ spec/plan に必須 | ⚠️ ガバナンスで暗示 | ✅ 全 PR に必須 |
| 例外処理 | ✅ 例外の文書化 | ✅ 明示的な正当化 | ✅ Complexity Tracking セクション |
| テンプレート同期 | ✅ 影響レポートヘッダー | ❌ 未指定 | ✅ SYNC IMPACT REPORT |

### 主な発見

- Codex と Claude は類似したガバナンス構造を持つ
- Claude が最も詳細な修正プロセスを持つ
- Gemini は明示的なバージョニングポリシーが欠落
- Codex と Claude は SYNC IMPACT REPORT ヘッダーを使用

## ソフトウェア開発完全性評価

ソフトウェア開発に必要な要素のカバレッジを評価します。

| 要件 | Codex | Gemini | Claude |
|------|-------|--------|--------|
| **技術スタック** | ✅ 完全 | ✅ 完全 | ✅ 完全 |
| **テスト要件** | ⚠️ 部分的 | ✅ 完全 | ✅ 完全 |
| **コードスタイル** | ✅ 完全 | ✅ 完全 | ✅ 完全 |
| **ワークフロー** | ⚠️ 部分的 | ⚠️ 部分的 | ✅ 完全 |
| **プロジェクト構造** | ❌ 欠落 | ❌ 欠落 | ✅ 完全 |
| **品質ゲート** | ⚠️ 部分的 | ⚠️ 部分的 | ✅ 完全 |

!!! success "結論"
    - **ソフトウェア開発完全性**: Claude が最も包括的なガイダンスを提供
    - **プライバシー重視プロジェクト**: Codex が最も強力なプライバシーファーストアプローチ
    - **モジュラーライブラリ開発**: Gemini の Library-First 原則が適合
    - **共通点**: 3つすべてが React/Next + FastAPI + uv + Pydantic + Ruff スタックを共有

## スコアリング方法論

### 評価アプローチ

このセクションでは、各エージェントの憲法を相対評価するためのスコアリング方法論を説明します。

**採用した方法**: 客観的基準によるスコアリング

| スコア | 日本語 | 基準 |
|--------|--------|------|
| 優 (Excellent) | ゆう | 要素が包括的な詳細、理由、例とともに存在 |
| 良 (Good) | りょう | 要素が適切な詳細とともに存在 |
| 可 (Acceptable) | か | 要素は言及されているが詳細が不足 |
| 不足 (Insufficient) | ふそく | 要素が欠落または重大な不完全性 |

**検討した代替案**:

1. 主観的品質評価 - 却下: 再現性がなく、バイアスの指摘を受けやすい
2. 重み付けルーブリックスコアリング - 却下: ドキュメント比較に対して複雑さが過剰

## 詳細評価と根拠

### Codex の評価

| カテゴリ | 評価 | 根拠 |
|----------|------|------|
| 基本原則 | 良 | Privacy, Scriptable Interfaces, Observability を明確に定義。ただし Modularity, Simplicity が欠落 |
| 技術スタック | 良 | コアスタックを指定。mypy, テストフレームワークが未指定 |
| 開発ワークフロー | 可 | ブランチ・PR は必須だが、コミットフォーマット・マージ戦略が未指定 |
| 品質基準 | 可 | 基本的なテスト・Linting はあるが、カバレッジ・フロントエンド品質が欠落 |
| ガバナンス | 良 | 修正プロセス・バージョニング・テンプレート同期を定義 |

> 「Local-First Privacy」は Codex のユニークな強みであり、プライバシー重視プロジェクトに最適です。

### Gemini の評価

| カテゴリ | 評価 | 根拠 |
|----------|------|------|
| 基本原則 | 良 | Library-First, Simplicity/YAGNI, Integration Testing を明確に定義 |
| 技術スタック | 良 | コアスタックを指定。mypy, テストフレームワークが未指定 |
| 開発ワークフロー | 良 | ブランチ・PR・レビュー要件を定義。コミットフォーマットが未指定 |
| 品質基準 | 良 | TDD 必須、統合テスト重視。フロントエンド品質が欠落 |
| ガバナンス | 可 | PR ベース修正はあるが、バージョニングポリシーが欠落 |

> 「Library-First」アプローチはモジュラー設計を促進し、再利用性の高いコードベースに貢献します。

### Claude の評価

| カテゴリ | 評価 | 根拠 |
|----------|------|------|
| 基本原則 | 優 | 全原則を詳細に定義、NON-NEGOTIABLE マーカー使用、具体的パフォーマンス目標 (<500ms) |
| 技術スタック | 優 | 完全なスタック指定 (mypy strict, pytest, Jest + RTL)、詳細なプロジェクト構造 |
| 開発ワークフロー | 優 | Conventional commits, Squash merge, コンプライアンスチェックリスト付き PR |
| 品質基準 | 優 | 3種類のテスト (unit, integration, contract)、フロントエンド品質要件 (ESLint, Prettier, tsc) |
| ガバナンス | 優 | 詳細な5ステップ修正プロセス、コンプライアンスレビュー必須 |

> NON-NEGOTIABLE マーカーにより、妥協できない要件が明確に区別されています。

## 相対ランキングと正当化

### 総合ランキング

| 順位 | エージェント | 総合評価 | 主な強み |
|------|--------------|----------|----------|
| 1位 | **Claude** | 優 | 最も包括的、NON-NEGOTIABLE マーカー、具体的目標 |
| 2位 | **Gemini** | 良 | Library-First、Simplicity/YAGNI、統合テスト重視 |
| 2位 | **Codex** | 良 | Local-First Privacy、Scriptable Interfaces |

!!! note "同率2位について"
    Codex と Gemini は総合評価「良」で同率2位です。プロジェクトの要件に応じて選択してください：

    - **プライバシー重視** → Codex
    - **モジュラー設計重視** → Gemini

### ランキング正当化

1. **Claude が1位の理由**:
    - 唯一すべてのカテゴリで「優」を獲得
    - 具体的で測定可能な基準（<500ms レイテンシ等）を含む
    - フロントエンドとバックエンド両方の品質要件を網羅
    - 詳細なプロジェクト構造テンプレートを提供

2. **Codex と Gemini が同率2位の理由**:
    - 両者とも基本的な要件を満たしているが、詳細度で Claude に劣る
    - それぞれユニークな強み（Privacy vs Library-First）を持つ
    - 総合評価は同等だが、得意分野が異なる

## エージェント別詳細

各エージェントの詳細な憲法分析は以下のページを参照してください：

- [Codex 憲法分析](../agents/codex/constitution.md)
- [Gemini 憲法分析](../agents/gemini/constitution.md)
- [Claude 憲法分析](../agents/claude/constitution.md)
