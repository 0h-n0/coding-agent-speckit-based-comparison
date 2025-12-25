# Research: MkDocs GitHub Pages Documentation Site

**Date**: 2025-12-25
**Feature**: 001-mkdocs-github-pages

## Overview

この機能にはNEEDS CLARIFICATIONはありません。技術スタックは憲法で明確に定義されており、
MkDocsとMaterial for MkDocsテーマを使用することが決定しています。

## Research Areas

### 1. MkDocs + Material Theme Setup

**Decision**: MkDocs with mkdocs-material テーマを使用

**Rationale**:
- 憲法のDocumentation Standardsセクションで「Material for MkDocs (recommended)」と明記
- Material for MkDocsは最も人気のあるMkDocsテーマで、以下の機能を標準で提供:
  - 全文検索機能
  - レスポンシブデザイン
  - シンタックスハイライト
  - ダークモード対応
  - 階層的ナビゲーション

**Alternatives Considered**:
- MkDocs default theme: 機能が限定的、検索機能が基本的
- Read the Docs theme: 良いが、Material ほど機能豊富ではない
- Sphinx: より複雑、Python専用ドキュメント向け

### 2. GitHub Pages Deployment Strategy

**Decision**: GitHub Actions + `gh-pages` ブランチへのデプロイ

**Rationale**:
- GitHub Actionsは無料で利用可能
- `gh-pages`ブランチへのデプロイはGitHub Pagesの標準パターン
- `mkdocs gh-deploy`コマンドで簡単にデプロイ可能
- ソースコード（main）と生成サイト（gh-pages）を分離

**Alternatives Considered**:
- GitHub Pages from docs/ folder: mainブランチに生成ファイルが混在
- Netlify/Vercel: 外部サービス依存、GitHub外にデプロイ設定
- Manual deployment: 自動化の原則に反する

### 3. Project Structure for Documentation

**Decision**: 憲法定義の構造に完全準拠

**Rationale**:
- 憲法v1.1.1のDocumentation Standardsセクションで詳細な構造が定義済み
- 6つのメインセクション: getting-started, features, agents, methodology, development, reference
- 各エージェント（codex, gemini, claude）に専用ディレクトリ

**Structure Implementation**:
```
docs/
├── index.md
├── getting-started/
├── features/
├── agents/
│   ├── codex/
│   ├── gemini/
│   └── claude/
├── methodology/
├── development/
└── reference/
```

### 4. MkDocs Plugins and Extensions

**Decision**: 最小限のプラグイン構成から開始

**Rationale**:
- シンプルさの原則（憲法 III. Simplicity-First と類似概念）
- 必要に応じて後から追加可能

**Selected Plugins**:
- `search`: 全文検索（Material テーマに内蔵）
- `pymdownx.highlight`: シンタックスハイライト
- `pymdownx.superfences`: コードブロック拡張

**Deferred Plugins** (必要時に追加):
- `mkdocs-git-revision-date-plugin`: 最終更新日表示
- `mkdocs-minify-plugin`: HTML/CSS/JS圧縮

### 5. GitHub Actions Workflow

**Decision**: 標準的な MkDocs デプロイワークフロー

**Rationale**:
- 公式ドキュメントで推奨されているパターン
- mainブランチへのプッシュ時に自動ビルド・デプロイ

**Workflow Steps**:
1. Checkout repository
2. Setup Python
3. Install dependencies (mkdocs, mkdocs-material)
4. Build site (`mkdocs build`)
5. Deploy to gh-pages branch

## Resolved Clarifications

| Item | Resolution |
|------|------------|
| Theme selection | Material for MkDocs (憲法で推奨) |
| Deployment method | GitHub Actions → gh-pages branch |
| Document structure | 憲法定義の階層構造に準拠 |
| Plugin selection | 最小限から開始、必要時に追加 |

## Dependencies

| Dependency | Version | Purpose |
|------------|---------|---------|
| mkdocs | >= 1.5 | Static site generator |
| mkdocs-material | >= 9.0 | Theme with search, responsive design |
| Python | >= 3.11 | MkDocs runtime |

## Next Steps

1. Phase 1: データモデル（docs/構造）の詳細定義
2. Phase 1: mkdocs.yml設定ファイルの設計
3. Phase 1: GitHub Actionsワークフローの設計
