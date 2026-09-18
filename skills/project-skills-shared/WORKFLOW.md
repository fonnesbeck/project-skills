# Project Skills Workflow

Inspired by Austin Marchese's [interpretation of Karpathy's advice](https://www.youtube.com/watch?v=7zZy1QTvokM): specify the goal, define proof, and maintain a useful environment. This is not a formal standard or a correctness guarantee.

## Entry and sequence

`project-environment` **foundation** can run before any project spec. It prepares persistent instructions, knowledge, skills, tools, and permissions—not implementation readiness.

For each bounded increment:

1. `project-spec`: clarify the goal, consequential decisions, scope, and checkpoint.
2. `project-verifier` **design**: predeclare acceptance criteria and evidence.
3. `project-environment` **readiness**: check those requirements against the persistent foundation; refresh only affected capabilities.
4. `project-implement`: build the authorized increment and produce evidence.
5. `project-verifier` **check**: assess the identified revision; return bounded repairs or the checkpoint decision.

Reuse the environment for later increments. A small change needs a small spec and verifier, not a mandatory task graph or whole-project plan.

## Authority

- Read the next skill at each handoff. Public stages allow model invocation; discoverability grants no permission.
- Continue mechanical handoffs within existing user authority. Honor explicit stage-only, interview-only, read-only, and named-increment limits.
- Stop for a required human checkpoint, exhausted repair authority, or a genuinely missing prerequisite. Continue independent authorized work when possible.
- User decisions—not document status or an old artifact—authorize consequential choices, new data uses, spending, deployment, and scope expansion.
- Domain skills provide expertise without replacing the active stage or its artifacts.

These are agent instructions, not a runtime-enforced state machine or sandbox.

## Shared handoff

Record applicable fields in artifact bodies; do not invent values for missing prerequisites.

| Field | Meaning |
|---|---|
| `increment_id` | Bounded outcome identifier; omitted in foundation mode |
| `spec_reference`, `spec_revision` | Canonical spec and explicit revision |
| `verifier_reference` | Design path **and revision**, never replaced by the check report |
| `environment_reference` | Persistent record and applicable readiness binding |
| `implementation_revision` | Identifiable source/output snapshot, not a moving branch |
| `evidence_references` | Observations, run artifacts, and check report |
| `status`, `next_action` | Stage result and authorized handoff or exact blocker |

Foundation records omit increment-specific fields and explicitly state that increment readiness is not assessed.

### Invalidation

- A spec change invalidates dependent design, readiness, and acceptance.
- A design change invalidates previous results and affected readiness.
- Changes to source, data, protocol, dependencies, access, or controls invalidate affected evidence.
- Retain historical revision bindings. Justify reuse of unaffected evidence; never relabel an old pass or weaken a criterion after failure.

## Canonical artifacts

Before agent-document access, inspect `~/Documents/Agents/Projects/` and run `agent-docs locate <working-path>` as a lookup. Match the result to existing project identity; do not create duplicates.

Follow `~/Documents/Agents/System/Conventions.md`. Use `ensure --migrate` only after confirming the canonical destination. If lookup is unavailable, use an established destination; ask only if identity remains unresolved.

| Artifact under the canonical project | Type |
|---|---|
| `Plans/YYYY-MM-DD-<increment>-spec.md` | Plan |
| `Docs/YYYY-MM-DD-<increment>-verifier.md` | Reference document |
| `Docs/YYYY-MM-DD-<project>-environment.md` | Persistent reference document; reuse existing file |
| `Docs/YYYY-MM-DD-<increment>-verification-<revision>.md` | Review document; distinct revision/run identifier |

Use vault frontmatter. Plans follow `draft` → `approved` → `in-progress` → `completed`; replacements use `superseded` and `superseded_by`. Documents use `current` or `superseded`. Stage outcomes belong in the body; written code alone does not complete a plan.

Keep raw evidence and calibration records in their declared authorized stores, referenced from documents. Keep sensitive rows and secrets out of workflow documents. Product skill definitions stay in the plugin; reusable skill assets use the established skill location. Do not introduce repo-local workflow config or a duplicate documentation tree.

Reuse existing domain/data notes with provenance and dates. Older artifacts supply context, not authority. A knowledge index points to actual curated material; it is neither the material itself nor model training.

## Scientific and verification boundaries

- Preserve target/estimand, data authority, privacy/licensing, leakage-safe evaluation, uncertainty, reproducibility, and claim limitations.
- Code tests, a rendered notebook, and finite samples do not establish scientific validity. Label synthetic test fixtures; never present simulated evidence as real outcomes.
- Consider code quality and reproducibility; activate scientific, Bayesian, governance, and deployment axes only when applicable.
- Human criteria need an identified reviewer, rubric, and actual disposition. Pending review is unresolved.
- Independent criticism uses the concrete delegation procedure in `project-verifier`. Agreement is not evidence; unresolved material disagreements prevent acceptance.

### Calibration ownership

Only an explicit probabilistic model **and** an assessable inferential/predictive claim trigger `project-calibration-repair`.

| Owner | Responsibility |
|---|---|
| Spec | Claim, constraints, authority |
| Verifier design | Predeclared model-specific checks and decision rules |
| Implementation | Fitted evidence, specialist invocation, immutable candidate records, bounded source repairs |
| Verifier check | Read-only inspection of records and supporting evidence; no specialist invocation or regeneration |

Non-model work has no calibration-plan, record, or repair-loop requirement. Calibration is not model selection, causal proof, or deployment approval.

## Environment safety

Classify actions as always permitted, ask first, or never permitted within actual authority. Prompt rules are advisory. Claim enforcement only for inspected controls and safely observed coverage.

A write/edit hook alone does not block shell writes, symlink aliases, subprocesses, other tools, or remote routes. Do not bypass denials, probe production mutations, or alter system access without authorization. Block only affected unsafe work when a required control is absent.

## Domain guidance

Load installed skills when applicable: `marimo-notebook`/`marimo-pair`; `model-plan-discovery`; `pymc-modeling`; `prior-elicitation`; `model-evaluation`; `pymc-testing`; and the conditional calibration specialist. Do not copy their API instructions. An unavailable optional skill is not automatically a blocker; use established guidance and name capabilities actually missing.

## Attribution

Retained safeguards derive from Matt Pocock's MIT-licensed `mattpocock/skills`. The current sequence is inspired by the linked video.
