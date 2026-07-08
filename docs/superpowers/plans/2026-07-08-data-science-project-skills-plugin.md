# Data Science Project Skills Plugin Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a usable oh-my-pi-native plugin that guides data science projects from vague idea through scoping, spec, tasks, implementation, and review.

**Architecture:** Create one plugin with six stage-specific skills plus shared workflow references and minimal eval prompts. The skills are OMP-native, delegate to existing PyMC/marimo/modeling skills instead of duplicating them, and default to local markdown artifacts with a configurable backend.

**Tech Stack:** Markdown skill files, `.claude-plugin/plugin.json` manifest, JSON eval prompts, local markdown artifact conventions, oh-my-pi tools and skill-invocation patterns.

## Global Constraints

- Source design: `docs/superpowers/specs/2026-07-08-data-science-project-skills-design.md`.
- Plugin shape: one installable plugin, not loose unrelated skills.
- Skill names: `setup-project-skills`, `scoping`, `project-spec`, `create-tasks`, `implement`, `project-review`.
- Naming rule: no `ds-` prefix.
- Workflow target: general data science with Bayesian hooks.
- Artifact policy: local markdown by default, configurable backend later.
- Project classes: research notebook, analysis/modeling, end-to-end ML product.
- Project class selection: infer, then confirm with the user.
- Review policy: adaptive axes; always code quality and reproducibility, add scientific validity/data governance/Bayesian/deployment when triggered.
- Implementation policy: delegate to existing oh-my-pi skills for PyMC, ArviZ, marimo, prior elicitation, model evaluation, and PyMC testing.
- Documentation policy: maintain domain and data docs, not only a project spec.
- Eval policy: minimal eval prompts in v1, no full benchmark loop unless requested later.
- Attribution policy: explicitly attribute Matt Pocock's MIT-licensed source skills in README and skill notes.

---

## File Structure

Create this structure:

```text
.claude-plugin/plugin.json
README.md
skills/project-development/shared/WORKFLOW.md
skills/project-development/setup-project-skills/SKILL.md
skills/project-development/scoping/SKILL.md
skills/project-development/project-spec/SKILL.md
skills/project-development/create-tasks/SKILL.md
skills/project-development/implement/SKILL.md
skills/project-development/project-review/SKILL.md
evals/evals.json
```

Responsibilities:

- `.claude-plugin/plugin.json`: plugin manifest listing all six skills.
- `README.md`: user-facing plugin overview, workflow, install notes, attribution.
- `skills/project-development/shared/WORKFLOW.md`: shared artifact model, project classes, review axes, and delegation rules referenced by all skills.
- Each `SKILL.md`: one stage-specific skill with complete trigger metadata and OMP-native instructions.
- `evals/evals.json`: minimal test prompts covering all stages.

---

### Task 1: Plugin shell and shared workflow contract

**Files:**
- Create: `.claude-plugin/plugin.json`
- Create: `README.md`
- Create: `skills/project-development/shared/WORKFLOW.md`

**Interfaces:**
- Consumes: approved design spec at `docs/superpowers/specs/2026-07-08-data-science-project-skills-design.md`.
- Produces: plugin manifest and shared workflow reference consumed by all later skill files.

- [ ] **Step 1: Create plugin manifest**

Write `.claude-plugin/plugin.json` exactly as:

```json
{
  "name": "project-skills",
  "skills": [
    "./skills/project-development/setup-project-skills",
    "./skills/project-development/scoping",
    "./skills/project-development/project-spec",
    "./skills/project-development/create-tasks",
    "./skills/project-development/implement",
    "./skills/project-development/project-review"
  ]
}
```

- [ ] **Step 2: Create README**

Write `README.md` exactly as:

