# Data Science Project Skills Plugin Design

## Status

Approved design from grilling session on 2026-07-08. Awaiting user review of this written spec before implementation planning.

## Source and attribution

This plugin will be derived from Matt Pocock's `mattpocock/skills` repository, especially these MIT-licensed source skills:

- `setup-matt-pocock-skills`
- `grill-with-docs`
- `wayfinder`
- `to-spec`
- `to-tickets`
- `implement`
- `code-review`

The new plugin will explicitly attribute the source repository in its README and in each adapted skill's notes.

## Goal

Create an oh-my-pi-native plugin for developing data science projects from vague idea to verified implementation.

The plugin must support three project classes:

1. Research notebook projects.
2. Analysis/modeling projects.
3. End-to-end ML products.

The workflow should infer the project class from the user's goal, proposed artifacts, data risks, and implementation needs, then ask the user to confirm or correct that classification.

## Non-goals

- Do not build a generic Claude Code slash-command plugin.
- Do not create a loose, unversioned pile of unrelated skills.
- Do not make every project go through the strictest end-to-end ML process.
- Do not duplicate existing oh-my-pi domain skills for PyMC, ArviZ, marimo, model evaluation, prior elicitation, or PyMC testing.
- Do not ship stubs or TODO-only skill skeletons as v1.

## Packaging decision

Use a single installable plugin as the primary artifact.

Rationale:

- The requested workflow is a coherent lifecycle, not independent utilities.
- A plugin can version the skills, manifest, attribution, eval prompts, and shared conventions together.
- Matt Pocock's source repository already uses a plugin-style structure with a manifest plus skill directories.
- oh-my-pi users should invoke stage-specific skills without reverse-engineering how they compose.

## Plugin architecture

The v1 plugin contains six user-facing skills.

### `setup-project-skills`

Purpose: configure project-level workflow defaults.

Source analogue: `setup-matt-pocock-skills`.

Responsibilities:

- Configure the artifact backend.
- Record documentation paths.
- Record preferred notebook/package policy.
- Record tracker backend if available.
- Record package manager and validation command hints when discoverable.
- Record optional domain skill availability.
- Prefer local markdown by default, with hooks for future tracker backends.

### `scoping`

Purpose: convert a vague data science idea into a scoped project direction.

Source analogue: `grill-with-docs` plus `wayfinder`.

Responsibilities:

- Run a one-question-at-a-time grilling session.
- Infer the project class, then ask the user to confirm it.
- Capture the decision target: research notebook, analysis/modeling project, or ML product.
- Build or update domain language.
- Build or update data assumptions and data inventory.
- Surface unresolved fog.
- Create investigation tasks only when the route is not yet clear.

### `project-spec`

Purpose: turn the scoped conversation into a formal data science project spec.

Source analogue: `to-spec`.

Responsibilities:

- Synthesize without re-interviewing unless a required decision is missing.
- Capture the problem, intended users, data sources, target/outcome, assumptions, methods, validation plan, artifacts, risks, testing seams, and out-of-scope work.
- Use layered test seams:
  - reusable Python modules for transformations/model code,
  - notebook or pipeline smoke checks,
  - statistical/model checks for diagnostics, metrics, or uncertainty.
- Preserve the dual-track artifact strategy: marimo for exploration/reporting, tested Python modules for reusable logic.

### `create-tasks`

Purpose: break the project spec into blocked, agent-sized tasks.

Source analogue: `to-tickets`.

Responsibilities:

- Prefer vertical tracer bullets.
- Permit stage-specific prerequisite tasks only when unavoidable.
- Require each task to produce a verifiable artifact or decision.
- Declare blocking edges explicitly.
- Keep task descriptions outcome-oriented, not file-path-oriented.

Allowed prerequisite task types:

- Data access.
- Schema profiling.
- Baseline notebook creation.
- Reproducibility setup.
- Infrastructure needed before a meaningful project slice can run.

### `implement`

Purpose: execute one approved task at a time.

Source analogue: `implement`.

Responsibilities:

- Work from an approved task or spec slice.
- Keep implementation bounded to one task.
- Use existing repo conventions and project configuration.
- Delegate to existing oh-my-pi domain skills when relevant.
- Use test-first or check-first implementation where appropriate.
- Verify behavior before yielding.
- For data science work, verify both code behavior and analytic reproducibility appropriate to the project class.

Expected domain-skill delegation:

- `marimo-notebook` for marimo notebooks.
- `marimo-pair` for active paired notebook sessions.
- `pymc-modeling` for PyMC, PyTensor, ArviZ, Bayesian modeling, MCMC, priors, diagnostics, and probabilistic programming.
- `model-plan-discovery` before statistical, Bayesian, ML, or data-science modeling plans.
- `prior-elicitation` for priors and prior predictive checks.
- `model-evaluation` for LOO, ELPD, stacking, and Bayesian model comparison.
- `pymc-testing` for tests touching PyMC models or sampling.
- `requesting-code-review` and `verification-before-completion` when implementation is complete.

### `project-review`

Purpose: review completed data science work against implementation quality, reproducibility, and analytic validity.

Source analogue: `code-review`, expanded for data science.

Responsibilities:

- Always review code quality.
- Always review reproducibility.
- Add adaptive axes based on project class and content:
  - scientific validity,
  - data governance,
  - Bayesian diagnostics,
  - deployment readiness.
- Keep axes separate so a project can pass one and fail another.
- Report findings with evidence and severity.

## Shared artifact model

Default backend: local markdown.

`setup-project-skills` may override paths, but v1 defaults are:

