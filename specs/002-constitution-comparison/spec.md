# Feature Specification: Constitution Comparison Analysis

**Feature Branch**: `002-constitution-comparison`
**Created**: 2025-12-26
**Status**: Draft
**Input**: User description: "submoduleの憲法を徹底的に違いを比較してください。またどのコミットで比較したかコミットの番号やURLを辿れるようにしてください。それぞれのエージェントが使っているtokenなどもわかれば嬉しいですが、まずはソフトウェア制作のために必要な情報がちゃんとあるか確認してください。最終的には良し悪しを相対比較してください。"

## Comparison Baseline

| Agent | Repository | Commit | Date |
|-------|------------|--------|------|
| Codex | [local-voice-assistant-codex](https://github.com/0h-n0/local-voice-assistant-codex) | [5c2031e347e42f8eefde569db2d1ad513c0a5564](https://github.com/0h-n0/local-voice-assistant-codex/commit/5c2031e347e42f8eefde569db2d1ad513c0a5564) | 2025-12-25 23:02:34 +0900 |
| Gemini | [local-voice-assistant-gemini](https://github.com/0h-n0/local-voice-assistant-gemini) | [5f9064f672641534adee80dd9fcb3a60c9b48527](https://github.com/0h-n0/local-voice-assistant-gemini/commit/5f9064f672641534adee80dd9fcb3a60c9b48527) | 2025-12-25 22:28:12 +0900 |
| Claude | [local-voice-assistant-claude](https://github.com/0h-n0/local-voice-assistant-claude) | [7a68ec67e66b30620dfcbe462698f8b311dabc8b](https://github.com/0h-n0/local-voice-assistant-claude/commit/7a68ec67e66b30620dfcbe462698f8b311dabc8b) | 2025-12-25 23:14:58 +0900 |

## User Scenarios & Testing *(mandatory)*

### User Story 1 - View Constitution Comparison Online (Priority: P1)

プロジェクト評価者として、3つのエージェントの憲法の違いをドキュメントサイトで閲覧し、
どのエージェントがソフトウェア開発に必要な情報を網羅しているか理解したい。

**Why this priority**: 比較結果をオンラインで閲覧できることが、このプロジェクトの主要な価値提供である。

**Independent Test**: ドキュメントサイトで比較ページにアクセスし、3つのエージェントの違いが一目で分かる表形式で表示されていることを確認できる。

**Acceptance Scenarios**:

1. **Given** ドキュメントサイトにアクセスしている, **When** 憲法比較ページに移動する, **Then** 3つのエージェントの比較表が表示される
2. **Given** 比較表を閲覧している, **When** 各セクションを確認する, **Then** コミットハッシュとURLが各エージェントに紐付けて表示されている
3. **Given** 比較表を閲覧している, **When** 評価セクションを確認する, **Then** 各エージェントの強み・弱みが相対比較で表示されている

---

### User Story 2 - Understand Software Development Completeness (Priority: P1)

開発者として、各エージェントの憲法がソフトウェア開発に必要な情報（技術スタック、
ワークフロー、品質基準など）をどの程度カバーしているか確認したい。

**Why this priority**: ソフトウェア開発の実用性判断に直結する重要な評価観点である。

**Independent Test**: 比較ページで「ソフトウェア開発必須要素」セクションを確認し、各エージェントのカバレッジが明確に分かる。

**Acceptance Scenarios**:

1. **Given** 比較ページを閲覧している, **When** 技術スタックセクションを確認する, **Then** 各エージェントが指定している技術（言語、フレームワーク、ツール）が一覧で表示される
2. **Given** 比較ページを閲覧している, **When** ワークフローセクションを確認する, **Then** 各エージェントの開発ワークフロー（ブランチ戦略、レビュー要件等）が比較表示される
3. **Given** 比較ページを閲覧している, **When** 品質基準セクションを確認する, **Then** テスト要件、リント設定、型チェック等の基準が比較表示される

---

### User Story 3 - Evaluate Relative Quality (Priority: P2)

プロジェクトマネージャーとして、3つのエージェントの憲法品質を相対評価し、
どのエージェントが最も完成度が高いか判断したい。

**Why this priority**: 評価結果は比較後の付加価値であり、まず比較データが揃っていることが前提。

**Independent Test**: 評価サマリーセクションで、各エージェントのスコアと順位が表示され、判断根拠が明示されている。

**Acceptance Scenarios**:

1. **Given** 比較ページを閲覧している, **When** 評価サマリーセクションを確認する, **Then** 各評価カテゴリ（完全性、明確性、実用性等）でスコアが表示される
2. **Given** 評価サマリーを確認している, **When** 詳細を展開する, **Then** スコアの根拠となる具体的な記述が引用表示される

---

### Edge Cases

- サブモジュールが未初期化の場合、比較ページで警告メッセージを表示
- 憲法ファイルが存在しないエージェントがある場合、該当エージェントは「N/A」と表示
- コミットハッシュが変更された場合、再比較が必要な旨を表示

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: システムは3つのエージェント（Codex, Gemini, Claude）の憲法を並列比較表示できなければならない
- **FR-002**: システムは各比較のベースとなったコミットハッシュとリポジトリURLを表示しなければならない
- **FR-003**: システムは以下のカテゴリで比較分析を提供しなければならない：
  - Core Principles（基本原則）
  - Technology Stack（技術スタック）
  - Development Workflow（開発ワークフロー）
  - Quality Standards（品質基準）
  - Governance（ガバナンス）
- **FR-004**: システムは各エージェントの強み・弱みを客観的基準（要素の有無および詳細度）に基づく相対評価で表示しなければならない
- **FR-005**: システムはソフトウェア開発必須要素のカバレッジを判定表示しなければならない

### Key Entities

- **Constitution**: 各エージェントの憲法文書。バージョン、原則、技術スタック、ワークフロー、ガバナンスを含む
- **Comparison Category**: 比較の観点（原則、技術、ワークフロー、品質、ガバナンス）
- **Evaluation Score**: 各カテゴリにおける相対評価スコア（優/良/可/不足）。客観的基準（要素の有無・詳細度）に基づいて判定
- **Commit Reference**: 比較ベースとなったコミット情報（ハッシュ、日時、URL）

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: ユーザーは30秒以内に3つのエージェントの主要な違いを把握できる
- **SC-002**: 比較表は全5カテゴリ（原則、技術、ワークフロー、品質、ガバナンス）をカバーしている
- **SC-003**: 各エージェントについて、コミットハッシュから1クリックでGitHubの該当コミットに遷移できる
- **SC-004**: 相対評価の根拠となる具体的な記述が各評価項目に紐付けて表示されている
- **SC-005**: ソフトウェア開発必須要素（技術スタック、テスト要件、コードスタイル、ワークフロー）の有無が明確に判定されている

## Clarifications

### Session 2025-12-26

- Q: How should the relative evaluation scores be determined? → A: Objective criteria (presence/absence of elements, detail level)

## Assumptions

- サブモジュールは `submodules/codex/`, `submodules/gemini/`, `submodules/claude/` に配置されている
- 各サブモジュールの憲法は `.specify/memory/constitution.md` に存在する
- ドキュメントサイトは MkDocs + Material for MkDocs で構築済み
- 比較結果は静的なドキュメントとして生成（動的な更新は対象外）