```markdown
# Project Skills

An oh-my-pi-native plugin for developing data science projects from vague idea to verified implementation.

## Workflow

1. `setup-project-skills` configures repo-local defaults.
2. `scoping` turns a vague idea into a scoped project direction.
3. `project-spec` writes the formal project spec.
4. `create-tasks` breaks the spec into blocked, agent-sized tasks.
5. `implement` executes one approved task at a time.
6. `project-review` reviews completed work across code quality, reproducibility, and adaptive data-science axes.

## Project classes

The workflow supports three project classes:

- Research notebook projects.
- Analysis/modeling projects.
- End-to-end ML products.

`scoping` infers the class from the user's goal, artifacts, data risks, and implementation needs, then asks the user to confirm or correct it.

## Defaults

The plugin defaults to local markdown artifacts:

```text
.project-skills/config.toml
docs/project-skills/domain.md
docs/project-skills/data.md
docs/project-skills/specs/
docs/project-skills/tasks/
docs/project-skills/reviews/
evals/
```

`setup-project-skills` can override these paths for a repo.

## oh-my-pi integration

The skills are written for oh-my-pi sessions. They refer to OMP-native tools and coordination patterns such as `read`, `grep`, `glob`, `todo`, `task`, `job`, `irc`, `lsp`, `edit`, `write`, local artifacts, and existing domain skills.

The workflow delegates specialized knowledge instead of duplicating it. Relevant domain skills include:

- `marimo-notebook`
- `marimo-pair`
- `model-plan-discovery`
- `pymc-modeling`
- `prior-elicitation`
- `model-evaluation`
- `pymc-testing`
- `requesting-code-review`
- `verification-before-completion`

## Attribution

This plugin is derived from Matt Pocock's `mattpocock/skills` repository under the MIT License. It adapts these source skills for data science project development and oh-my-pi:

- `setup-matt-pocock-skills`
- `grill-with-docs`
- `wayfinder`
- `to-spec`
- `to-tickets`
- `implement`
- `code-review`
```

- [ ] **Step 3: Create shared workflow reference**

Write `skills/project-development/shared/WORKFLOW.md` exactly as:

```markdown
# Project Skills Shared Workflow

This reference is shared by the project-development skills.

## Source lineage

This plugin is derived from Matt Pocock's `mattpocock/skills` repository under the MIT License. The adapted source skills are `setup-matt-pocock-skills`, `grill-with-docs`, `wayfinder`, `to-spec`, `to-tickets`, `implement`, and `code-review`.

## Project classes

Every project is classified as one of:

1. **Research notebook** — the main deliverable is an exploratory or explanatory notebook.
2. **Analysis/modeling** — the main deliverable includes reusable analysis/model code, metrics, diagnostics, or reports.
3. **End-to-end ML product** — the main deliverable includes production-oriented behavior such as batch scoring, services, monitoring, deployment handoff, or operational review.

The agent should infer the class from the user's goal and repo context, state the inferred class, and ask the user to confirm or correct it.

## Default artifacts

Local markdown is the default backend:

```text
.project-skills/config.toml
docs/project-skills/domain.md
docs/project-skills/data.md
docs/project-skills/specs/
docs/project-skills/tasks/
docs/project-skills/reviews/
evals/
```

If `.project-skills/config.toml` exists, its paths override these defaults.

## Config schema

Use this schema for `.project-skills/config.toml`:

```toml
[artifacts]
backend = "local-markdown"
domain_doc = "docs/project-skills/domain.md"
data_doc = "docs/project-skills/data.md"
specs_dir = "docs/project-skills/specs"
tasks_dir = "docs/project-skills/tasks"
reviews_dir = "docs/project-skills/reviews"

[project]
default_class = "infer"
notebook_policy = "marimo-for-exploration-python-modules-for-reuse"

[commands]
test = ""
lint = ""
check = ""

[skills]
marimo_notebook = true
marimo_pair = true
model_plan_discovery = true
pymc_modeling = true
prior_elicitation = true
model_evaluation = true
pymc_testing = true
requesting_code_review = true
verification_before_completion = true
```

Empty command strings mean the agent must inspect the repo before choosing commands.

## Domain and data docs

Maintain two durable docs when the workflow needs persistent context:

- `domain.md`: glossary, project-specific terms, domain assumptions, hard-to-explain decisions.
- `data.md`: sources, access paths, schema notes, quality issues, target definitions, leakage risks, privacy/licensing constraints, reproducibility requirements.

## Task slicing

Prefer vertical tracer bullets. A task should produce one independently verifiable behavior or decision.

Permit prerequisite stage tasks only when a vertical slice would be fake. Allowed prerequisite types are:

- data access,
- schema profiling,
- baseline notebook creation,
- reproducibility setup,
- infrastructure needed before a meaningful project slice can run.

Every task declares blockers and acceptance criteria.

## Review axes

Always review:

1. Code quality.
2. Reproducibility.

Add adaptive axes when triggered:

- Scientific validity for analysis/modeling projects.
- Bayesian diagnostics for PyMC, PyTensor, ArviZ, MCMC, priors, posterior predictive checks, or model comparison.
- Data governance for sensitive, external, regulated, licensed, or production-bound data.
- Deployment readiness for end-to-end ML products.

Keep axes separate in review output.

## Delegation rules

Delegate specialized work to existing oh-my-pi skills when they apply:

- Use `marimo-notebook` for marimo notebooks.
- Use `marimo-pair` for active paired notebook sessions.
- Use `model-plan-discovery` before statistical, Bayesian, ML, or data-science modeling plans.
- Use `pymc-modeling` for PyMC, PyTensor, ArviZ, Bayesian modeling, MCMC, priors, diagnostics, and probabilistic programming.
- Use `prior-elicitation` for prior selection and prior predictive checks.
- Use `model-evaluation` for LOO, ELPD, stacking, and Bayesian model comparison.
- Use `pymc-testing` for tests touching PyMC models or sampling.
- Use `requesting-code-review` and `verification-before-completion` before claiming implementation complete.
```

