<!--
SYNC IMPACT REPORT
==================
Version change: 1.1.0 → 1.1.1 (PATCH: submodule update requirement added)
Modified principles: None
Modified sections:
  - Branch & PR Workflow: Added step 0 for submodule update requirement
Added sections: None
Removed sections: None
Templates checked:
  - .specify/templates/plan-template.md ✅ (Constitution Check section present)
  - .specify/templates/spec-template.md ✅ (Requirements structure compatible)
  - .specify/templates/tasks-template.md ✅ (Task organization compatible)
  - .specify/templates/checklist-template.md ✅ (Checklist structure compatible)
Follow-up TODOs: None
-->

# Coding Agent Comparison Constitution

## Core Principles

### I. Extensibility

The system MUST be designed for easy addition of new coding agents, test cases, and
evaluation criteria without requiring changes to core comparison logic.

- New agents MUST be addable via configuration or plugin mechanism
- Test case formats MUST be documented and follow a consistent schema
- Evaluation criteria MUST be modular and independently configurable
- Breaking changes to extension points require MAJOR version bump

**Rationale**: A comparison tool is only valuable if it can adapt to the rapidly
evolving landscape of coding agents. Rigid architecture limits utility.

### II. Transparency

All methodology, metrics, scoring criteria, and evaluation processes MUST be
explicitly documented and publicly accessible.

- Scoring algorithms MUST be documented with clear mathematical definitions
- Weighting factors MUST be justified and configurable
- Evaluation criteria MUST be published before comparisons are run
- Changes to methodology MUST be versioned and announced

**Rationale**: Without transparency, comparison results cannot be trusted or
reproduced by third parties. Opaque scoring invites accusations of bias.

### III. Data Integrity

All comparisons MUST be based on verified, unmodified source documents with
complete provenance tracking.

- Source documents MUST be stored with cryptographic hashes for verification
- Modifications to test inputs MUST be logged and justified
- Agent outputs MUST be captured verbatim without post-processing
- All data transformations MUST be reversible and auditable

**Rationale**: Comparison validity depends entirely on the authenticity of inputs
and outputs. Corrupted or modified data invalidates all derived conclusions.

### IV. Fairness

All coding agents MUST be evaluated under identical conditions with equal
access to resources and information.

- Each agent MUST receive the same prompt/input for a given test case
- Time limits and resource constraints MUST be applied uniformly
- No agent-specific hints, context, or advantages are permitted
- Evaluation order MUST be randomized or otherwise controlled for bias

**Rationale**: Unfair comparisons are worse than no comparisons. Results from
biased evaluations mislead users and damage credibility.

### V. Reproducibility

All comparison runs MUST be fully reproducible given the same inputs,
configuration, and agent versions.

- Agent versions MUST be recorded with semantic version or commit hash
- Random seeds MUST be fixed and documented when randomness is used
- Environment configurations MUST be captured (OS, dependencies, API versions)
- Re-running a comparison with identical inputs MUST yield identical results

**Rationale**: Reproducibility is the foundation of scientific validity.
Non-reproducible results cannot be verified or built upon.

### VI. Measurable Outcomes

All comparisons MUST produce quantifiable, objective metrics that enable
meaningful statistical analysis.

- Metrics MUST have clear definitions and units
- Subjective assessments MUST be converted to numerical scores via rubrics
- Statistical significance MUST be reported for comparative claims
- Confidence intervals MUST accompany point estimates where applicable

**Rationale**: Vague qualitative assessments ("Agent A is better") are not
actionable. Quantified metrics enable data-driven decisions.

### VII. Automation

Comparison workflows MUST be automated and scriptable to ensure consistency
and enable continuous evaluation.

- All comparison steps MUST be executable via command-line interface
- Manual intervention MUST NOT be required for standard comparison runs
- Batch processing of multiple test cases MUST be supported
- Results MUST be exportable in machine-readable formats (JSON, CSV)

**Rationale**: Manual processes introduce variability and limit scale.
Automation ensures consistency and enables integration with CI/CD pipelines.

## Quality Standards

- Documentation MUST be treated as a first-class deliverable
- All public interfaces MUST include usage examples
- Error messages MUST be actionable and include remediation steps
- Test coverage requirements:
  - Unit tests for core comparison logic
  - Integration tests for agent adapters
  - Contract tests for external API interactions

