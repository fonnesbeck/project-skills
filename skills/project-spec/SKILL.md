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
