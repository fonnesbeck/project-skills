---
name: project-spec
description: Use when a scoped data science conversation should be turned into a formal project spec before creating tasks or implementing work.
disable-model-invocation: true
---

# Project Spec

Synthesize the scoped conversation into a formal data science project spec.

Source lineage: derived from Matt Pocock's `to-spec`, adapted for oh-my-pi and data science project development.

## Required background

Read `skill://project-skills-shared/WORKFLOW.md` before acting.

If the spec includes modeling decisions, use `model-plan-discovery` before writing the modeling plan.

If the spec includes PyMC, PyTensor, ArviZ, Bayesian modeling, priors, MCMC, diagnostics, posterior predictive checks, or model comparison, use `pymc-modeling` and any more specific Bayesian skill that applies.

If the scoped project has an explicit probabilistic model and an assessable inferential or predictive claim, define its calibration plan in this stage. Reserve `project-calibration-repair` for evaluating a candidate revision after its fitted inference evidence exists. Do not require calibration repair for descriptive analysis, reporting-only notebooks, data engineering, deterministic transformations, or other projects without an explicit model.

## Process

1. Gather existing context.
   - Read `.project-skills/config.toml` if present.
   - Read the configured domain and data docs if present.
   - Read any scoping notes or conversation artifacts the user names.
   - Determine whether the project-calibration-repair applicability gate is met. Record why it applies when it does; otherwise retain the ordinary specification workflow without a project-calibration-repair requirement.
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
6. Ask the user to review the spec before `project-tasks`.

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

For an eligible probabilistic-model project, include the project-calibration-repair applicability decision and a calibration plan: authorized data access; an evaluation protocol appropriate to the intended use, including leakage-safe predictive validation when the claim is predictive; model-specific diagnostic statistics and decision rules; reproducibility evidence; the `calibration_record_reference` location; and the bounded repair authority/budget.
State that later calibration assessment consumes a versioned candidate and its fitted inference evidence, produces the separate per-candidate record, then returns control to the authorized stage for candidate creation. This stage only declares that lifecycle; do not run assessment, create a record, mutate model source, select a repair, or authorize deployment.

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
