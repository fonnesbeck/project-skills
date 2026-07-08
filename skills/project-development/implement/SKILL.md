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
   - For Python tabular-data work, prefer Polars over pandas unless the repo already standardizes on pandas or the task requires pandas.
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