- [ ] **Step 4: Verify shell files**

Run: `git diff --check`

Expected: no output and exit code 0.

Run: `git status --short`

Expected: the three new files from this task appear as untracked or modified, with no unrelated files changed.

- [ ] **Step 5: Commit task 1**

Run:

```bash
git add .claude-plugin/plugin.json README.md skills/project-development/shared/WORKFLOW.md
git commit -m "Add project skills plugin shell"
```

Expected: commit succeeds and includes exactly the three files from this task.

---

### Task 2: Setup skill

**Files:**
- Create: `skills/project-development/setup-project-skills/SKILL.md`

**Interfaces:**
- Consumes: shared workflow reference at `skills/project-development/shared/WORKFLOW.md`.
- Produces: `setup-project-skills`, which records repo-local defaults consumed by later workflow skills.

- [ ] **Step 1: Write setup skill**

Write `skills/project-development/setup-project-skills/SKILL.md` exactly as:

```markdown
---
name: setup-project-skills
description: Use when configuring a repository to use the project-skills data science workflow, especially before scoping, specs, task creation, implementation, or project review in a new repo.
disable-model-invocation: true
---

# Setup Project Skills

Configure repo-local defaults for the project-skills workflow.

Source lineage: derived from Matt Pocock's `setup-matt-pocock-skills`, adapted for oh-my-pi and data science project development.

## Required background

Read `../shared/WORKFLOW.md` before acting.

## Process

1. Inspect the repo before asking questions.
   - List the root directory.
   - Check for existing project configuration such as `pixi.toml`, `pyproject.toml`, `.project-skills/config.toml`, notebook directories, data directories, and docs directories.
   - Do not search for agent instruction files; the relevant instructions are already in session context.
2. Infer defaults.
   - Artifact backend: `local-markdown` unless a tracker integration is already obvious.
   - Notebook policy: marimo for exploration/reporting and Python modules for reusable logic.
   - Project class: `infer` unless the repo is clearly dedicated to one class.
   - Commands: prefer existing pixi tasks when available; otherwise leave command strings empty.
3. Ask only for decisions that repo context cannot answer.
   - Ask one question at a time.
   - Provide a recommended answer.
4. Write `.project-skills/config.toml` using the shared schema.
5. Create docs directories if they do not exist.
6. Report the config path and the decisions recorded.

## Default config

Use this config when the repo has no stronger convention:

```toml
[artifacts]
backend = "local-markdown"
domain_doc = "docs/project-skills/domain.md"
data_doc = "docs/project-skills/data.md"
specs_dir = "docs/project-skills/specs"
tasks_dir = "docs/project-skills/tasks"
reviews_dir = "docs/project-skills/reviews"

[project]
default_class = "infer"
notebook_policy = "marimo-for-exploration-python-modules-for-reuse"

[commands]
test = ""
lint = ""
check = ""

[skills]
marimo_notebook = true
marimo_pair = true
model_plan_discovery = true
pymc_modeling = true
prior_elicitation = true
model_evaluation = true
pymc_testing = true
requesting_code_review = true
verification_before_completion = true
```

## Output

End with:

```markdown
Configured project-skills workflow.

Config: `.project-skills/config.toml`

Recorded decisions:
- Artifact backend: ...
- Domain doc: ...
- Data doc: ...
- Specs dir: ...
- Tasks dir: ...
- Reviews dir: ...
- Project class default: ...
- Notebook policy: ...
- Commands recorded: ...
```

Replace the dots with the concrete values written to config.
```

- [ ] **Step 2: Verify frontmatter and relative reference**

Run: `git diff --check`

Expected: no output and exit code 0.

Open `skills/project-development/setup-project-skills/SKILL.md` and verify:

- frontmatter contains `name`, `description`, and `disable-model-invocation`,
- description starts with `Use when`,
- the skill tells the agent to read `../shared/WORKFLOW.md`,
- the default config matches the shared schema.

