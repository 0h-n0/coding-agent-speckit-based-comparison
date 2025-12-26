# Contracts

This feature (Constitution Comparison Analysis) does not require API contracts.

## Rationale

This is a documentation-only feature that produces static Markdown pages for the
existing MkDocs site. There are no:

- REST APIs
- GraphQL endpoints
- WebSocket connections
- External service integrations
- Data import/export endpoints

## Data Contracts

The "contracts" for this feature are the documentation structures defined in
`data-model.md`:

- Agent entity structure
- ComparisonCategory definitions
- EvaluationScore format (優/良/可/不足)
- Comparison result schema

## Output Format

The feature produces:

1. **Markdown files** in `docs/` directory
2. **Navigation updates** in `mkdocs.yml`
3. **Static HTML** via `mkdocs build`

All output follows MkDocs-compatible Markdown format.
