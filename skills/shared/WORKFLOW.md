# Project Skills Shared Workflow

This reference is shared by the project-development skills.

## Source lineage

This plugin is derived from Matt Pocock's `mattpocock/skills` repository under the MIT License. The adapted source skills are `setup-matt-pocock-skills`, `grill-with-docs`, `wayfinder`, `to-spec`, `to-tickets`, `implement`, and `code-review`.

## Workflow stage ownership

A project-skills command owns its active workflow stage. It determines the
questions to ask, the artifacts it may create, and the condition for moving
to the next stage.

While a project-skills stage is active, other skills — including generic
brainstorming, planning, specification, task-breakdown, or implementation
workflows — may be invoked when their guidance is needed, but the stage is
never handed off to them. They do not take process control, select an
artifact path, replace the active stage, or advance the workflow.
Domain-specific skills likewise supply subject-matter guidance only.

A stage may create only the artifacts named by that stage. In particular,
`project-scoping` does not create a project spec, task file, implementation
plan, or review. Those artifacts belong respectively to `project-spec`,
`project-tasks`, `project-implement`, and `project-review` after their
required approval gates.

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
- Use `project-calibration-repair` only when a project has an explicit probabilistic model and an assessable inferential or predictive claim; projects that fail this gate continue their ordinary workflow.
- Use `requesting-code-review` and `verification-before-completion` before claiming implementation complete.
