# Research: LLM Implementation Comparison

**Feature**: 004-llm-implementation-comparison
**Date**: 2025-12-26

## Research Summary

This research documents the analysis of LLM service implementations across three coding agents. All data was collected from the submodule repositories at their current HEAD commits.

## Decision 1: Documentation Structure

**Decision**: Follow the established STT comparison structure

**Rationale**:
- Maintains consistency across the documentation site
- Users familiar with STT comparison can easily navigate LLM comparison
- Reduces cognitive load for readers comparing multiple features
- Reuses proven information architecture

**Alternatives Considered**:
- New standalone structure: Rejected (breaks consistency)
- Merged single-page comparison: Rejected (too long, harder to navigate)

## Decision 2: Comparison Dimensions

**Decision**: Compare on 6 dimensions: Spec Features, Implementation Structure, API Design, Error Handling, Chat History, Test Coverage

**Rationale**:
- These dimensions capture the most meaningful differences between implementations
- Each dimension has measurable/observable attributes
- Aligned with the STT comparison dimensions for consistency

**Alternatives Considered**:
- Performance benchmarks: Deferred (requires runtime testing, out of scope)
- Security audit: Deferred (requires separate security review)

## Decision 3: Alignment Scoring Method

**Decision**: Score each Functional Requirement as Implemented (✅), Partial (⚠️), or Not Implemented (❌)

**Rationale**:
- Simple, understandable scoring
- Consistent with STT comparison methodology
- Enables percentage-based summary statistics

**Alternatives Considered**:
- Weighted scoring: Rejected (adds complexity without clear benefit)
- Binary only: Rejected (loses nuance for partially implemented features)

## Submodule Analysis Results

### Source Commits

| Agent | Commit | Repository |
|-------|--------|------------|
| Codex | d77c272 | local-voice-assistant-codex |
| Gemini | 361573e | local-voice-assistant-gemini |
| Claude | aa6d1e3 | local-voice-assistant-claude |

### Specification Paths

| Agent | Spec Path | Feature ID |
|-------|-----------|------------|
| Codex | `specs/001-llm-service/spec.md` | 001-llm-service |
| Gemini | `specs/003-llm-service/spec.md` | 003-llm-service |
| Claude | `specs/003-llm-text-service/spec.md` | 003-llm-text-service |

### Implementation Paths

| Agent | API File | Service File | Test Files |
|-------|----------|--------------|------------|
| Codex | `backend/app/api/llm.py` | `backend/app/services/llm_client.py` | `tests/test_llm_*.py` |
| Gemini | `backend/src/api/v1/endpoints/llm.py` | `backend/src/core/llm/service.py` | `tests/integration/test_llm.py` |
| Claude | `backend/src/api/llm.py` | `backend/src/services/llm_service.py` | `tests/*/test_llm_*.py` |

### Code Metrics

| Agent | Implementation Lines | Test Lines | Total |
|-------|---------------------|------------|-------|
| Codex | 91 | 51 | 142 |
| Gemini | 174 | 119 | 293 |
| Claude | 678 | 615 | 1293 |

### Key Feature Differences

| Feature | Codex | Gemini | Claude |
|---------|-------|--------|--------|
| Streaming (SSE) | ❌ | ✅ | ❌ |
| Chat History | ❌ | ✅ | ✅ |
| Parameter Config (temp, max_tokens) | ❌ | ✅ | ❌ |
| Status Endpoint | ❌ | ❌ | ✅ |
| Conversation Clear | ❌ | ❌ | ✅ |
| Concurrent Request Limiting | ❌ | ❌ | ✅ |
| Fixed System Prompt | ❌ | ❌ | ✅ |
| API Versioning | ❌ | ✅ | ❌ |

## Clarifications Resolved

No clarifications were needed. All technical decisions follow established patterns from STT comparison.

## Open Items

None. Ready to proceed to Phase 1.