## Development Workflow

### Branch & PR Workflow (NON-NEGOTIABLE)

Every feature or change MUST follow this workflow:

0. **Update submodules** before starting any work (NON-NEGOTIABLE)
   - Run `git submodule update --remote --merge` to fetch latest changes
   - Verify all submodules are on their expected branches (typically `main`)
   - This ensures comparisons are based on the latest agent implementations
   - Submodule updates MUST be committed if changes are pulled

1. **Create feature branch** before any code changes
   - Branch naming: `feature/<description>` or `fix/<description>`
   - Branch MUST be created from `main` (or designated development branch)
   - No direct commits to `main` are permitted

2. **Implement changes** following Constitution principles
   - Commit messages MUST follow conventional commit format
   - Commits SHOULD be atomic and logically grouped

3. **Update documentation** after implementation
   - MkDocs documentation MUST be updated for user-facing changes
   - README MUST reflect current project state

4. **Create Pull Request** for review
   - PR MUST include: summary, test plan, Constitution compliance checklist
   - All CI checks MUST pass before review

5. **Code Review** required before merge
   - At least one reviewer MUST approve
   - Reviewer MUST verify Constitution compliance
   - All review comments MUST be addressed

6. **Merge and cleanup**
   - Squash merge preferred for feature branches
   - Delete feature branch after merge

### General Requirements

- Breaking changes MUST be documented in CHANGELOG
- Deprecations MUST include migration guidance and sunset timeline

## Documentation Standards

### MkDocs Documentation (NON-NEGOTIABLE)

All project documentation MUST be managed via MkDocs with hierarchical organization
by feature/functionality.

**Structure Requirements**:

```text
docs/
├── index.md                    # Project overview and quick start
├── getting-started/
│   ├── installation.md         # Setup instructions
│   ├── configuration.md        # Configuration options
│   └── quickstart.md           # First steps tutorial
├── features/
│   ├── index.md                # Feature overview
│   ├── <feature-name>/         # One directory per major feature
│   │   ├── overview.md         # Feature description
│   │   ├── usage.md            # Usage examples
│   │   ├── api.md              # API reference (if applicable)
│   │   └── troubleshooting.md  # Common issues
│   └── ...
├── agents/                     # Documentation per coding agent
│   ├── index.md                # Agent comparison overview
│   ├── codex/                  # Codex-specific docs
│   ├── gemini/                 # Gemini-specific docs
│   └── claude/                 # Claude-specific docs
├── methodology/
│   ├── evaluation-criteria.md  # Scoring methodology
│   ├── metrics.md              # Metric definitions
│   └── benchmarks.md           # Benchmark descriptions
├── development/
│   ├── contributing.md         # Contribution guidelines
│   ├── architecture.md         # System architecture
│   └── changelog.md            # Version history
└── reference/
    ├── cli.md                  # CLI reference
    ├── api.md                  # API reference
    └── glossary.md             # Term definitions
```

**Content Requirements**:

- Every feature MUST have its own documentation directory
- Documentation MUST be updated in the same PR as code changes
- All public APIs MUST have reference documentation
- Examples MUST be tested and working
- Navigation MUST be configured in `mkdocs.yml`

**MkDocs Configuration**:

- Theme: Material for MkDocs (recommended)
- Search MUST be enabled
- Navigation MUST be hierarchical and intuitive
- Code blocks MUST have syntax highlighting

## Governance

This constitution supersedes all other development practices for this project.
All contributions, reviews, and architectural decisions MUST verify compliance
with the principles defined above.

**Amendment Process**:
1. Create feature branch for constitution amendment
2. Propose amendment via pull request with rationale
3. Amendment requires explicit approval from project maintainer(s)
4. Approved amendments take effect upon merge
5. All dependent templates and documentation MUST be updated within the same PR

**Versioning Policy**:
- MAJOR: Removal or incompatible redefinition of principles
- MINOR: Addition of new principles or sections, material expansion of guidance
- PATCH: Clarifications, wording improvements, typo fixes

**Compliance Review**:
- All PRs MUST include a constitution compliance statement
- Violations MUST be flagged and resolved before merge
- Exceptions require documented justification and maintainer approval

**Version**: 1.1.1 | **Ratified**: 2025-12-25 | **Last Amended**: 2025-12-25
