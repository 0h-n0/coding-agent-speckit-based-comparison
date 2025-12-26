# Data Model: LLM Implementation Comparison

**Feature**: 004-llm-implementation-comparison
**Date**: 2025-12-26

## Overview

This document defines the data structures used in the LLM comparison documentation. Since this is a documentation feature, the "data model" represents the structure of comparison tables and analysis sections.

## Entities

### ComparisonBaseline

Represents the source data for comparison.

| Field | Type | Description |
|-------|------|-------------|
| agent | string | Agent name (Codex, Gemini, Claude) |
| commit | string | Git commit hash |
| repository | string | Repository name |
| spec_path | string | Path to spec.md within submodule |
| feature_id | string | Speckit feature ID |

### SpecificationMetrics

Represents specification-level analysis.

| Field | Type | Description |
|-------|------|-------------|
| agent | string | Agent name |
| fr_count | integer | Number of functional requirements |
| sc_count | integer | Number of success criteria |
| user_story_count | integer | Number of user stories |
| has_streaming | boolean | Spec mentions streaming support |
| has_chat_history | boolean | Spec mentions chat history |
| has_parameter_config | boolean | Spec mentions temperature/max_tokens |

### ImplementationMetrics

Represents implementation-level analysis.

| Field | Type | Description |
|-------|------|-------------|
| agent | string | Agent name |
| api_file | string | Path to API implementation |
| service_file | string | Path to service layer |
| impl_lines | integer | Lines of implementation code |
| test_lines | integer | Lines of test code |
| total_lines | integer | Total lines |
| api_path | string | API endpoint path |
| has_streaming | boolean | Implementation supports streaming |
| has_chat_history | boolean | Implementation supports chat history |
| has_status_endpoint | boolean | Has health/status endpoint |

### AlignmentResult

Represents spec-to-implementation alignment.

| Field | Type | Description |
|-------|------|-------------|
| agent | string | Agent name |
| fr_id | string | Functional requirement ID |
| requirement | string | Requirement description |
| status | enum | IMPLEMENTED, PARTIAL, NOT_IMPLEMENTED |
| evidence | string | Code reference or explanation |

### OverallEvaluation

Represents summary evaluation.

| Field | Type | Description |
|-------|------|-------------|
| agent | string | Agent name |
| code_volume | integer | Total lines |
| test_coverage | string | Rating (優/良/可) |
| error_handling | string | Rating |
| api_design | string | Rating |
| alignment_rate | float | Percentage (0-100) |
| overall_rating | string | Rating |

## Relationships

```text
ComparisonBaseline 1--* SpecificationMetrics
ComparisonBaseline 1--* ImplementationMetrics
ImplementationMetrics 1--* AlignmentResult
AlignmentResult *--1 OverallEvaluation
```

## Document Structure

The comparison documentation follows this hierarchy:

```text
llm-comparison.md (main)
├── Overview
├── Comparison Baseline (ComparisonBaseline)
├── Specification Comparison (SpecificationMetrics)
│   ├── Functional Requirements Comparison
│   └── Success Criteria Comparison
├── Implementation Comparison (ImplementationMetrics)
│   ├── Code Structure
│   ├── API Endpoint Design
│   ├── Error Handling Pattern
│   └── Test Coverage
├── Spec-to-Implementation Alignment (AlignmentResult)
│   ├── Codex Alignment
│   ├── Gemini Alignment
│   └── Claude Alignment
├── Overall Evaluation (OverallEvaluation)
└── Key Findings and Conclusions
```
