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

## Stage ownership

When invoked as `/scoping`, this skill exclusively owns the scoping stage.
Other skills — including generic brainstorming, planning, specification,
task-breakdown, or implementation workflows — may be invoked when their
guidance helps answer the user’s actual question, but never hand the scoping
stage off to them.

Skills invoked during scoping, including those named in Required background,
provide subject-matter guidance only; they do not take process control,
choose artifact paths, or replace this scoping process.

During `/scoping`, do not create a project spec, task file, implementation
plan, or review. Continue to the next project-skills stage only when this
skill has reached its stop condition and the user explicitly invokes or
approves that stage.

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
   - Follow the Stage ownership boundary.
   - Do not write the project spec unless the user invokes or approves
     `project-spec`.
   - Do not create a task breakdown or implementation plan.

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
