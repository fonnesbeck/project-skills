---
name: project-spec
description: Use to clarify a data science goal, interview for consequential decisions, and specify one bounded increment before verifier design and implementation.
---

# Project Spec

Turn the user's goal into one small, useful increment with an explicit checkpoint. Discover the goal before choosing a solution; no full-project plan or task ledger is required.

Source lineage: derived from Matt Pocock's `grill-with-docs`, `wayfinder`, and `to-spec`, adapted for oh-my-pi and data science project development.

## Boundary

Read `skill://project-skills-shared/WORKFLOW.md` first. It owns artifact routing, authority, handoff fields, stage transitions, and revision invalidation. Load matching domain skills for subject-matter guidance, not another workflow.

This stage owns discovery and specification—not source changes, environment setup, fitting, calibration records, or executable verifier design. Respect spec-only and interview-only limits; existing notes are context, not renewed authority.

## Process

1. **Ground discovery.** Inspect relevant instructions, source, outputs, and canonical vault context using shared routing before asking questions.
2. **Find the goal.** Identify the consumer, decision or behavior, and useful success. Distinguish exploration, inference, prediction, and operational use when consequential; infer deliverable shape without a classification ceremony.
3. **Resolve consequential unknowns.** Ask one focused question at a time, with a recommendation and tradeoff where useful. Resolve target meaning, data authority, scope, costs, and decision-changing assumptions. Confirm choices not already authorized; label reversible technical assumptions. Do not ask repository-answerable questions, repeat settled answers, or write a confident spec for an unclear goal.
4. **Bound one increment.** Define inputs, outputs, non-goals, and the smallest observable end-to-end result. An access or data-understanding prerequisite can itself be the increment; state its evidentiary limits.
5. **State acceptance intent.** Record required outcomes, constraints, rationale, and user-agreed thresholds. Leave criterion IDs, methods, evidence sources, diagnostic rules, and failure actions to verifier design. Flag requirements that block meaningful design.
6. **Set authority and checkpoint.** Record permitted work/data/resources, stopping condition, reviewable result, and consequential decisions needed before expansion. Do not infer permission for acquisition, costly inference, publication, or deployment.
7. **Persist and continue.** Use the shared spec location/frontmatter and handoff fields, including stable `increment_id` and identifiable `spec_revision`. Summarize decisions and blockers. Automatically continue to `project-verifier` **design** within existing authority; otherwise report the exact missing decision or stage-only stop.

## Conditional data-science safeguards

Include only applicable safeguards; reporting and data engineering need no modeling checklist.

| Area | Required specification |
|---|---|
| Data and claims | Target/estimand/metric; input shape and quality; access, privacy and licensing; leakage and missingness; reproducibility; causal/external-validity limits. Reference protected data, never copy raw records. |
| Modeling | Explain the candidate and how observations inform its claim before proposing replacement. Lack of validation is not demonstrated failure; redesign requires a named flaw and supporting evidence. |
| Forecasting | Connect forecast-origin information, horizon, and baseline. For development forecasts distinguish current ability, expected change, and unexplained variation. Justify latent-state granularity by observation support and cost. Define instant/period semantics, observation/publication lags, and which observations inform each state without future leakage. |

**Calibration gate:** only an explicit probabilistic model **and** an assessable inferential/predictive claim activate `project-calibration-repair`. Specify intended use, authorized data, leakage-safe evaluation intent, decision-relevant discrepancies, and evidence/resource constraints. Assign the downstream roles explicitly in the spec: verifier design makes these requirements executable; implementation produces fitted evidence and immutable calibration records; verifier check performs read-only acceptance checking against the declared criteria. Spec issues no calibration verdict. Outside the gate, require no calibration record, budget, or loop.

## Spec body

Use this outline without irrelevant boilerplate; prepend shared vault plan frontmatter.

```markdown
# <Increment>: specification

## Goal and consumer
Decision or observable behavior, consumer, and value.

## Bounded increment
Inputs, outputs, deliverables, and explicit non-goals.

## Context and decisions
Source references, confirmed choices, reversible assumptions, data authority,
risks, and blocking unknowns.

## Approach
The smallest coherent approach; conditional modeling/forecasting rationale.

## Acceptance intent
Observable outcomes, constraints, rationale, and agreed thresholds.

## Authority and checkpoint
Permitted work/data/resources, stopping condition, reviewable result,
and decisions required before expansion.

## Handoff
Applicable shared fields, including this spec's identity/revision, readiness
or blockers, and the next authorized action.
```

Keep scope decisions authoritative in one place. Apply shared invalidation rules after substantive revisions; return to verifier design before downstream work.