- [ ] **Step 3: Commit task 2**

Run:

```bash
git add skills/project-development/setup-project-skills/SKILL.md
git commit -m "Add setup project skills workflow"
```

Expected: commit succeeds and includes exactly the setup skill.

---

### Task 3: Scoping and project spec skills

**Files:**
- Create: `skills/project-development/scoping/SKILL.md`
- Create: `skills/project-development/project-spec/SKILL.md`

**Interfaces:**
- Consumes: shared workflow reference and optional `.project-skills/config.toml`.
- Produces: two user-invoked skills that move a project from idea to approved spec.

- [ ] **Step 1: Write scoping skill**

Write `skills/project-development/scoping/SKILL.md` exactly as:

```markdown
---
name: scoping
description: Use when a data science project idea is vague, risky, multi-stage, or needs clarification before a spec, task breakdown, implementation, or review.
disable-model-invocation: true
---

# Scoping

Turn a vague data science idea into a scoped direction, with domain language and data assumptions captured as durable context.

Source lineage: derived from Matt Pocock's `grill-with-docs` and `wayfinder`, adapted for oh-my-pi and data science project development.

## Required background

Read `../shared/WORKFLOW.md` before acting.

If the prompt involves statistical, Bayesian, ML, or data-science modeling plans, also use `model-plan-discovery` before settling modeling details.

If the prompt involves PyMC, PyTensor, ArviZ, Bayesian modeling, priors, MCMC, diagnostics, or model comparison, also use `pymc-modeling` before responding further.

## Process

1. Explore repo context first.
   - Read the root directory.
   - Read `.project-skills/config.toml` if present.
   - Read existing domain/data docs if present.
   - Use search or code intelligence for facts that the repo can answer.
2. Infer the project class.
   - Research notebook.
   - Analysis/modeling.
   - End-to-end ML product.
3. Ask the user to confirm or correct the inferred class.
   - Ask one question at a time.
   - Provide the recommended answer.
4. Grill one decision at a time.
   - Problem and decision target.
   - Stakeholders and consumers.
   - Data sources and access.
   - Target or outcome definition.
   - Data leakage risks.
   - Privacy, licensing, and governance risks.
   - Modeling or analysis approach.
   - Reproducibility requirements.
   - Expected deliverables.
   - Out-of-scope work.
5. Update durable docs when facts become stable.
   - Domain terms and decisions go to the configured domain doc.
   - Data sources, schemas, risks, and reproducibility facts go to the configured data doc.
6. Identify fog.
   - If the path to `project-spec` is clear, say so.
   - If not, list investigation tasks that would clear the fog.
7. Stop after scoping.
   - Do not write the project spec unless the user invokes or approves `project-spec`.

## Question discipline

Ask one question per message. Do not bundle unrelated decisions.

If repo exploration can answer the question, explore instead of asking.

Every question includes a recommended answer and why that answer is safest.

## Completion output

End with:

```markdown
Scoping state:
- Project class: research notebook | analysis/modeling | end-to-end ML product
- Ready for project-spec: yes | no
- Domain doc updated: path or not updated
- Data doc updated: path or not updated
- Remaining fog:
  - item, or None
```
```

- [ ] **Step 2: Write project-spec skill**

Write `skills/project-development/project-spec/SKILL.md` exactly as:

```markdown
---
name: project-spec
description: Use when a scoped data science conversation should be turned into a formal project spec before creating tasks or implementing work.
disable-model-invocation: true
---

# Project Spec

Synthesize the scoped conversation into a formal data science project spec.

Source lineage: derived from Matt Pocock's `to-spec`, adapted for oh-my-pi and data science project development.

## Required background

Read `../shared/WORKFLOW.md` before acting.

If the spec includes modeling decisions, use `model-plan-discovery` before writing the modeling plan.

If the spec includes PyMC, PyTensor, ArviZ, Bayesian modeling, priors, MCMC, diagnostics, posterior predictive checks, or model comparison, use `pymc-modeling` and any more specific Bayesian skill that applies.

## Process

1. Gather existing context.
   - Read `.project-skills/config.toml` if present.
   - Read the configured domain and data docs if present.
   - Read any scoping notes or conversation artifacts the user names.
2. Do not restart the interview.
   - Synthesize what is already known.
   - Ask only for missing decisions that block a coherent spec.
   - Ask one question at a time when blocked.
3. Choose the output path.
   - Default: `docs/project-skills/specs/YYYY-MM-DD-<short-name>.md`.
   - Use configured `specs_dir` if present.
4. Write the spec using the template below.
5. Run a self-review.
   - Check for unsupported assumptions.
   - Check that data risks are explicit.
   - Check that testing seams are named.
   - Check that out-of-scope work is explicit.
6. Ask the user to review the spec before `create-tasks`.

## Spec template

```markdown
# <Project Name> Project Spec

