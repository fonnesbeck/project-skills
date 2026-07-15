# Tasks: Calibration Repair

Source spec: `docs/project-skills/specs/2026-07-10-calibration-repair.md`

Work the frontier: start any task whose blockers are complete.

## Task 1: Create the project-calibration-repair specialist skill

**What to build:** Add `skills/project-calibration-repair/SKILL.md` as the single owner of calibration-based diagnosis and bounded repair for eligible target projects. Define trigger criteria, the explicit applicability gate, authorized-data and leakage preconditions, the calibration-record schema, tiered evidence selection, structured diagnostic feedback, stopping behavior, delegation to Bayesian skills, and explicit limitations.

**Blocked by:** None — can start immediately.

**Acceptance criteria:**

- The skill triggers only when the target project has an explicit probabilistic model and a declared inferential or predictive claim that calibration evidence can assess.
- It does not trigger merely because work is “data science,” and it declines to apply for descriptive analysis, reporting-only notebooks, data engineering, deterministic transformations, or any project without an explicit model.
- It requires a project-specific diagnostic plan, authorized data access, and a leakage-safe evaluation protocol before issuing a verdict.
- It emits a calibration record with `calibrated`, `repair_required`, or `unresolved`; the record contains model revision, intended use, protocol, reproducibility metadata, applicability, evidence, findings, repair history, and limitations.
- It requires model-specific evidence from inference health, prior/posterior predictive checks, and predictive validation where applicable; it neither supplies universal thresholds nor prescribes a likelihood replacement.
- It separates inference-health failures from likely model–data mismatch and treats SBC as feasibility-gated inference evidence, never sufficient evidence of model–data fit.
- It defaults to aggregate diagnostic feedback and requires explicit authorization before using or exposing raw target-project records.
- It performs bounded diagnose → repair → re-check behavior and returns `unresolved` rather than fabricating a pass when evidence is inadequate or the repair budget ends.

## Task 2: Integrate conditional calibration repair into workflow stages

**What to build:** Update `skills/project-spec/SKILL.md`, `skills/project-implement/SKILL.md`, `skills/project-review/SKILL.md`, and `skills/shared/WORKFLOW.md` to invoke `project-calibration-repair` only for eligible probabilistic-model work while preserving each stage’s exclusive ownership.

**Blocked by:** Task 1: Create the project-calibration-repair specialist skill.

**Acceptance criteria:**

- `project-spec` requires a calibration plan only after the applicability gate confirms an explicit probabilistic model and assessable inferential/predictive claim.
- `project-implement` invokes calibration repair/re-check after relevant model changes and does not treat execution, finite samples, or unit tests as sufficient acceptance evidence.
- `project-review` inspects calibration records under separate scientific-validity and Bayesian-diagnostics axes when the gate applies.
- A project that fails the applicability gate remains on the existing stage workflow and creates no project-calibration-repair requirement or artifact.
- Shared workflow guidance identifies the specialist delegation path without reproducing the detailed calibration procedure.
- No stage creates artifacts reserved for a different workflow stage.

## Task 3: Add focused project-calibration-repair evaluation prompts

**What to build:** Extend `evals/evals.json` with realistic project-calibration-repair and integration prompts, expected outcomes, and any evaluation metadata or fixtures required for later grading.

**Blocked by:** Task 2: Integrate conditional calibration repair into workflow stages.

**Acceptance criteria:**

- Evaluation prompts cover over-dispersed or zero-inflated counts, sampler geometry failure, already-calibrated no-op behavior, unsafe or unauthorized data access, and infeasible SBC or inadequate predictive evidence.
- Evaluation prompts include a descriptive/reporting or deterministic transformation project without an explicit probabilistic model and require correct non-trigger behavior.
- Expected outcomes require evidence-backed diagnosis, calibration-record fields, bounded stopping behavior, privacy-safe feedback, and no execution-only acceptance.
- The prompt set distinguishes the specialist workflow from generic Bayesian-checklist advice and does not rely on source-text assertions.
- Integration prompts verify that `project-spec`, `project-implement`, and `project-review` preserve stage ownership while conditionally using the specialist skill.

## Task 4: Run and grade the project-calibration-repair evaluation cycle

**What to build:** Run with-skill and baseline evaluations for the new capability, grade the agreed behavior, aggregate the results, and provide a human-review artifact before accepting the extension. Keep only source prompts and fixtures under version control; exclude generated execution workspaces.

**Blocked by:** Task 3: Add focused project-calibration-repair evaluation prompts.

**Acceptance criteria:**

- The versioned evaluation corpus and fixtures make each scenario reproducible; local run outputs, metadata, timing, and grading evidence are retained for review but excluded from the plugin source tree.
- The results show the skill does not accept a model merely because its code runs and distinguishes inference-health failures from model–data mismatch.
- The results show both correct triggering for an eligible probabilistic model and correct non-triggering for a project without an explicit model.
- The generated review artifact exposes qualitative outputs and quantitative grading for human review.
- Any failing behavior is corrected through the skill-authoring iteration before this task is accepted.

## Task 5: Publish verified project-calibration-repair discoverability

**What to build:** After behavioral evaluation passes, update `README.md` only as needed to make `project-calibration-repair` discoverable and explain its conditional relationship to the existing project stages and Bayesian skills.

**Blocked by:** Task 4: Run and grade the calibration skill evaluation cycle.

**Acceptance criteria:**

- Documentation names `project-calibration-repair`, its eligible use case, and its stage-delegation boundaries.
- Documentation states that it is not a general data-science gate and does not apply to projects without an explicit probabilistic model.
- Documentation does not imply that calibration proves causal correctness, automatically selects models, or grants production approval.
- Documentation links the capability to existing PyMC and model-evaluation guidance without duplicating framework-specific procedures.
