# Data Model: MkDocs GitHub Pages Documentation Site

**Date**: 2025-12-25
**Feature**: 001-mkdocs-github-pages

## Overview

このドキュメントでは、MkDocsドキュメントサイトのデータ構造を定義します。
主にファイル構造、設定スキーマ、およびコンテンツ構造を含みます。

## Entities

### 1. Document Page

マークダウン形式のドキュメントページ。

| Field | Type | Description | Required |
|-------|------|-------------|----------|
| title | string | ページタイトル（H1または frontmatter） | Yes |
| path | string | docs/からの相対パス | Yes |
| content | markdown | ページ本文 | Yes |
| nav_order | integer | ナビゲーション内の順序 | No |

**Validation Rules**:
- パスは`.md`拡張子で終わる
- タイトルは空でない
- パスはdocs/配下に存在

### 2. Navigation Structure

ドキュメント間の階層関係を定義。

| Field | Type | Description | Required |
|-------|------|-------------|----------|
| section | string | セクション名 | Yes |
| items | array | 子ページまたはサブセクション | Yes |

**Constitution-Defined Sections** (必須):
1. Getting Started
2. Features
3. Agents
4. Methodology
5. Development
6. Reference

### 3. Site Configuration (mkdocs.yml)

MkDocsの設定ファイル。

| Field | Type | Description | Required |
|-------|------|-------------|----------|
| site_name | string | サイト名 | Yes |
| site_url | string | 公開URL | Yes |
| theme | object | テーマ設定 | Yes |
| nav | array | ナビゲーション構造 | Yes |
| plugins | array | 有効なプラグイン | No |
| markdown_extensions | array | Markdown拡張 | No |

## File Structure

### Documentation Root (docs/)

```
docs/
├── index.md                          # ホームページ
├── getting-started/
│   ├── installation.md               # インストール手順
│   ├── configuration.md              # 設定ガイド
│   └── quickstart.md                 # クイックスタート
├── features/
│   └── index.md                      # 機能概要
├── agents/
│   ├── index.md                      # エージェント比較概要
│   ├── codex/
│   │   └── index.md                  # Codexドキュメント
│   ├── gemini/
│   │   └── index.md                  # Geminiドキュメント
│   └── claude/
│       └── index.md                  # Claudeドキュメント
├── methodology/
│   ├── evaluation-criteria.md        # 評価基準
│   ├── metrics.md                    # メトリクス定義
│   └── benchmarks.md                 # ベンチマーク説明
├── development/
│   ├── contributing.md               # 貢献ガイドライン
│   ├── architecture.md               # アーキテクチャ
│   └── changelog.md                  # 変更履歴
└── reference/
    ├── cli.md                        # CLIリファレンス
    ├── api.md                        # APIリファレンス
    └── glossary.md                   # 用語集
```

### Configuration Files

```
/                                     # Repository root
├── mkdocs.yml                        # MkDocs configuration
└── .github/
    └── workflows/
        └── docs.yml                  # GitHub Actions workflow
```

## Configuration Schema

### mkdocs.yml

```yaml
site_name: Coding Agent Comparison
site_url: https://0h-n0.github.io/coding-agent-speckit-based-comparison/
site_description: Documentation for comparing coding agents
repo_url: https://github.com/0h-n0/coding-agent-speckit-based-comparison
repo_name: coding-agent-speckit-based-comparison

theme:
  name: material
  language: ja
  features:
    - navigation.tabs
    - navigation.sections
    - navigation.expand
    - search.suggest
    - search.highlight
    - content.code.copy
  palette:
    - scheme: default
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
    - scheme: slate
      primary: indigo
      accent: indigo
      toggle:
        icon: material/brightness-4
        name: Switch to light mode

plugins:
  - search

markdown_extensions:
  - pymdownx.highlight:
      anchor_linenums: true
  - pymdownx.superfences
  - admonition
  - tables

nav:
  - Home: index.md
  - Getting Started:
    - Installation: getting-started/installation.md
    - Configuration: getting-started/configuration.md
    - Quickstart: getting-started/quickstart.md
  - Features: features/index.md
  - Agents:
    - Overview: agents/index.md
    - Codex: agents/codex/index.md
    - Gemini: agents/gemini/index.md
    - Claude: agents/claude/index.md
  - Methodology:
    - Evaluation Criteria: methodology/evaluation-criteria.md
    - Metrics: methodology/metrics.md
    - Benchmarks: methodology/benchmarks.md
  - Development:
    - Contributing: development/contributing.md
    - Architecture: development/architecture.md
    - Changelog: development/changelog.md
  - Reference:
    - CLI: reference/cli.md
    - API: reference/api.md
    - Glossary: reference/glossary.md
```

### GitHub Actions Workflow (.github/workflows/docs.yml)

```yaml
name: Deploy Documentation

on:
  push:
    branches:
      - main
    paths:
      - 'docs/**'
      - 'mkdocs.yml'
  workflow_dispatch:

permissions:
  contents: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
          submodules: true

      - uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          pip install mkdocs-material

      - name: Build and deploy
        run: mkdocs gh-deploy --force
```

## State Transitions

ドキュメントサイトの状態遷移:

```
[No Site] → [Local Build] → [Deployed]
              ↑                  ↓
              └── [Updated] ←────┘
```

| State | Description | Trigger |
|-------|-------------|---------|
| No Site | gh-pagesブランチが存在しない | 初期状態 |
| Local Build | mkdocs buildでローカルビルド成功 | mkdocs build |
| Deployed | GitHub Pagesで公開中 | mkdocs gh-deploy or CI |
| Updated | 新しいコンテンツがデプロイ済み | mainへのpush |