## Problem statement

Describe the problem from the project user's perspective.

## Project class

One of: research notebook, analysis/modeling, end-to-end ML product.

State why this class was selected.

## Users and stakeholders

List the people or systems that consume the outputs.

## Data sources

For each source, record:

- source name,
- access path or access prerequisite,
- schema or shape if known,
- refresh cadence if known,
- privacy, licensing, or governance constraints,
- known quality issues.

## Target or outcome definition

Define the target, estimand, metric, decision, or reporting outcome.

## Analysis or modeling approach

State the planned approach and why it fits the target and data.

For Bayesian work, include prior strategy, sampling strategy, posterior predictive checks, diagnostics, and model comparison plan when relevant.

## Assumptions and risks

List assumptions, leakage risks, causal limitations, missing-data risks, and external validity limits.

## Deliverables

List notebooks, Python modules, reports, models, datasets, APIs, or deployment handoff artifacts.

## Testing and verification

Describe layered seams:

- reusable module tests,
- notebook or pipeline smoke checks,
- data contract checks,
- metric or diagnostic checks,
- reproducibility checks.

## Out of scope

List work that is explicitly excluded.

## Open questions

List only questions that remain after scoping and do not block task creation.
```

Replace `<Project Name>` and `<short-name>` with a concrete name from the scoped conversation.
```

- [ ] **Step 3: Verify spec-stage skills**

Run: `git diff --check`

Expected: no output and exit code 0.

Open both files and verify:

- each description starts with `Use when`,
- both skills reference `../shared/WORKFLOW.md`,
- `scoping` asks one question at a time,
- `project-spec` does not restart the interview,
- Bayesian hooks delegate to existing skills.

- [ ] **Step 4: Commit task 3**

Run:

```bash
git add skills/project-development/scoping/SKILL.md skills/project-development/project-spec/SKILL.md
git commit -m "Add scoping and project spec skills"
```

Expected: commit succeeds and includes exactly the two skills.

---

### Task 4: Task creation and implementation skills

**Files:**
- Create: `skills/project-development/create-tasks/SKILL.md`
- Create: `skills/project-development/implement/SKILL.md`

**Interfaces:**
- Consumes: project specs written by `project-spec`.
- Produces: task breakdowns and one-task-at-a-time implementation workflow.

- [ ] **Step 1: Write create-tasks skill**

Write `skills/project-development/create-tasks/SKILL.md` exactly as:

```markdown
---
name: create-tasks
description: Use when an approved data science project spec needs to be broken into blocked, agent-sized tasks before implementation.
disable-model-invocation: true
---

# Create Tasks

Break an approved project spec into blocked, agent-sized tasks.

Source lineage: derived from Matt Pocock's `to-tickets`, adapted for oh-my-pi and data science project development.

## Required background

Read `../shared/WORKFLOW.md` before acting.

## Process

1. Load the spec.
   - If the user gives a path, read it.
   - Otherwise find the relevant spec under the configured specs directory.
   - If multiple specs match, ask the user to choose one.
2. Read configured domain and data docs if present.
3. Draft tasks.
   - Prefer vertical tracer bullets.
   - Permit prerequisite stage tasks only for data access, schema profiling, baseline notebook creation, reproducibility setup, or infrastructure needed before a meaningful slice can run.
   - Each task must be small enough for one fresh agent session.
   - Each task must produce a verifiable artifact or decision.
   - Each task must declare blockers.
4. Present the task breakdown for approval.
   - Ask whether the granularity is right.
   - Ask whether blockers are correct.
   - Ask whether any task should be merged or split.
5. After approval, write the task file.
   - Default: `docs/project-skills/tasks/YYYY-MM-DD-<short-name>-tasks.md`.
   - Use configured `tasks_dir` if present.
6. Stop after writing tasks.
   - Do not implement until the user invokes or approves `implement`.

## Task file template

```markdown
# Tasks: <Project Name>

Source spec: <spec path>

Work the frontier: start any task whose blockers are complete.

## Task 1: <Outcome title>

**What to build:** Describe the end-to-end behavior, artifact, or decision this task delivers.

**Blocked by:** None — can start immediately.

**Acceptance criteria:**

- Criterion with observable evidence.
- Criterion with observable evidence.

## Task 2: <Outcome title>