```text
.project-skills/config.toml
docs/project-skills/domain.md
docs/project-skills/data.md
docs/project-skills/specs/
docs/project-skills/tasks/
docs/project-skills/reviews/
evals/
```

### `.project-skills/config.toml`

Workflow configuration.

Expected fields:

- artifact backend.
- docs paths.
- tracker backend if configured.
- preferred project class if the repo has one.
- notebook policy.
- package/test/lint command hints.
- optional domain skill availability.

### `docs/project-skills/domain.md`

Glossary and domain decisions.

Contents:

- Ubiquitous language.
- Project-specific terms.
- Domain assumptions.
- Hard-to-explain decisions.

### `docs/project-skills/data.md`

Data inventory and data-risk record.

Contents:

- Data sources.
- Access paths or access prerequisites.
- Schema notes.
- Known quality issues.
- Target/outcome definitions.
- Leakage risks.
- Privacy or licensing constraints.
- Reproducibility requirements.

### `docs/project-skills/specs/`

Formal project specs generated by `project-spec`.

### `docs/project-skills/tasks/`

Task breakdowns generated by `create-tasks`.

### `docs/project-skills/reviews/`

Review reports generated by `project-review`.

### `evals/`

Minimal plugin eval prompts.

## Workflow behavior

### Stage 1: setup

`setup-project-skills` configures repo-local defaults and avoids repeated questions.

If no setup has run, later skills should infer sensible defaults and offer to create the config when useful.

### Stage 2: scoping

`scoping` starts with a loose project idea.

It should:

1. Inspect available repo context before asking questions.
2. Infer project class.
3. Confirm or correct the classification with the user.
4. Ask one question at a time.
5. Capture domain and data assumptions.
6. Identify fog.
7. Decide whether the project is ready for `project-spec` or needs investigation tasks first.

### Stage 3: project-spec

`project-spec` converts the scoped conversation into a formal spec.

It should not restart the interview unless a required decision is missing.

It should produce a spec containing:

- Problem statement.
- Project class.
- Users/stakeholders.
- Data sources and access constraints.
- Target/outcome definitions.
- Analysis/modeling approach.
- Assumptions.
- Validation plan.
- Reproducibility plan.
- Deliverables.
- Testing decisions.
- Out of scope.
- Open questions.

### Stage 4: create-tasks

`create-tasks` converts the spec into blocked tasks.

Task policy:

- Prefer vertical tracer bullets.
- Allow prerequisite stage tasks only when vertical slicing would be fake.
- Every task must have acceptance criteria.
- Every task must be small enough for one fresh agent session.
- Every task must declare blockers.

### Stage 5: implement

`implement` executes one approved task.

It should:

- Load the relevant task and spec.
- Use the repo's configured tools.
- Use existing oh-my-pi domain skills instead of duplicating their knowledge.
- For Python projects, follow project environment conventions.
- Prefer polars over pandas unless the repo requires otherwise.
- Prefer marimo over Jupyter unless the repo requires otherwise.
- Verify the artifact relevant to the task.

### Stage 6: project-review

`project-review` reviews the completed implementation.

Always-run axes:

1. Code quality.
2. Reproducibility.

Adaptive axes:

- Scientific validity for analysis/modeling projects.
- Bayesian diagnostics for PyMC/ArviZ/PyTensor projects.
- Data governance for sensitive, external, or regulated data.
- Deployment readiness for end-to-end ML products.

## Data governance policy

Use a risk-scaled default.

Always ask or infer:

- data source,
- schema or shape,
- target/outcome,
- leakage risks,
- privacy/licensing constraints,
- reproducible access path or access prerequisite.

Escalate to stricter handling only for sensitive, external, regulated, or production-bound data.

## Testing and verification philosophy

Use scientific review gates.

Implementation is not verified merely because code tests pass.

Depending on project class, verification may include:

- unit tests for transformations and reusable modules,
- notebook or pipeline smoke runs,
- regenerated output artifacts,
- fixed seeds where appropriate,
- data contract checks,
- metric checks,
- posterior/prior predictive checks,
- MCMC diagnostics,
- model comparison checks,
- uncertainty and limitation reporting,
- leakage review,
- reproducibility review.

## Minimal v1 eval prompts

V1 should include minimal eval prompts for the plugin itself.

The eval set should cover:

1. Scoping a research notebook project.
2. Scoping an end-to-end ML product.
3. Creating a spec from a Bayesian modeling conversation.
4. Breaking a project spec into blocked tasks.
5. Implementing one bounded data-prep or modeling task.
6. Reviewing a completed project with adaptive axes.

The full skill-creator baseline/with-skill benchmark loop is out of scope for v1 unless the user later requests it.

## Implementation acceptance criteria

A usable v1 is complete when:

- The plugin has a manifest listing all skills.
- Each skill has a complete `SKILL.md` with frontmatter and actionable instructions.
- The plugin includes README/attribution text.
- The plugin defines the shared artifact model.
- The plugin includes minimal eval prompts.
- The skills are OMP-native and reference OMP tools and delegation patterns directly.
- The skills delegate to existing domain skills instead of duplicating their full contents.
- The workflow can take a data science project from vague idea through scoped spec, tasks, implementation, and review.

## Open implementation details for the planning phase

- Exact plugin manifest format for oh-my-pi in this repository.
- Exact install/package command, if any.
- Whether README should live at repo root or plugin root.
- Exact config schema for `.project-skills/config.toml`.
- Exact eval JSON schema to use for minimal v1 prompts.
