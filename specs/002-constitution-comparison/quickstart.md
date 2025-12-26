# Quickstart: Constitution Comparison Analysis

## Overview

Add a comprehensive comparison of three coding agent constitutions (Codex, Gemini, Claude)
to the documentation site.

## Prerequisites

- Existing MkDocs site configured (`mkdocs.yml` present)
- Git submodules initialized (`git submodule update --init --recursive`)
- MkDocs with Material theme installed

## Quick Implementation Steps

### 1. Create Main Comparison Page

Create `docs/methodology/constitution-comparison.md`:

```markdown
# 憲法比較分析

3つのコーディングエージェント（Codex, Gemini, Claude）の憲法を比較分析します。

## 比較ベースライン

| Agent | Version | Commit |
|-------|---------|--------|
| Codex | 1.1.0 | [5c2031e](link) |
| Gemini | 1.2.0 | [5f9064f](link) |
| Claude | 1.1.0 | [7a68ec6](link) |

## 評価サマリー

| Category | Codex | Gemini | Claude |
|----------|-------|--------|--------|
| Core Principles | 良 | 良 | 優 |
| Technology Stack | 良 | 良 | 優 |
| Development Workflow | 可 | 良 | 優 |
| Quality Standards | 可 | 良 | 優 |
| Governance | 良 | 可 | 優 |
```

### 2. Create Agent-Specific Constitution Pages

For each agent, create `docs/agents/{agent}/constitution.md`:

```markdown
# {Agent} 憲法分析

## 概要

- バージョン: X.Y.Z
- コミット: [hash](url)
- 最終更新: YYYY-MM-DD

## 原則一覧

1. Principle I: ...
2. Principle II: ...

## 強み

- ...

## 弱み

- ...
```

### 3. Update Navigation

Add to `mkdocs.yml`:

```yaml
nav:
  - Methodology:
    - Constitution Comparison: methodology/constitution-comparison.md
  - Agents:
    - Codex:
      - Constitution: agents/codex/constitution.md
    - Gemini:
      - Constitution: agents/gemini/constitution.md
    - Claude:
      - Constitution: agents/claude/constitution.md
```

### 4. Verify Build

```bash
mkdocs build --strict
mkdocs serve  # Local preview at http://127.0.0.1:8000
```

### 5. Deploy

Push to main branch to trigger GitHub Actions deployment.

## File Structure

```
docs/
├── methodology/
│   └── constitution-comparison.md  # Main comparison page
└── agents/
    ├── codex/
    │   └── constitution.md         # Codex analysis
    ├── gemini/
    │   └── constitution.md         # Gemini analysis
    └── claude/
        └── constitution.md         # Claude analysis
```

## Evaluation Criteria

Use objective scoring based on element presence:

| Score | Criteria |
|-------|----------|
| 優 (Excellent) | Comprehensive detail with rationale and examples |
| 良 (Good) | Adequate detail |
| 可 (Acceptable) | Mentioned but lacking detail |
| 不足 (Insufficient) | Missing or critically incomplete |

## Reference

- Full comparison data: [research.md](./research.md)
- Data model: [data-model.md](./data-model.md)
- Feature spec: [spec.md](./spec.md)