**What to build:** Describe the end-to-end behavior, artifact, or decision this task delivers.

**Blocked by:** Task 1: <Outcome title>.

**Acceptance criteria:**

- Criterion with observable evidence.
- Criterion with observable evidence.
```

When writing the real file, replace every angle-bracket field with concrete project text.
```

- [ ] **Step 2: Write implement skill**

Write `skills/project-development/implement/SKILL.md` exactly as:

```markdown
---
name: implement
description: Use when executing one approved project-skills task for a data science project, after scoping, project-spec, and create-tasks have produced a bounded task.
disable-model-invocation: true
---

# Implement

Execute one approved data science project task at a time.

Source lineage: derived from Matt Pocock's `implement`, adapted for oh-my-pi and data science project development.

## Required background

Read `../shared/WORKFLOW.md` before acting.

Use `verification-before-completion` before claiming completion.

Use `requesting-code-review` when the implementation is substantial or before merge/PR handoff.

Use `marimo-notebook`, `model-plan-discovery`, `pymc-modeling`, `prior-elicitation`, `model-evaluation`, or `pymc-testing` when their triggers match the task.

## Process

1. Load the task.
   - If the user gives a task path, read it.
   - If the user gives a spec path but no task file, ask whether to run `create-tasks` first.
   - Do not execute more than one task unless the user explicitly approves a batch.
2. Load supporting context.
   - Read the source spec.
   - Read domain and data docs if present.
   - Inspect relevant repo files before editing.
3. Confirm the implementation seam.
   - Use the highest stable seam available.
   - For dual-track projects, keep exploration/reporting in marimo and reusable logic in Python modules.
4. Write or update tests/checks first when feasible.
   - Use behavior tests for transformations and reusable modules.
   - Use smoke checks for notebooks or pipelines.
   - Use model diagnostics or metric checks for modeling tasks.
5. Implement the minimal change that satisfies the task.
6. Verify the task.
   - Run the specific tests/checks that cover the changed behavior.
   - Run notebook or pipeline smoke checks when the artifact is a notebook or pipeline.
   - Run Bayesian diagnostics when the task includes PyMC, MCMC, priors, or model comparison.
7. Update docs only for facts changed by this task.
8. Report evidence.
   - Files changed.
   - Checks run.
   - Outputs regenerated.
   - Remaining risks.

## Guardrails

- Do not silently widen scope.
- Do not create placeholder notebooks, fake data, fake models, no-op fallbacks, or stub implementations as delivered work.
- Do not duplicate existing PyMC, ArviZ, marimo, or model-evaluation guidance; invoke the relevant skill.
- Do not claim scientific validity from code tests alone.
- Do not skip reproducibility evidence for data science artifacts.

## Completion output

End with:

```markdown
Implemented task: <task title>

Evidence:
- Check: <command or scenario> — <observed result>
- Check: <command or scenario> — <observed result>

Changed artifacts:
- <path>: <why it changed>

Risks:
- <risk or None>
```

Replace every angle-bracket field with concrete task evidence.
```

- [ ] **Step 3: Verify task and implementation skills**

Run: `git diff --check`

Expected: no output and exit code 0.

Open both files and verify:

- `create-tasks` requires task approval before implementation,
- `create-tasks` declares blocker and acceptance-criteria requirements,
- `implement` executes one task at a time,
- `implement` delegates to existing domain skills,
- `implement` requires verification evidence.

- [ ] **Step 4: Commit task 4**

Run:

```bash
git add skills/project-development/create-tasks/SKILL.md skills/project-development/implement/SKILL.md
git commit -m "Add task creation and implementation skills"
```

Expected: commit succeeds and includes exactly the two skills.

---

### Task 5: Project review skill

**Files:**
- Create: `skills/project-development/project-review/SKILL.md`

**Interfaces:**
- Consumes: completed diff, source spec/task, domain/data docs, and configured review axes.
- Produces: adaptive project review report.

- [ ] **Step 1: Write project-review skill**

Write `skills/project-development/project-review/SKILL.md` exactly as:

