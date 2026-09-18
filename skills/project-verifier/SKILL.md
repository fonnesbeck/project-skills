---
name: project-verifier
description: Use before implementation to design evidence-based acceptance criteria, or afterward to check a bounded data science increment against those predeclared criteria without changing its source or evidence.
---

# Project Verifier

Predeclare observable success, then check the identified increment against that contract. Code quality, reproducibility, scientific validity, and operational readiness need distinct evidence—not test-suite success or model agreement alone.

Source lineage: review axes adapted from Matt Pocock's `code-review` for oh-my-pi and data science project development.

## Boundary

Read `skill://project-skills-shared/WORKFLOW.md` first for routing, authority, handoff fields, transitions, and invalidation. Load matching domain skills rather than duplicate API guidance.

- State **design** or **check**, inferred from an unambiguous request/handoff. Ask only if ambiguity changes authorized actions.
- Respect design-only/check-only limits. Neither mode owns source/data changes, environment setup/controls, fitting, or calibration-record production.
- Existing artifacts provide context, not authority.

## Design mode

Read the current bounded spec and its identity/revision, relevant source/data contracts, and authority limits. Return ambiguous requirements to `project-spec`; do not choose a convenient interpretation.

1. Translate acceptance intent and material risks into falsifiable criteria across applicable axes. Include threatening failure/boundary cases, not a test quota.
2. Give each criterion a stable ID and the following fields:

   | Field | Content |
   |---|---|
   | Requirement/axis | Spec outcome or constraint tested |
   | Method | Concrete command, observation, experiment, inspection, or named human judgment; inputs/conditions and, for judgment, rubric/reviewer |
   | Evidence source | Observable output/artifact/reference, bound to revision/run/data/protocol where applicable |
   | Decision rule | Predeclared threshold or qualitative rule, applicability, and failure condition; no invented universal statistical cutoffs |
   | Failure action | Bounded remediation owner/action or stop/escalation; missing evidence prerequisites |

3. Prefer external evidence: real behavior, authorized data checks, fitted diagnostics, held-out results, reproducibility outputs, or inspectable source. Static inspection proves code properties, not unobserved runtime/scientific outcomes. Unavailable tools/data/reviewers are prerequisites, not passes.
4. Name required environment capabilities/permissions without installing or acquiring data. Separate nonmutating verifier checks from implementation-owned evidence-producing runs.
5. Record permitted correction classes, resources, and a finite iteration/stopping bound within existing authority. Zero is valid; missing authority/budget never authorizes repair.
6. Save the design using shared routing/frontmatter, criterion table, prerequisites, bounds, checkpoint, and handoff fields. Bind `verifier_reference` to the **design path + revision**.

Ready means each acceptance intent has an actionable criterion and no blocking prerequisite/interpretation remains. Automatically continue to `project-environment` **readiness** within authority; foundation alone is insufficient. Respect stage-only stops.

Criteria must precede the implementation they accept. Retrospective inspection can report findings, not claim predeclared verification; return through design/readiness before new implementation or evidence collection.

## Check mode

### Establish identity

Read the spec, versioned design, increment readiness, implementation revision, and evidence. Respect the user's target; otherwise resolve a fixed revision or identifiable working-tree/output snapshot. No commit or non-empty diff is required. Ask only if the target remains ambiguous.

Match evidence to the implementation, spec/design, data, fitted run, and protocol as applicable. Missing/stale prerequisites are unavailable, not replaceable with nearby evidence. Follow shared invalidation rules and return to design/readiness when required.

### Evaluate and report

1. Inspect evidence and run only authorized **nonmutating** checks. Check mode writes only its report: never source, data, expected outputs, thresholds, environment/controls, fitted artifacts, or calibration records. Evidence requiring outputs, refitting, or installation returns to implementation/environment.
2. Apply each declared rule exactly. Record criterion ID, axis, result, evidence/observation, reason, and next action:

   | Result | Meaning |
   |---|---|
   | `pass` | Current, sufficient evidence satisfies the rule |
   | `fail` | Observed requirement violation |
   | `unresolved` | Missing, stale, inaccessible, unreliable, contradictory, or insufficient evidence |
   | `not_applicable` | Declared applicability condition is absent; give the reason, never substitute for an unrun/failed check |

