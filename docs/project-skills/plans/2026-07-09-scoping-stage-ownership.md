# Scoping Stage Ownership Implementation Plan

**Goal:** Make `/scoping` an explicit stage-exclusive workflow so it cannot be displaced by generic brainstorming, planning, or implementation workflows.

**Architecture:** Put the stage-ownership rule in the plugin’s shared workflow so every stage has a common precedence contract. Repeat the scoping-specific boundary in `skills/scoping/SKILL.md`, where it is visible at invocation time. Add evaluation fixtures that assert the stop boundary: `/scoping` may gather context and ask questions, but it must not create a spec, task breakdown, implementation plan, or use a generic workflow’s artifact path.

**Tech stack:** Markdown skill definitions, JSON evaluation fixtures, OMP plugin discovery.

## Global Constraints

- Do not add any externally owned artifact path, skill reference, execution handoff, or generic-workflow dependency.
- Preserve the existing project-skills stage sequence: `setup-project-skills` → `scoping` → `project-spec` → `create-tasks` → `implement` → `project-review`.
- `/scoping` may invoke only domain-specific skills necessary to resolve the user’s subject matter. It must not hand process ownership to a generic workflow.
- `/scoping` must continue to ask one question at a time and end with its existing `Scoping state` output.
- Do not change the default `docs/project-skills/...` artifact schema in this fix.

---

### Task 1: Define shared workflow-stage ownership

**Files:**
- Modify: `skills/shared/WORKFLOW.md`
- Modify: `README.md`

**Interfaces:**
- Consumes: the existing six-stage workflow documented in `README.md` and the shared delegation list in `skills/shared/WORKFLOW.md`.
- Produces: a plugin-wide rule that stage skills own process control, artifact types, and stage transitions; domain skills supply subject-matter guidance only.

- [ ] **Step 1: Add a `Workflow stage ownership` section to `skills/shared/WORKFLOW.md` before `## Project classes`.**

  Add this contract verbatim:

  ```markdown
  ## Workflow stage ownership

  A project-skills command owns its active workflow stage. It determines the
  questions to ask, the artifacts it may create, and the condition for moving
  to the next stage.

  Do not invoke or follow a generic brainstorming, planning, specification,
  task-breakdown, or implementation workflow while a project-skills stage is
  active. Domain-specific skills may be used only for their subject-matter
  guidance; they must not replace the active stage, select an artifact path,
  or advance the workflow.

  A stage may create only the artifacts named by that stage. In particular,
  `scoping` does not create a project spec, task file, implementation plan, or
  review. Those artifacts belong respectively to `project-spec`,
  `create-tasks`, `implement`, and `project-review` after their required
  approval gates.
  ```

- [ ] **Step 2: Add a stage-boundary sentence after the numbered workflow in `README.md`.**

  Add:

  ```markdown
  Each command owns only its named stage. While a command is active, generic
  workflows must not choose artifacts or advance the project; use the next
  project-skills command only after the current command reaches its documented
  stop condition.
  ```

- [ ] **Step 3: Review the edited text against the existing sequence.**

  Confirm that the new language does not prevent the documented domain-skill delegation in `skills/shared/WORKFLOW.md` or change any of the six workflow stages.

- [ ] **Step 4: Commit the stage-ownership contract.**

  ```sh
  git add README.md skills/shared/WORKFLOW.md
  git commit -m "docs: define project workflow stage ownership"
  ```

### Task 2: Make `/scoping` explicitly exclusive

**Files:**
- Modify: `skills/scoping/SKILL.md`

**Interfaces:**
- Consumes: `skills/shared/WORKFLOW.md` and its new stage-ownership contract.
- Produces: an invocation-local instruction that defines permitted delegation and forbids generic process substitution.

- [ ] **Step 1: Add a `Stage ownership` section in `skills/scoping/SKILL.md` after `## Required background`.**

  Add this text after the existing PyMC/model-planning prerequisites and before `## Process`:

  ```markdown
  ## Stage ownership

  When invoked as `/scoping`, this skill exclusively owns the scoping stage.
  Do not invoke or follow generic brainstorming, planning, specification,
  task-breakdown, or implementation workflows.

  Use only the domain-specific skills required to answer the user’s actual
  question, including the skills named in Required background. Those skills
  provide subject-matter guidance only; they do not choose artifact paths or
  replace this scoping process.

  During `/scoping`, do not create a project spec, task file, implementation
  plan, or review. Continue to the next project-skills stage only when this
  skill has reached its stop condition and the user explicitly invokes or
  approves that stage.
  ```

- [ ] **Step 2: Tighten Process step 7 to reference the ownership rule.**

  Replace the existing step-7 text with:

  ```markdown
  7. Stop after scoping.
     - Follow the Stage ownership boundary.
     - Do not write the project spec unless the user invokes or approves
       `project-spec`.
     - Do not create a task breakdown or implementation plan.
  ```