```markdown
---
name: project-review
description: Use when reviewing completed data science project work for code quality, reproducibility, scientific validity, data governance, Bayesian diagnostics, or deployment readiness.
---

# Project Review

Review completed data science work across separate axes so code quality does not mask analytic or reproducibility failures.

Source lineage: derived from Matt Pocock's `code-review`, adapted for oh-my-pi and data science project development.

## Required background

Read `../shared/WORKFLOW.md` before acting.

Use `pymc-modeling` and `model-evaluation` when the diff or spec includes PyMC, PyTensor, ArviZ, priors, MCMC, posterior predictive checks, LOO, ELPD, stacking, or Bayesian model comparison.

## Process

1. Pin the review target.
   - If the user supplies a fixed point, use it.
   - If no fixed point is supplied, ask for one.
   - Confirm the diff is non-empty before reviewing.
2. Find the source spec or task.
   - Prefer a path the user supplied.
   - Otherwise look under configured specs and tasks directories.
   - If no spec exists, run code quality and reproducibility axes and mark spec-dependent axes as unavailable.
3. Determine axes.
   - Always run code quality.
   - Always run reproducibility.
   - Add scientific validity for analysis/modeling work.
   - Add Bayesian diagnostics for PyMC, PyTensor, ArviZ, priors, MCMC, posterior predictive checks, or model comparison.
   - Add data governance for sensitive, external, regulated, licensed, or production-bound data.
   - Add deployment readiness for end-to-end ML products.
4. Run independent review passes.
   - Use subagents for independent axes when there is enough context and the axes do not need shared state.
   - Keep axis findings separate.
5. Write the review report.
   - Default: `docs/project-skills/reviews/YYYY-MM-DD-<short-name>-review.md`.
   - Use configured `reviews_dir` if present.

## Axis checks

### Code quality

Check documented repo standards, maintainability, naming, unnecessary abstraction, duplicated logic, and tests for behavior rather than implementation details.

### Reproducibility

Check that the work can be rerun from documented inputs with documented commands, stable environments, deterministic seeds where appropriate, and regenerated outputs when required.

### Scientific validity

Check target definition, assumptions, leakage, missing data, validation design, metric choice, uncertainty, limitations, and whether conclusions are supported by evidence.

### Bayesian diagnostics

Check prior justification, prior predictive checks, sampler configuration, convergence diagnostics, divergences, effective sample size, posterior predictive checks, log likelihood availability for comparison, and LOO/ELPD usage when models are compared.

### Data governance

Check source permissions, privacy constraints, licensing, data retention assumptions, leakage paths, derived-data sensitivity, and whether risky data handling is documented.

### Deployment readiness

Check operational interface, batch or service behavior, monitoring handoff, failure modes, dependency assumptions, and rollback or retraining notes when relevant.

## Report template

```markdown
# Project Review: <short name>

Fixed point: <fixed point>
Source spec/task: <path or unavailable>

## Code quality

Findings:

- Severity: blocker | major | minor
  Evidence: quote or path
  Issue: concrete issue
  Recommendation: concrete fix

## Reproducibility

Findings:

- Severity: blocker | major | minor
  Evidence: quote or path
  Issue: concrete issue
  Recommendation: concrete fix

## Adaptive axes

Include only axes that were triggered.

## Summary

- Code quality findings: count and worst severity.
- Reproducibility findings: count and worst severity.
- Adaptive-axis findings: count by axis and worst severity.
```

Replace every angle-bracket field with concrete review facts.
```

- [ ] **Step 2: Verify project-review skill**

Run: `git diff --check`

Expected: no output and exit code 0.

Open the file and verify:

- description starts with `Use when`,
- code quality and reproducibility are always-run axes,
- Bayesian diagnostics are adaptive,
- findings stay separated by axis,
- the skill writes a durable review report.

- [ ] **Step 3: Commit task 5**

Run:

```bash
git add skills/project-development/project-review/SKILL.md
git commit -m "Add project review skill"
```

Expected: commit succeeds and includes exactly the project review skill.

---

### Task 6: Minimal evals and final verification

**Files:**
- Create: `evals/evals.json`
- Modify: `README.md` if the final file list or eval location differs from Task 1.

**Interfaces:**
- Consumes: all six skills and shared workflow reference.
- Produces: minimal eval prompt set and final verification evidence.

- [ ] **Step 1: Write eval prompts**

Write `evals/evals.json` exactly as:

```json
{
  "skill_name": "project-skills",
  "evals": [
    {
      "id": 1,
      "prompt": "Use scoping for a project where I have a CSV of patient appointment history and want a marimo notebook that explains no-show patterns for clinic managers. I am not sure whether this is just exploratory analysis or a model-building project.",
      "expected_output": "The agent should infer a research notebook or analysis/modeling class, ask the user to confirm, ask one scoping question at a time, and capture data/privacy/leakage concerns before proposing a spec.",
      "files": []
    },
    {
      "id": 2,
      "prompt": "Use scoping for a project where we need a daily batch model that predicts customer churn, writes scores to a warehouse table, and has a dashboard for monitoring drift.",
      "expected_output": "The agent should infer end-to-end ML product, confirm that classification, identify deployment and monitoring concerns, and record data governance and reproducibility risks.",
      "files": []
    },
    {
      "id": 3,
      "prompt": "Use project-spec on this scoped conversation: we are building a Bayesian hierarchical model in PyMC for store-level demand, with partial pooling by region, weekly seasonality, posterior predictive checks, and LOO comparison against a pooled baseline.",
      "expected_output": "The agent should invoke or require PyMC/model-evaluation guidance, write a spec with priors, sampling, posterior predictive checks, diagnostics, log likelihood, and LOO/ELPD comparison decisions.",
      "files": []
    },
    {
      "id": 4,
      "prompt": "Use create-tasks for a spec that includes data access, schema profiling, a baseline marimo notebook, reusable feature engineering code, model fitting, metric reporting, and a final reproducibility review.",
      "expected_output": "The agent should create blocked tasks that prefer vertical tracer bullets but allow data access, schema profiling, baseline notebook, and reproducibility setup as prerequisite tasks.",
      "files": []
    },
    {
      "id": 5,
      "prompt": "Use implement for the task: add a tested function that validates a Polars data frame has the columns needed for a downstream model, then smoke-test the marimo notebook that calls it.",
      "expected_output": "The agent should execute only the named task, write or update behavior tests first where feasible, implement the function, run targeted tests and notebook smoke checks, and report evidence.",
      "files": []
    },
    {
      "id": 6,
      "prompt": "Use project-review on a completed PyMC modeling task that changed model code, generated posterior predictions, and updated a report, comparing the branch to main.",
      "expected_output": "The agent should review code quality and reproducibility, add Bayesian diagnostics and scientific validity axes, keep findings separated, and write a durable review report.",
      "files": []
    }
  ]
}
```

- [ ] **Step 2: Validate JSON**

Run a JSON parser on `evals/evals.json` using the project's available environment. In an oh-my-pi eval cell, this equivalent check is sufficient:

```python
import json
from pathlib import Path
json.loads(Path("evals/evals.json").read_text())
```

Expected: no exception.

- [ ] **Step 3: Run final text checks**

Run: `git diff --check`

Expected: no output and exit code 0.

Search the new plugin files for unresolved placeholders using the repo search tool or equivalent regex:

```text
TBD|TODO|PLACEHOLDER|\?\?\?
```

Expected: no matches in plugin files or evals. The approved design spec may contain historical text and is not part of this check.

- [ ] **Step 4: Verify manifest paths**

For each path in `.claude-plugin/plugin.json`, verify that a `SKILL.md` file exists:

```text
skills/project-development/setup-project-skills/SKILL.md
skills/project-development/scoping/SKILL.md
skills/project-development/project-spec/SKILL.md
skills/project-development/create-tasks/SKILL.md
skills/project-development/implement/SKILL.md
skills/project-development/project-review/SKILL.md
```

Expected: all six files exist.

- [ ] **Step 5: Verify source coverage**

Read `docs/superpowers/specs/2026-07-08-data-science-project-skills-design.md` and check each acceptance criterion against the created files:

- plugin manifest exists,
- all six skills have complete `SKILL.md` files,
- README attribution exists,
- shared artifact model exists,
- minimal eval prompts exist,
- OMP-native tool/delegation wording exists,
- domain-skill delegation exists,
- full idea-to-review workflow is represented.

Expected: every criterion is covered by a committed file.

- [ ] **Step 6: Commit task 6**

Run:

```bash
git add evals/evals.json README.md .claude-plugin/plugin.json skills/project-development
git commit -m "Add project skills eval prompts"
```

Expected: commit succeeds. If README and manifest have no changes from earlier tasks, Git commits only `evals/evals.json`.

- [ ] **Step 7: Final status report**

Run: `git status --short`

Expected: no uncommitted changes.

Report:

```markdown
Implementation plan complete.

Created plugin files:
- .claude-plugin/plugin.json
- README.md
- skills/project-development/shared/WORKFLOW.md
- skills/project-development/setup-project-skills/SKILL.md
- skills/project-development/scoping/SKILL.md
- skills/project-development/project-spec/SKILL.md
- skills/project-development/create-tasks/SKILL.md
- skills/project-development/implement/SKILL.md
- skills/project-development/project-review/SKILL.md
- evals/evals.json

Verification:
- git diff --check: passed
- evals/evals.json JSON parse: passed
- placeholder scan: passed
- manifest path check: passed
- design acceptance coverage: passed
```
