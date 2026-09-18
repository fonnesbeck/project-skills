---
name: project-implement
description: Use to implement one authorized, bounded data science increment from a current spec, verifier design, and increment-ready environment, then hand real evidence to verifier check.
---

# Project Implement

Implement the smallest authorized increment, gather real evidence, and continue to verifier check. No task file or task graph is required.

Source lineage: derived from Matt Pocock's `implement`, adapted for oh-my-pi and data science project development.

## Readiness gate

Read `skill://project-skills-shared/WORKFLOW.md` first; it owns routing, authority, handoff fields, transitions, and invalidation. Load applicable domain skills listed there. Optional completion/review skills can assist, not replace verifier check; unavailable optional skills are not mandatory setup dependencies.

Before editing:

1. Read the spec, versioned verifier design, and persistent environment reference. Match their increment/spec/revision bindings and confirm implementation authority.
2. Require criterion IDs, methods, evidence sources, decision rules, and failure actions. Return ambiguity to verifier **design**; changed scope or claims go to `project-spec`.
3. Require current **increment readiness**, not merely a prepared environment **foundation**. Foundation without spec/design cannot authorize implementation. Have `project-environment` bind capabilities, data/evidence access, and permissions to the current spec/design; refresh affected readiness when inputs change. Control gaps block affected unsafe work, not independent safe work.
4. Record permitted changes, stage-only limits, stopping condition, and explicit repair allowance; assume no repairs if undeclared.
5. Identify a recoverable checkpoint and starting source/data/artifact identities using existing conventions. Do not force a commit, reset user work, or overwrite prior evidence.

Missing prerequisites require the shared blocked handoff, not invented criteria, permissions, or a task ledger.

## Execute one increment

1. **Inspect the seam.** Read relevant source/conventions; prefer stable interfaces. In dual-track projects, keep exploration/reporting in marimo and reusable logic in Python modules. Prefer Polars unless repository standards or requirements favor pandas.
2. **Change only what is authorized.** Reuse project commands, skills, and controls. No speculative features, placeholders, fake deliverables, no-op fallbacks, or unrelated refactors. Never bypass denied access through another tool; ask before impactful out-of-authority operations.
3. **Gather designed evidence.** Exercise actual changed behavior through commands, scenarios, notebook/pipeline runs, or scientific evaluations. Record configuration, observations, source revision, and artifact references; separate observation from interpretation. Skipped, inaccessible, or interrupted checks did not pass.
4. **Test contracts, not activity.** Fix behavior-affected existing tests; retain plausible-bug regressions where useful. Use throwaway smoke checks for straightforward new behavior, not indiscriminate suites. Mocks cannot prove connectivity or scientific validity.
5. **Preserve reproducibility/privacy.** Record source revision/fingerprint, environment/dependencies, data/protocol versions, seeds, and settings. Store necessary raw outputs only in authorized storage; expose aggregate/redacted references in workflow documents. Follow the leakage-safe protocol; never tune quietly on held-out data.
6. **Finish the delivered surface.** Follow notebook skills/project cleanup commands: remove stale execution state, debug cells, unused imports, and unintended sensitive outputs without deleting scientific evidence. Re-execute/smoke-check the delivered notebook after cleanup. Update existing docs for changed facts; remove throwaway scripts and accidental artifacts, preserving intended outputs/checkpoints.
7. **Stop spawned notebook processes.** Stop every server, kernel, and execution process this run spawned; verify none remain with `ps`. Never terminate unrelated user processes. Runtime cleanup does not replace checking the notebook.

Compilation, finite samples, rendering, and passing code tests do not establish scientific validity. Missing data/compute means unavailable evidence, not success.

## Conditional calibration: implementation owns records

Apply `project-calibration-repair` only to an explicit probabilistic model **with** an assessable inferential/predictive claim. Otherwise state the absent condition; create no calibration record, status, budget, or loop.

For eligible work:

1. Read the predeclared calibration plan: intended use, authorized data, leakage-safe protocol, gates/rules, reproducibility, `calibration_record_reference` location, and repair authority/budget. Missing preconditions stay unresolved; never invent thresholds after results.
2. Identify the candidate revision and fitted inference run. Invoke calibration for each new candidate/run/data/protocol tuple needing evaluation, checking every applicable gate. Reuse evidence only for the identical tuple and current design.
3. Persist the returned **immutable** record at the planned canonical artifact location after candidate and inference evidence exist. Include `calibration_record_reference` in `evidence_references`; link protected raw outputs and preserve prior candidate records.

| Calibration result | Implementation action |
|---|---|
| `calibrated` | Submit the record. It supports only the declared claim/gates—not causal proof, production approval, or automatic model selection. |
| `repair_required` | Preserve the failure. Only if failure action and remaining allowance permit, make one bounded candidate revision, record rationale, regenerate evidence, and invoke calibration again. Charge the revision to the same allowance across stages. |
| `unresolved` | Report missing, infeasible, or unreliable evidence; stop affected calibration work. Inference-health failure is not likelihood failure, and code tests cannot substitute. |

The specialist diagnoses/advises; implementation alone changes model source and persists candidate records. Verifier consumes them read-only and returns missing/stale records here. Exhausted allowance means stop, never weaker gates.

## Verifier check and bounded repair

Automatically hand the candidate to `project-verifier` **check** within existing authority. Supply exact shared references, implementation revision, and all evidence—including incomplete/failing evidence. An explicit implementation-only boundary stops with evidence and names check as next action, without claiming verification.

| Check outcome | Next action |
|---|---|
| Pass | Preserve accepted checkpoint/evidence. Completion requires all applicable required criteria satisfied, justified exclusions, and no blocking unresolved result. |
| Fail | Follow the failure action within unchanged scope/criteria and remaining allowance. Preserve the failed revision; make one identifiable repair, regenerate affected evidence (including applicable calibration), and return to check. Justify reuse of unaffected evidence. |
| Unresolved | Name the missing prerequisite/unreliable evidence and responsible stage/action. Continue only safe independent authorized work; never manufacture an observation. |
| Scope/criterion change | Return through spec/design and shared invalidation/readiness rules before implementation. Never move a threshold to pass a failed candidate. |

Stop at verification, explicit stage limit, exhausted authority, genuine blocker, or user stop. Further authorized increments return to spec; do not expand this one or abandon authorized mechanical handoffs.

## Completion or blocked handoff

Use shared fields: `verifier_reference` remains the **design path + revision**; the check report belongs in `evidence_references`.

Report changed artifacts/rationale, actual commands and observations, criterion/calibration references, preserved checkpoint, allowance used/remaining, risks/blockers, and next authorized action. Implementation evidence is not its own acceptance verdict; never describe an unverified or calibration-limited candidate as complete scientific success.