- [ ] **Step 3: Verify the required domain delegation remains intact.**

  Confirm that `model-plan-discovery` and `pymc-modeling` remain the only process-adjacent dependencies named by `/scoping`; they are subject-matter prerequisites, not replacement workflows.

- [ ] **Step 4: Commit the invocation-local guard.**

  ```sh
  git add skills/scoping/SKILL.md
  git commit -m "fix: keep generic workflows out of scoping"
  ```

### Task 3: Add scoping-boundary evaluation fixtures

**Files:**
- Modify: `evals/evals.json`

**Interfaces:**
- Consumes: the existing JSON object with sequential numeric `id` values and `prompt`, `expected_output`, and `files` fields.
- Produces: two deterministic acceptance fixtures for stage ownership and the scoping stop boundary.

- [ ] **Step 1: Append evaluation `id: 7` to the `evals` array.**

  Add this object after the existing `id: 6` object, preserving valid JSON commas:

  ```json
  {
    "id": 7,
    "prompt": "Use /scoping for a four-hour PyMC and marimo teaching-material project. The repository has a course proposal and older example notebooks. Explore the materials and scope the work, but do not write a specification or implementation plan yet.",
    "expected_output": "The agent should infer research notebook as the project class, ask the user to confirm or correct that classification, explore the proposal and named source materials, ask one scoping decision at a time, and finish with the Scoping state block. It must not invoke a generic workflow, create a specification, task file, or implementation plan, or use an external workflow-owned artifact path.",
    "files": []
  }
  ```

- [ ] **Step 2: Append evaluation `id: 8` to the `evals` array.**

  Add this object immediately after evaluation 7:

  ```json
  {
    "id": 8,
    "prompt": "Use /scoping for a Bayesian analysis project. The user asks to move directly into implementation after one answer, but has not invoked or approved project-spec.",
    "expected_output": "The agent should continue or conclude the scoping stage according to the remaining fog, state whether the project is ready for project-spec, and stop. It must not create a project spec, tasks, or implementation plan, and it must not route to a generic planning or implementation workflow.",
    "files": []
  }
  ```

- [ ] **Step 3: Validate the edited fixture file is syntactically valid JSON.**

  Run:

  ```sh
  bun --eval 'JSON.parse(await Bun.file("evals/evals.json").text()); console.log("evals JSON valid")'
  ```

  Expected output:

  ```text
  evals JSON valid
  ```

- [ ] **Step 4: Manually evaluate fixture 7 against `skills/scoping/SKILL.md`.**

  Verify every expected behavior maps to an explicit instruction:

  - project-class confirmation: Process step 3;
  - one question at a time: Question discipline;
  - no spec/tasks/plan: Stage ownership and Process step 7;
  - terminal state: Completion output.

- [ ] **Step 5: Commit the regression fixtures.**

  ```sh
  git add evals/evals.json
  git commit -m "test: cover scoping stage boundaries"
  ```

### Task 4: Verify the plugin’s user-facing workflow

**Files:**
- Modify: none

**Interfaces:**
- Consumes: the revised shared workflow, scoping skill, README, and evaluation fixtures.
- Produces: evidence that the documented command lifecycle, the invocation-local guard, and the regression fixtures agree.

- [ ] **Step 1: Read the final relevant sections together.**

  Inspect:

  ```text
  README.md
  skills/shared/WORKFLOW.md
  skills/scoping/SKILL.md
  evals/evals.json
  ```

  Confirm that only `project-spec` selects a spec path and that neither the shared workflow nor `/scoping` refers to an external generic workflow.

- [ ] **Step 2: Run the JSON validation again from the repository root.**

  Run:

  ```sh
  bun --eval 'JSON.parse(await Bun.file("evals/evals.json").text()); console.log("evals JSON valid")'
  ```

  Expected output:

  ```text
  evals JSON valid
  ```

- [ ] **Step 3: Record acceptance evidence in the implementation handoff.**

  The completed implementation must demonstrate all of the following:

  - `/scoping` names its project class, asks one question at a time, and emits `Scoping state`.
  - `/scoping` permits required domain skills without permitting them to own workflow progression.
  - `/scoping` cannot select generic artifact paths or create specs, tasks, plans, or reviews.
  - `project-spec` remains the only stage that chooses `docs/project-skills/specs/...` by default.
  - The evaluation fixture file parses as JSON and contains both new boundary cases.

- [ ] **Step 4: Commit only if verification required a documentation correction.**

  If no correction was necessary, do not create an empty commit. Otherwise:

  ```sh
  git add README.md skills/shared/WORKFLOW.md skills/scoping/SKILL.md evals/evals.json
  git commit -m "docs: clarify scoping workflow boundaries"
  ```