3. Keep failures visible: overall **fail** if any required criterion fails; otherwise **unresolved** if any remains unresolved. Accept only when all applicable required criteria pass and exclusions are justified. This establishes neither general correctness nor deployment permission.
4. Report unexpected defects with evidence. Consequential contract changes return to spec/design under shared invalidation rules; preserve the old result and obtain fresh evidence. Never move thresholds, relabel failures, or retry until passing.
5. Save the report using shared routing/frontmatter and a distinct filename-safe revision/run identifier; retain full identity in its body and preserve prior reports. Include shared fields, per-criterion results, axis findings, limitations, and checkpoint decision. Keep the design in `verifier_reference`; put this report in `evidence_references`.

### Continue or stop

- **Accepted:** honor the checkpoint and required human review. Already-authorized further increments may return to spec only when no human decision is pending.
- **Failed/unresolved:** route the evidenced defect or missing prerequisite to its owner. Within recorded authority/bounds, automatically hand remediation to implementation/environment; verifier makes no repairs.
- **Stop:** explicit check-only boundary, exhausted allowance, missing authority, or changed target/data use/claim/scope requiring spec/design. Never hide unresolved limitations behind optimistic acceptance.

## Applicable axes

Consider code quality and reproducibility always; explain code-quality inapplicability when no code changed. Activate other axes only as relevant; assign defects once and cross-reference.

| Axis | Inspect |
|---|---|
| Code quality | Repository standards, maintainability, abstraction/duplication, error handling, observable-behavior tests |
| Reproducibility | Input/artifact references, commands, environment/versions, seeds, run/protocol identities |
| Scientific validity | Target/estimand, observation support, leakage/missingness, evaluation design, metrics/baselines, uncertainty, claim limits, predictive/held-out misfit |
| Bayesian diagnostics | Prior implications, inference configuration, convergence/divergences/ESS, posterior reliability, applicable predictive/comparison evidence; good sampling is not adequate fit |
| Data governance | Permissions, privacy/licensing/retention, derived sensitivity, exposure routes, authorized aggregate/raw-data boundaries; do not request/echo raw records by default |
| Deployment readiness | Operational interfaces, dependencies, failures, monitoring ownership, rollback/retraining; verification never authorizes deployment |

For forecasting, inspect origin/horizon information, instant/period semantics, publication lags, state granularity, and split design. Lack of validation is not demonstrated structural failure. Distinguish inference-health defects (Bayesian diagnostics) from model–data misfit (scientific validity) and missing identities (reproducibility).

### Conditional calibration-record inspection

Require a persisted record only for an explicit probabilistic model **and** an assessable inferential/predictive claim. Otherwise state the absent condition; no calibration statuses or repair loop.

For eligible work, read implementation's record and the `project-calibration-repair` record contract. Check candidate/run, intended use, authority, data/protocol, reproducibility, applicability, predeclared rules, evidence, status, and limitations. Inspect supporting references, not just the label:

- Missing/stale evidence is `unresolved`; demonstrated violations are `fail`.
- Calibration status does not replace criterion decisions; inference failure does not prove likelihood misspecification.
- Check mode never invokes calibration, regenerates/revalidates/recalibrates, or mutates its immutable record. Attribute defects to their axes and return them to implementation, which alone owns fitted evidence and record production.

## Independent critic

An explicit user review/critic request requires actual delegation, not a second self-review. Otherwise use critique only when useful; no mandatory model-consensus ritual.

1. **Delegate:** use OMP `task` with `agent: reviewer` in a fresh context. Supply a self-contained, read-only assignment: fixed target identity, current spec/design revisions, criterion IDs, authorized evidence references, and scope/authority limits. No source/data/control/evidence changes or evidence-producing runs; critic returns findings only.
   Wait for actual findings through runtime notifications or the available `hub` tool—not a shell command. A spawn acknowledgment is not a completed review.
2. **Identify independence:** prefer a different configured model when the runtime supports it. Record actual available agent/session/model identity; mark unknown model identity unknown. Same-model fresh context is **agent-independent**, not cross-model. Do not fabricate model choice/identity or install plugins to obtain it.
3. **Request findings:** counterexamples and unsupported claims, each with criterion ID (or outside-design defect), location, severity, cited evidence, and recommendation—not a vote or confidence score.
4. **Record dispositions:** the parent includes each relevant finding/disagreement in the design/report as accepted, rejected, or unresolved, with evidence/rationale and authorized next action. Resolve through external evidence or the designated human judgment; decision-blocking disagreements remain unresolved.
5. **Handle unavailability:** if delegation is unavailable, report that explicitly and leave required review unresolved. An unavailable different model may still permit agent-independent review, but cannot satisfy an explicit cross-model requirement.

Critic agreement, confidence, or majority consensus is never proof; independent agents can share failure modes.
