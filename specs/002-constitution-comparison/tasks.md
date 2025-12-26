# Tasks: Constitution Comparison Analysis

**Input**: Design documents from `/specs/002-constitution-comparison/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Tests are not required. Build validation via `mkdocs build --strict` serves as verification.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Documentation site**: `docs/` at repository root
- **Configuration**: `mkdocs.yml` at repository root
- **Specs**: `specs/002-constitution-comparison/`

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Verify prerequisites and create base structure

- [x] T001 Verify submodules are initialized with `git submodule update --init --recursive`
- [x] T002 Verify MkDocs configuration exists in mkdocs.yml
- [x] T003 [P] Read research.md to confirm comparison data is available

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Create documentation structure that all user stories depend on

**⚠️ CRITICAL**: No user story content can be created until this phase is complete

- [x] T004 Create main comparison page placeholder in docs/methodology/constitution-comparison.md
- [x] T005 [P] Create Codex constitution page placeholder in docs/agents/codex/constitution.md
- [x] T006 [P] Create Gemini constitution page placeholder in docs/agents/gemini/constitution.md
- [x] T007 [P] Create Claude constitution page placeholder in docs/agents/claude/constitution.md
- [x] T008 Update mkdocs.yml navigation to include new constitution pages

**Checkpoint**: Foundation ready - user story implementation can now begin

---

## Phase 3: User Story 1 - View Constitution Comparison Online (Priority: P1) 🎯 MVP

**Goal**: プロジェクト評価者が3つのエージェントの憲法の違いをドキュメントサイトで閲覧できる

**Independent Test**: ドキュメントサイトで比較ページにアクセスし、3つのエージェントの違いが一目で分かる表形式で表示されていることを確認

### Implementation for User Story 1

- [x] T009 [US1] Write comparison baseline section with commit references in docs/methodology/constitution-comparison.md
- [x] T010 [US1] Write core principles comparison table in docs/methodology/constitution-comparison.md
- [x] T011 [US1] Write overall evaluation summary table in docs/methodology/constitution-comparison.md
- [x] T012 [US1] Add links to agent-specific constitution pages in docs/methodology/constitution-comparison.md
- [x] T013 [US1] Verify local build with `mkdocs build --strict` (skipped - mkdocs not available, will verify via CI)

**Checkpoint**: At this point, User Story 1 should be fully functional - comparison visible online

---

## Phase 4: User Story 2 - Understand Software Development Completeness (Priority: P1)

**Goal**: 開発者が各エージェントの憲法がソフトウェア開発に必要な情報をどの程度カバーしているか確認できる

**Independent Test**: 比較ページで「ソフトウェア開発必須要素」セクションを確認し、各エージェントのカバレッジが明確に分かる

### Implementation for User Story 2

- [x] T014 [US2] Write technology stack comparison section in docs/methodology/constitution-comparison.md
- [x] T015 [US2] Write development workflow comparison section in docs/methodology/constitution-comparison.md
- [x] T016 [US2] Write quality standards comparison section in docs/methodology/constitution-comparison.md
- [x] T017 [US2] Write governance comparison section in docs/methodology/constitution-comparison.md
- [x] T018 [US2] Add software development completeness assessment table in docs/methodology/constitution-comparison.md
- [x] T019 [US2] Verify local build with `mkdocs build --strict` (skipped - mkdocs not available, will verify via CI)

**Checkpoint**: At this point, User Stories 1 AND 2 should both work - full comparison with SW dev coverage

---

## Phase 5: User Story 3 - Evaluate Relative Quality (Priority: P2)

**Goal**: プロジェクトマネージャーが3つのエージェントの憲法品質を相対評価し、最も完成度が高いエージェントを判断できる

**Independent Test**: 評価サマリーセクションで、各エージェントのスコアと順位が表示され、判断根拠が明示されている

### Implementation for User Story 3

- [x] T020 [P] [US3] Write Codex strengths and weaknesses in docs/agents/codex/constitution.md
- [x] T021 [P] [US3] Write Gemini strengths and weaknesses in docs/agents/gemini/constitution.md
- [x] T022 [P] [US3] Write Claude strengths and weaknesses in docs/agents/claude/constitution.md
- [x] T023 [US3] Write scoring methodology explanation in docs/methodology/constitution-comparison.md
- [x] T024 [US3] Write detailed evaluation with evidence quotes in docs/methodology/constitution-comparison.md
- [x] T025 [US3] Add relative ranking with justification in docs/methodology/constitution-comparison.md
- [x] T026 [US3] Verify local build with `mkdocs build --strict` (skipped - mkdocs not available, will verify via CI)

**Checkpoint**: All user stories should now be independently functional - full comparison with relative evaluation

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Final improvements and validation

- [x] T027 [P] Cross-link constitution pages with main comparison page
- [x] T028 [P] Add Japanese/English toggle notes where applicable
- [x] T029 Run final build validation with `mkdocs build --strict` (skipped - mkdocs not available, will verify via CI)
- [x] T030 Update specs/002-constitution-comparison/tasks.md to mark all tasks complete
- [x] T031 Commit all changes and push to feature branch

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
  - US1 can complete independently
  - US2 extends US1's comparison page but is independently testable
  - US3 depends on both US1 and US2 data being present
- **Polish (Final Phase)**: Depends on all user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - MVP comparison display
- **User Story 2 (P1)**: Can start after Foundational (Phase 2) - Extends US1 comparison sections
- **User Story 3 (P2)**: Depends on US1 and US2 comparison data - Adds evaluation layer

### Within Each User Story

- Create/update files before building
- Verify with `mkdocs build --strict` at each checkpoint
- Agent-specific pages can be created in parallel

### Parallel Opportunities

- T005, T006, T007 (agent page placeholders) can run in parallel
- T020, T021, T022 (agent strengths/weaknesses) can run in parallel
- T027, T028 (polish tasks) can run in parallel

---

## Parallel Example: User Story 3

```bash
# Launch all agent constitution pages together:
Task: "Write Codex strengths and weaknesses in docs/agents/codex/constitution.md"
Task: "Write Gemini strengths and weaknesses in docs/agents/gemini/constitution.md"
Task: "Write Claude strengths and weaknesses in docs/agents/claude/constitution.md"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup (T001-T003)
2. Complete Phase 2: Foundational (T004-T008)
3. Complete Phase 3: User Story 1 (T009-T013)
4. **STOP and VALIDATE**: Comparison page is visible and has comparison baseline
5. Deploy/demo MVP

### Incremental Delivery

1. Complete Setup + Foundational → Directory structure ready
2. Add User Story 1 → Test independently → Basic comparison visible (MVP!)
3. Add User Story 2 → Test independently → Software development coverage visible
4. Add User Story 3 → Test independently → Relative evaluation with rankings
5. Each story adds value without breaking previous stories

### Single Developer Strategy

1. Work through phases sequentially
2. Complete all [P] tasks in parallel within each phase
3. Commit after each checkpoint
4. Validate with `mkdocs build --strict` frequently

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- Build validation: `mkdocs build --strict`
- Local preview: `mkdocs serve`
