# Research: Constitution Comparison Analysis

**Feature**: 002-constitution-comparison
**Date**: 2025-12-26

## Executive Summary

This research documents the comparison methodology for evaluating three coding agent constitutions
(Codex, Gemini, Claude) based on objective criteria. The analysis identifies what each constitution
covers for software development purposes and establishes a fair evaluation framework.

## Comparison Baseline

| Agent | Version | Commit | Repository |
|-------|---------|--------|------------|
| Codex | 1.1.0 | [5c2031e](https://github.com/0h-n0/local-voice-assistant-codex/commit/5c2031e347e42f8eefde569db2d1ad513c0a5564) | local-voice-assistant-codex |
| Gemini | 1.2.0 | [5f9064f](https://github.com/0h-n0/local-voice-assistant-gemini/commit/5f9064f672641534adee80dd9fcb3a60c9b48527) | local-voice-assistant-gemini |
| Claude | 1.1.0 | [7a68ec6](https://github.com/0h-n0/local-voice-assistant-claude/commit/7a68ec67e66b30620dfcbe462698f8b311dabc8b) | local-voice-assistant-claude |

## Evaluation Methodology

### Decision: Objective Criteria Scoring

**Rationale**: Per clarification session, scoring is based on objective criteria (presence/absence
of elements, detail level) rather than subjective quality assessment.

**Scoring Scale**:
| Score | Japanese | Criteria |
|-------|----------|----------|
| 優 (Excellent) | ゆう | Element present with comprehensive detail, rationale, and examples |
| 良 (Good) | りょう | Element present with adequate detail |
| 可 (Acceptable) | か | Element mentioned but lacking detail |
| 不足 (Insufficient) | ふそく | Element missing or critically incomplete |

**Alternatives Considered**:
1. Subjective quality assessment - Rejected: Not reproducible, invites bias accusations
2. Weighted rubric scoring - Rejected: Adds complexity without clear benefit for documentation comparison

## Category Analysis

### 1. Core Principles

| Element | Codex | Gemini | Claude |
|---------|-------|--------|--------|
| Privacy/Security | ✅ Principle I (Local-First Privacy) | ⚠️ Security section (brief) | ✅ Principle I (Privacy-First, NON-NEGOTIABLE) |
| Performance | ❌ Not specified | ❌ Not specified | ✅ Principle II (Performance-Conscious with targets) |
| Simplicity/YAGNI | ❌ Not specified | ✅ Principle VII (Simplicity/YAGNI) | ✅ Principle III (Simplicity-First) |
| Test-Driven Development | ✅ Principle III (Reliability) | ✅ Principle III (Test-First) | ✅ Principle IV (TDD, NON-NEGOTIABLE) |
| Modularity | ❌ Not specified | ✅ Principle I (Library-First) | ✅ Principle V (Modular Architecture) |
| Observability | ✅ Principle IV | ✅ Principle V | ✅ Principle VI |
| Semantic Versioning | ✅ Principle V | ✅ Principle VI | ❌ Implied in amendment process |
| Scriptable Interfaces | ✅ Principle II | ✅ Principle II (CLI Interface) | ❌ Not specified |

**Findings**:
- Claude has the most detailed principles with explicit NON-NEGOTIABLE markers
- Gemini has unique Library-First approach
- Codex emphasizes Local-First Privacy and Scriptable Interfaces
- Claude includes specific performance targets (<500ms latency)

### 2. Technology Stack

| Element | Codex | Gemini | Claude |
|---------|-------|--------|--------|
| Frontend Framework | ✅ React/Next + TypeScript | ✅ Next.js + React + TypeScript | ✅ Next.js + React + TypeScript |
| Backend Framework | ✅ FastAPI | ✅ FastAPI | ✅ FastAPI |
| Package Manager | ✅ uv | ✅ uv | ✅ uv |
| Type Validation | ✅ Pydantic | ✅ Pydantic | ✅ Pydantic v2 |
| Linting | ✅ Ruff | ✅ Ruff | ✅ Ruff |
| Type Checking | ❌ Not specified | ❌ Not specified | ✅ mypy (strict) |
| Testing Framework | ❌ Not specified | ❌ Not specified | ✅ pytest, Jest + RTL |
| Project Structure | ❌ Not documented | ❌ Not documented | ✅ Detailed directory tree |

**Findings**:
- All three agents specify the same core stack (React/Next, FastAPI, uv, Pydantic, Ruff)
- Claude provides the most comprehensive stack specification including mypy strict mode
- Claude includes detailed project structure template
- Codex and Gemini lack testing framework specification

### 3. Development Workflow

| Element | Codex | Gemini | Claude |
|---------|-------|--------|--------|
| Branch Strategy | ✅ Feature branch required | ✅ Branch creation required | ✅ Feature branch (NON-NEGOTIABLE) |
| PR Requirement | ✅ Required | ✅ Required with review | ✅ Required with compliance checklist |
| Code Review | ⚠️ Mentioned but not detailed | ✅ Required (1+ approver) | ✅ Detailed requirements |
| README Update | ✅ Required | ✅ Required | ✅ Required |
| Commit Format | ❌ Not specified | ❌ Not specified | ✅ Conventional commits |
| Merge Strategy | ❌ Not specified | ❌ Not specified | ✅ Squash merge preferred |

**Findings**:
- Claude has the most detailed workflow with explicit NON-NEGOTIABLE designation
- All require feature branches and PR workflow
- Claude specifies commit message format and merge strategy
- Codex and Gemini lack commit format guidance

### 4. Quality Standards

| Element | Codex | Gemini | Claude |
|---------|-------|--------|--------|
| Test Requirements | ✅ Automated tests for critical paths | ✅ TDD required | ✅ TDD (NON-NEGOTIABLE), 3 test types |
| Linting Rules | ✅ Ruff enforced | ✅ Ruff required | ✅ Ruff zero warnings |
| Type Checking | ❌ Not specified | ✅ Pydantic for runtime | ✅ mypy strict + Pydantic |
| Coverage Requirements | ❌ Not specified | ❌ Not specified | ✅ Coverage must not decrease |
| Frontend Quality | ❌ Not specified | ❌ Not specified | ✅ ESLint, Prettier, tsc |
| Integration Tests | ⚠️ Mentioned | ✅ Principle IV | ✅ Required |
| Contract Tests | ❌ Not specified | ❌ Not specified | ✅ Required |

**Findings**:
- Claude provides the most comprehensive quality standards
- Claude specifies 3 test types (unit, integration, contract)
- Claude includes frontend-specific quality requirements
- Codex and Gemini lack frontend quality specifications

### 5. Governance

| Element | Codex | Gemini | Claude |
|---------|-------|--------|--------|
| Amendment Process | ✅ Written proposal required | ✅ PR-based amendment | ✅ Detailed 5-step process |
| Versioning Policy | ✅ MAJOR/MINOR/PATCH defined | ❌ Not specified | ✅ MAJOR/MINOR/PATCH defined |
| Compliance Review | ✅ Required for specs/plans | ⚠️ Implicit in governance | ✅ Required for all PRs |
| Exception Handling | ✅ Documented exceptions | ✅ Explicit justification | ✅ Complexity Tracking section |
| Template Sync | ✅ Impact report header | ❌ Not specified | ✅ SYNC IMPACT REPORT |

**Findings**:
- Codex and Claude have similar governance structures
- Claude has the most detailed amendment process
- Gemini lacks explicit versioning policy
- Codex and Claude use SYNC IMPACT REPORT headers

## Software Development Completeness Assessment

| Requirement | Codex | Gemini | Claude |
|-------------|-------|--------|--------|
| **Technology Stack** | ✅ | ✅ | ✅ |
| **Test Requirements** | ⚠️ Partial | ✅ | ✅ |
| **Code Style** | ✅ | ✅ | ✅ |
| **Workflow** | ⚠️ Partial | ⚠️ Partial | ✅ |
| **Project Structure** | ❌ | ❌ | ✅ |
| **Quality Gates** | ⚠️ Partial | ⚠️ Partial | ✅ |

## Relative Evaluation Summary

### Overall Scores

| Category | Codex | Gemini | Claude |
|----------|-------|--------|--------|
| Core Principles | 良 | 良 | 優 |
| Technology Stack | 良 | 良 | 優 |
| Development Workflow | 可 | 良 | 優 |
| Quality Standards | 可 | 良 | 優 |
| Governance | 良 | 可 | 優 |
| **Overall** | **良** | **良** | **優** |

### Strengths

**Codex**:
- Strong privacy-first approach with explicit local-only data handling
- Unique emphasis on scriptable/deterministic interfaces
- Good observability requirements with user control

**Gemini**:
- Clean Library-First architecture principle
- Strong YAGNI/Simplicity emphasis
- Integration testing as explicit principle

**Claude**:
- Most comprehensive and detailed constitution
- Explicit NON-NEGOTIABLE markers for critical requirements
- Specific performance targets (e.g., <500ms latency)
- Complete project structure template
- Frontend-specific quality requirements
- Detailed workflow with merge strategy

### Weaknesses

**Codex**:
- Missing project structure guidance
- No commit message format
- Limited frontend quality requirements
- No coverage requirements

**Gemini**:
- Missing versioning policy
- No project structure template
- No commit message format
- Missing frontend quality requirements

**Claude**:
- No explicit scriptable interface requirement
- Semantic versioning only implied in amendment section
- Most complex/verbose constitution (could be seen as overhead)

## Conclusions

1. **For software development completeness**: Claude provides the most comprehensive guidance
2. **For privacy-focused projects**: Codex offers the strongest privacy-first approach
3. **For modular library development**: Gemini's Library-First principle is well-suited
4. **All three share**: React/Next + FastAPI + uv + Pydantic + Ruff stack

The comparison data is now ready for documentation generation in Phase 1.
