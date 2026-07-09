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
