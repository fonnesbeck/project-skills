---
name: project-skills-setup
description: Use when configuring a repository to use the project-skills data science workflow, especially before scoping, specs, task creation, implementation, or project review in a new repo.
disable-model-invocation: true
---

# Project Skills Setup

Configure repo-local defaults for the project-skills workflow.

Source lineage: derived from Matt Pocock's `setup-matt-pocock-skills`, adapted for oh-my-pi and data science project development.

## Required background

Read `skill://project-skills-shared/WORKFLOW.md` before acting.

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
