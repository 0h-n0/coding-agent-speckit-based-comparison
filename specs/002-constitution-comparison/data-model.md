# Data Model: Constitution Comparison Analysis

**Feature**: 002-constitution-comparison
**Date**: 2025-12-26

## Overview

This feature produces static documentation only. No runtime data persistence is required.
The "data model" describes the conceptual entities represented in the comparison documentation.

## Entities

### Agent

Represents a coding agent whose constitution is being compared.

| Field | Type | Description |
|-------|------|-------------|
| name | string | Agent identifier (Codex, Gemini, Claude) |
| repository | URL | GitHub repository URL |
| commit | string | Commit hash at comparison time |
| commit_date | datetime | Commit timestamp |
| constitution_version | string | Semantic version from constitution |
| constitution_path | string | Path to constitution file in submodule |

**Instances**:
- Codex: `submodules/codex/.specify/memory/constitution.md`
- Gemini: `submodules/gemini/.specify/memory/constitution.md`
- Claude: `submodules/claude/.specify/memory/constitution.md`

### ComparisonCategory

Defines a dimension for comparing constitutions.

| Field | Type | Description |
|-------|------|-------------|
| id | string | Category identifier |
| name_en | string | English name |
| name_ja | string | Japanese name |
| description | string | What this category evaluates |
| elements | Element[] | List of elements to check |

**Instances**:
1. `core_principles` - Core Principles / 基本原則
2. `technology_stack` - Technology Stack / 技術スタック
3. `development_workflow` - Development Workflow / 開発ワークフロー
4. `quality_standards` - Quality Standards / 品質基準
5. `governance` - Governance / ガバナンス

### Element

An evaluatable item within a comparison category.

| Field | Type | Description |
|-------|------|-------------|
| name | string | Element name |
| description | string | What to look for |
| required | boolean | Whether this is a core requirement |

**Examples by Category**:

**Core Principles**:
- Privacy/Security
- Performance Targets
- Simplicity/YAGNI
- Test-Driven Development
- Modularity
- Observability
- Semantic Versioning
- Scriptable Interfaces

**Technology Stack**:
- Frontend Framework
- Backend Framework
- Package Manager
- Type Validation
- Linting
- Type Checking
- Testing Framework
- Project Structure

**Development Workflow**:
- Branch Strategy
- PR Requirement
- Code Review
- README Update
- Commit Format
- Merge Strategy

**Quality Standards**:
- Test Requirements
- Linting Rules
- Type Checking
- Coverage Requirements
- Frontend Quality
- Integration Tests
- Contract Tests

**Governance**:
- Amendment Process
- Versioning Policy
- Compliance Review
- Exception Handling
- Template Sync

### EvaluationScore

Score assigned to an agent for a specific category.

| Field | Type | Description |
|-------|------|-------------|
| agent | Agent | Reference to agent |
| category | ComparisonCategory | Reference to category |
| score | enum | 優/良/可/不足 |
| score_en | enum | Excellent/Good/Acceptable/Insufficient |
| evidence | string[] | Quotes or references supporting the score |

**Scoring Criteria** (from research.md):

| Score | Japanese | English | Criteria |
|-------|----------|---------|----------|
| 優 | ゆう | Excellent | Element present with comprehensive detail, rationale, and examples |
| 良 | りょう | Good | Element present with adequate detail |
| 可 | か | Acceptable | Element mentioned but lacking detail |
| 不足 | ふそく | Insufficient | Element missing or critically incomplete |

### ComparisonResult

Aggregated comparison result for documentation display.

| Field | Type | Description |
|-------|------|-------------|
| baseline_date | date | Date comparison was performed |
| agents | Agent[] | List of compared agents |
| categories | ComparisonCategory[] | Categories evaluated |
| scores | EvaluationScore[] | All scores |
| summary | string | Executive summary |
| strengths | map[Agent, string[]] | Identified strengths per agent |
| weaknesses | map[Agent, string[]] | Identified weaknesses per agent |

## Relationships

```
Agent (1) ----< (N) EvaluationScore
ComparisonCategory (1) ----< (N) EvaluationScore
ComparisonCategory (1) ----< (N) Element
ComparisonResult (1) ----< (N) Agent
ComparisonResult (1) ----< (N) EvaluationScore
```

## Data Flow

1. **Input**: Raw constitution markdown files from submodules
2. **Analysis**: Manual comparison produces research.md
3. **Documentation**: Comparison results rendered as MkDocs pages
4. **Output**: Static HTML via `mkdocs build`

## Validation Rules

- Agent.commit MUST be a valid 40-character hex string
- Agent.repository MUST be a valid HTTPS URL
- EvaluationScore.score MUST be one of: 優, 良, 可, 不足
- All agents MUST have scores for all categories
- Evidence MUST reference specific constitution sections
