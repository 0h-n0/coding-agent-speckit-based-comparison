# Research: TTS Implementation Comparison

**Feature**: 005-tts-implementation-comparison
**Date**: 2025-12-27

## Overview

This document consolidates research findings for the TTS implementation comparison documentation feature.

## Technology Decisions

### Documentation Format

**Decision**: Markdown with MkDocs Material theme
**Rationale**: Consistent with existing STT and LLM comparison documentation
**Alternatives Considered**: None - established pattern must be followed

### Comparison Structure

**Decision**: Follow STT/LLM comparison format with main page + agent-specific pages
**Rationale**: FR-011 requires consistency with existing comparisons
**Alternatives Considered**: Single-page comparison (rejected for scalability)

## TTS Specification Analysis

### Codex TTS Spec (001-tts-api)

| Aspect | Details |
|--------|---------|
| User Stories | 3 (P1: Text-to-Speech, P2: Voice Config, P3: Operational Visibility) |
| Functional Requirements | 7 (FR-001 to FR-007) |
| Audio Format | WAV |
| Max Text Length | 1,000 characters |
| Streaming | Not specified |
| Authentication | Not specified |
| Key Features | Voice selection, health endpoint, structured errors |

### Gemini TTS Spec (004-tts-japanese-api)

| Aspect | Details |
|--------|---------|
| User Stories | 3 (P1: Voice Synthesis, P2: Style Control, P2: Parameter Tuning) |
| Functional Requirements | 10 (FR-001 to FR-010) |
| Audio Format | WAV |
| Max Text Length | 500 characters |
| Streaming | Yes (batch and streaming modes) |
| Authentication | API Key required |
| Key Features | Style/emotion control, voice parameters, Prometheus metrics |

### Claude TTS Spec (004-tts-api)

| Aspect | Details |
|--------|---------|
| User Stories | 3 (P1: Text-to-Speech, P2: Parameter Customization, P3: Health Check) |
| Functional Requirements | 7 (FR-001 to FR-007) |
| Audio Format | WAV |
| Max Text Length | 5,000 characters |
| Streaming | Not supported (out of scope) |
| Authentication | Not specified |
| Key Features | Speed parameter, processing time tracking, concurrent request limiting |

## Implementation Analysis

### Code Structure Comparison

| Agent | API Layer | Service Layer | Models | Tests | Total |
|-------|-----------|---------------|--------|-------|-------|
| Codex | 37 lines | 21 lines | 8 lines | 33 lines | 99 lines |
| Gemini | 44 lines | - | 24 lines (+66 deps/CLI) | 0 lines | 134 lines |
| Claude | 161 lines | 235 lines | 107 lines | 364 lines | 867 lines |

### API Endpoint Comparison

| Agent | Synthesize Path | Method | Additional Endpoints |
|-------|-----------------|--------|---------------------|
| Codex | `/tts/synthesize` | POST | Health (referenced) |
| Gemini | `/api/v1/tts/synthesize` | POST | `/api/v1/tts/models` (GET) |
| Claude | `/api/tts/synthesize` | POST | `/api/tts/status` (GET) |

### Feature Support Matrix

| Feature | Codex | Gemini | Claude |
|---------|-------|--------|--------|
| Basic Synthesis | ✅ | ✅ | ✅ |
| Voice Selection | ✅ | ✅ | ❌ |
| Style/Emotion | ❌ | ✅ | ❌ |
| Speed Control | ❌ | ✅ | ✅ |
| Streaming | ❌ | ✅ | ❌ |
| API Authentication | ❌ | ✅ | ❌ |
| Health/Status | ✅ | ❌ | ✅ |
| Metrics/Logging | ❌ | ✅ | ✅ |

### Error Handling Patterns

| Agent | Pattern | Error Codes | HTTP Status |
|-------|---------|-------------|-------------|
| Codex | ValueError catch | `unsupported_voice`, `empty_text`, `text_too_long` | 400, 500 |
| Gemini | HTTPException | Generic (via `detail`) | 400, 500 |
| Claude | Enum-based TTSErrorCode | `EMPTY_TEXT`, `TEXT_TOO_LONG`, `MODEL_NOT_LOADED`, `SERVICE_BUSY`, `SYNTHESIS_FAILED` | 400, 500, 503 |

### Test Coverage Analysis

| Agent | Unit Tests | Integration Tests | Contract Tests | Total |
|-------|------------|-------------------|----------------|-------|
| Codex | 3 files | 0 files | 0 files | 33 lines |
| Gemini | 0 files | 0 files | 0 files | 0 lines |
| Claude | 1 file | 1 file | 1 file | 364 lines |

## Key Findings

1. **Specification Richness**: Gemini has the most comprehensive spec (10 FRs) with advanced features like streaming and style control
2. **Implementation Volume**: Claude has the largest implementation (867 lines) with comprehensive test coverage
3. **Test Coverage**: Claude is the only agent with tests; Gemini has no tests despite feature richness
4. **Feature Parity**: All agents implement basic synthesis; advanced features vary significantly
5. **API Design**: All use RESTful patterns; Gemini has versioned API (`/api/v1/`)

## Comparison with Previous Features

| Metric | STT | LLM | TTS |
|--------|-----|-----|-----|
| Codex Total Lines | 110 | 142 | 99 |
| Gemini Total Lines | 282 | 293 | 134 |
| Claude Total Lines | 656 | 1293 | 867 |
| Streaming Support | Codex only | Gemini only | Gemini only |
