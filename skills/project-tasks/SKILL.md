---
name: project-tasks
description: Use when an approved data science project spec needs to be broken into blocked, agent-sized tasks before implementation.
disable-model-invocation: true
---

# Project Tasks

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
   - Do not implement until the user invokes or approves `project-implement`.

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
