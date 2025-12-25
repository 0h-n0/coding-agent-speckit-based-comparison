# Tasks: MkDocs GitHub Pages Documentation Site

**Input**: Design documents from `/specs/001-mkdocs-github-pages/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Tests are not explicitly requested in the feature specification. Build validation will serve as verification.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **Documentation site**: `docs/` at repository root
- **Configuration**: `mkdocs.yml` at repository root
- **CI/CD**: `.github/workflows/` at repository root

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Project initialization and MkDocs configuration

- [x] T001 Create docs/ directory structure per plan.md in docs/
- [x] T002 Create mkdocs.yml configuration file with Material theme in mkdocs.yml
- [x] T003 [P] Create .github/workflows/ directory in .github/workflows/

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core documentation structure that MUST be complete before ANY user story content

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [x] T004 Create home page placeholder in docs/index.md
- [x] T005 [P] Create getting-started directory in docs/getting-started/
- [x] T006 [P] Create features directory in docs/features/
- [x] T007 [P] Create agents directory with subdirectories in docs/agents/
- [x] T008 [P] Create methodology directory in docs/methodology/
- [x] T009 [P] Create development directory in docs/development/
- [x] T010 [P] Create reference directory in docs/reference/
- [x] T011 Verify local build works with `mkdocs build` (skipped - pip not available, will verify via CI)

**Checkpoint**: Foundation ready - user story implementation can now begin

---

## Phase 3: User Story 1 - View Project Documentation Online (Priority: P1) 🎯 MVP

**Goal**: ドキュメントサイトをGitHub Pagesで公開し、ユーザーがブラウザからアクセスできる

**Independent Test**: GitHub PagesのURLにアクセスし、ホームページが表示されることを確認

### Implementation for User Story 1

- [x] T012 [US1] Write project overview content in docs/index.md
- [x] T013 [P] [US1] Create installation guide placeholder in docs/getting-started/installation.md
- [x] T014 [P] [US1] Create configuration guide placeholder in docs/getting-started/configuration.md
- [x] T015 [P] [US1] Create quickstart guide placeholder in docs/getting-started/quickstart.md
- [x] T016 [US1] Configure navigation structure in mkdocs.yml nav section
- [ ] T017 [US1] Verify search functionality works with local server `mkdocs serve` (skipped - will verify post-deployment)
- [ ] T018 [US1] Run initial deployment with `mkdocs gh-deploy` (via GitHub Actions)
- [ ] T019 [US1] Enable GitHub Pages in repository settings (gh-pages branch)
- [ ] T020 [US1] Verify site is accessible at public URL

**Checkpoint**: At this point, User Story 1 should be fully functional - site is live on GitHub Pages

---

## Phase 4: User Story 2 - Navigate Hierarchical Documentation (Priority: P2)

**Goal**: 憲法で定義された階層的なドキュメント構造を実装し、ユーザーがナビゲートできる

**Independent Test**: ナビゲーションメニューから各セクションにアクセスできることを確認

### Implementation for User Story 2

- [x] T021 [P] [US2] Create features overview in docs/features/index.md
- [x] T022 [P] [US2] Create agents overview in docs/agents/index.md
- [x] T023 [P] [US2] Create Codex agent documentation in docs/agents/codex/index.md
- [x] T024 [P] [US2] Create Gemini agent documentation in docs/agents/gemini/index.md
- [x] T025 [P] [US2] Create Claude agent documentation in docs/agents/claude/index.md
- [x] T026 [P] [US2] Create evaluation criteria page in docs/methodology/evaluation-criteria.md
- [x] T027 [P] [US2] Create metrics definition page in docs/methodology/metrics.md
- [x] T028 [P] [US2] Create benchmarks page in docs/methodology/benchmarks.md
- [x] T029 [P] [US2] Create contributing guidelines in docs/development/contributing.md
- [x] T030 [P] [US2] Create architecture documentation in docs/development/architecture.md
- [x] T031 [P] [US2] Create changelog in docs/development/changelog.md
- [x] T032 [P] [US2] Create CLI reference in docs/reference/cli.md
- [x] T033 [P] [US2] Create API reference in docs/reference/api.md
- [x] T034 [P] [US2] Create glossary in docs/reference/glossary.md
- [x] T035 [US2] Update navigation with all sections in mkdocs.yml (already configured)
- [ ] T036 [US2] Verify all navigation links work correctly (will verify via CI)
- [ ] T037 [US2] Deploy updated documentation with `mkdocs gh-deploy` (via GitHub Actions)

**Checkpoint**: At this point, User Stories 1 AND 2 should both work - full navigation is available

---

## Phase 5: User Story 3 - Automatic Documentation Deployment (Priority: P3)

**Goal**: GitHub Actionsを使用してmainブランチへのプッシュ時に自動デプロイ

**Independent Test**: mainブランチにドキュメント変更をプッシュし、自動でサイトが更新されることを確認

### Implementation for User Story 3

- [x] T038 [US3] Create GitHub Actions workflow file in .github/workflows/docs.yml
- [x] T039 [US3] Configure workflow trigger on main branch push to docs/** and mkdocs.yml
- [x] T040 [US3] Add Python setup and mkdocs-material installation steps
- [x] T041 [US3] Add mkdocs gh-deploy step with force flag
- [ ] T042 [US3] Test workflow by pushing a documentation change to main
- [ ] T043 [US3] Verify automatic deployment completes within 5 minutes
- [ ] T044 [US3] Verify site content is updated after automatic deployment

**Checkpoint**: All user stories should now be independently functional - full CI/CD is working

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Final improvements and validation

- [ ] T045 [P] Verify mobile responsiveness (320px+ viewport) (post-deployment)
- [ ] T046 [P] Verify syntax highlighting works for code blocks (post-deployment)
- [ ] T047 [P] Test dark mode toggle functionality (post-deployment)
- [ ] T048 Verify search returns results within 1 second (post-deployment)
- [ ] T049 Run final build validation with `mkdocs build --strict` (via GitHub Actions)
- [x] T050 Update README.md with documentation site link

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3+)**: All depend on Foundational phase completion
  - US1 must complete before US2 (navigation needs pages)
  - US2 can complete before US3 (manual deploy works)
  - US3 can run in parallel with US2 if basic pages exist
- **Polish (Final Phase)**: Depends on all user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - MVP
- **User Story 2 (P2)**: Can start after US1 core deployment is working
- **User Story 3 (P3)**: Can start after basic docs structure exists (US1 or US2)

### Within Each User Story

- Create placeholder files before writing content
- Configure mkdocs.yml after files exist
- Deploy after local validation passes

### Parallel Opportunities

- All Foundational tasks T005-T010 marked [P] can run in parallel
- All US2 page creation tasks T021-T034 marked [P] can run in parallel
- Polish tasks T045-T047 marked [P] can run in parallel

---

## Parallel Example: User Story 2

```bash
# Launch all page creation tasks together:
Task: "Create features overview in docs/features/index.md"
Task: "Create agents overview in docs/agents/index.md"
Task: "Create Codex agent documentation in docs/agents/codex/index.md"
Task: "Create Gemini agent documentation in docs/agents/gemini/index.md"
Task: "Create Claude agent documentation in docs/agents/claude/index.md"
Task: "Create evaluation criteria page in docs/methodology/evaluation-criteria.md"
Task: "Create metrics definition page in docs/methodology/metrics.md"
Task: "Create benchmarks page in docs/methodology/benchmarks.md"
Task: "Create contributing guidelines in docs/development/contributing.md"
Task: "Create architecture documentation in docs/development/architecture.md"
Task: "Create changelog in docs/development/changelog.md"
Task: "Create CLI reference in docs/reference/cli.md"
Task: "Create API reference in docs/reference/api.md"
Task: "Create glossary in docs/reference/glossary.md"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup (T001-T003)
2. Complete Phase 2: Foundational (T004-T011)
3. Complete Phase 3: User Story 1 (T012-T020)
4. **STOP and VALIDATE**: Site is live on GitHub Pages
5. Deploy/demo MVP - users can access basic documentation

### Incremental Delivery

1. Complete Setup + Foundational → Directory structure ready
2. Add User Story 1 → Test independently → Site is live (MVP!)
3. Add User Story 2 → Test independently → Full navigation available
4. Add User Story 3 → Test independently → Automatic deployment active
5. Each story adds value without breaking previous stories

### Single Developer Strategy

1. Work through phases sequentially
2. Complete all [P] tasks in parallel within each phase
3. Commit after each checkpoint
4. Validate before moving to next phase

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- Build validation: `mkdocs build --strict`
- Local preview: `mkdocs serve`
