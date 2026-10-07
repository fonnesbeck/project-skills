# Project Skills

An oh-my-pi-native plugin for data-science work using **spec → verifier →
environment → implement**, in small, reviewable increments, with compact visual
explanations through `show-me` and explicitly invoked retrospectives through
`project-retro`.

The video supplies three conceptual layers. This plugin provides a persistent
environment foundation and an incremental spec → verifier → implementation loop.
It does not promise a speedup or treat model agreement as proof.

## Sequential workflow

Before any increment, `project-environment` **foundation** can prepare the
workspace without a spec or verifier. Foundation preparation is not permission
or readiness to implement a product.

| Order | Skill | Outcome |
|---|---|---|
| 1 | `project-spec` | The actual goal, explicit key decisions, and one bounded increment with a review checkpoint |
| 2 | `project-verifier` — design | Predeclared acceptance criteria, evidence sources, checks, and failure actions |
| 3 | `project-environment` — readiness | Check the current spec/design against the reusable foundation; refresh only affected capabilities |
| 4 | `project-implement` | The authorized increment, real evidence, and a `project-verifier` check against that evidence |

Review the result at the agreed checkpoint, adjust the next specification, and
repeat. Reuse the environment while it remains suitable. Implementation may
repair failures only within the authorized scope and budget; it cannot relax
acceptance criteria to make a failing result pass.

### Invocation

For a new workspace, foundation setup can come first:

```text
/skill:project-environment
Prepare the persistent foundation. No increment exists yet. Reuse our canonical
instructions, curate authorized knowledge, and inventory skills, tools and
permissions. Stop before specifying or implementing a product.
```

Start with a goal, not just a deliverable:

```text
/skill:project-spec
Help me determine what clinic managers need to decide from our monthly
no-show report. Interview me about unresolved decisions, then specify the
smallest useful increment. Stop before implementation.
```

Once that increment is agreed:

```text
/skill:project-verifier
Design the verifier for the agreed spec. Define the evidence and decision
rules before any implementation changes.
```

```text
/skill:project-environment
Check readiness for that spec and verifier against the existing foundation.
Reuse knowledge and tooling. Distinguish advisory rules from enforced controls.
```

```text
/skill:project-implement
Implement the authorized increment using its verifier and environment
record, then run project-verifier in check mode. Stop at the agreed checkpoint.
```

Provide artifact references or use those already in the conversation.
All four workflow-stage skills allow model invocation, so an authorized full increment
can proceed without repeated approval at mechanical handoffs. Explicit stage
limits, readiness requirements, and human checkpoints still apply. Skill
instructions are not a runtime-enforced state machine or permission system.

## Sample workflow: a monthly no-show report

Suppose a project has an authorized clinic-month aggregate CSV and an existing
marimo notebook. Clinic managers want to identify months that warrant closer
investigation, not predict individual attendance. This example describes proposed
work, not an executed analysis; use the actual input and notebook references from
your project.

### 1. Agree on one useful increment

```text
/skill:project-spec
Help clinic managers compare monthly no-show rates using our authorized
clinic-month aggregates. Inspect the existing notebook and data documentation,
then interview me about the decision, denominator, exclusions, and missing data.
Scope one descriptive increment: counts, rates, and a clear account of limitations.
No patient-level data, predictive model, or causal claims. Stop after the spec.
```

Resolve the questions before approving the spec. For this example, suppose the
agreed input contains `clinic_id`, `month`, `eligible`, and `missed`; cancelled
appointments are already excluded. Rates use `missed / eligible`, and a zero
denominator must display as unavailable, not as a zero rate.

An optional visual check can expose misunderstandings before implementation:

```text
/skill:show-me
Sketch how the agreed aggregate inputs become counts and rates in the notebook.
Show the denominator and zero-eligible case. Keep it schematic and in chat;
do not execute or change the analysis.
```

### 2. Authorize the bounded implementation

```text
I approve this descriptive increment. Use the saved spec and proceed through
verifier design, environment readiness, implementation, and verifier check.
Prepare or refresh the environment foundation only as needed. Reuse the existing
project environment; ask before any dependency installation or new data access.
Change only the agreed notebook and its supporting validation code. I authorize
one bounded repair attempt under the unchanged acceptance criteria.
Stop at the verification checkpoint, or ask if scope or criteria need to change.
```

Within that authority, the skills proceed without repeated approval at each
mechanical handoff:

| Stage | Concrete work in this example |
|---|---|
| `project-verifier` design | Predeclare source-total reconciliation, count validity, rate and zero-denominator behavior, notebook execution, and a human review rubric for interpretability. |
| `project-environment` readiness | Check the current spec/design against the existing environment, authorized CSV access, notebook tooling, and permitted changes. |
| `project-implement` | Update the agreed surface, exercise the declared cases, execute and inspect the notebook, and preserve evidence for the identified revision. |
| `project-verifier` check | Assess that evidence against the unchanged criteria; distinguish failures, missing evidence, and pending human review from passes. |

The spec belongs in the canonical vault's `Plans/`; the verifier design,
environment record, and verification report belong in `Docs/`. Notebook/code
changes stay in the repository, and data and execution evidence stay in their
authorized stores. No calibration record is needed for this descriptive work.
A successful notebook run alone does not satisfy the interpretability review;
the designated reviewer must record a disposition.

### 3. Review the working process separately

After the checkpoint, explicitly invoke the optional retrospective:

```text
/skill:project-retro
Review this reporting increment and directly related sessions. Identify observed
workflow or scientific-practice problems, distinguishing already-fixed issues.
Save the review in the canonical vault and ask which proposed fixes to implement.
Do not treat this request as approval to change instructions or rerun the analysis.
```

Select only the proposals you want applied. The retrospective updates its report
with their dispositions and verification; a new analytical goal starts another
bounded increment rather than extending this one implicitly.

## Visual explanations

`show-me` is an optional explanation aid, not another workflow stage. Use it
before a spec exists or during any stage to see the current question as a compact
visual rather than a long prose answer.

```text
/skill:show-me
Show how our appointment data become clinic-month no-show rates. Include the
observation unit, join cardinality, exclusions, and denominator.
```

```text
/skill:show-me
Show where our forecasting split leaks future information, and sketch the
proposed correction without running or changing the analysis.
```

With an established topic, `/skill:show-me` alone asks for a visual restatement.
The skill chooses a data-lineage flow, model sketch, validation timeline,
uncertainty or comparison table, notebook-dependency map, or focused analysis
diff. It distinguishes reported results from proposed or schematic content,
preserves interval meanings, and does not turn predictive associations into
causal claims.

The default is read-only output in chat: no fitting, notebook execution, file
creation, or workflow handoff. Requested computed figures use authorized data
and the existing plotting stack; missing evidence stays explicit rather than
becoming invented scores, curves, or error bars.

## Retrospectives

`project-retro` is an explicitly invoked companion, not another required stage.
It reviews agent workflow and scientific practice using the current session and
directly related same-project history. Findings require observed problems, not
generic missing-tooling concerns. Inspection does not execute analyses or fixes.

```text
/skill:project-retro
Review this increment and related sessions for workflow and scientific-practice
problems. Save the findings, then ask which fixes I want implemented.
```

By default it saves an evidence-linked review in the canonical vault's
`Docs/YYYY-MM-DD-<topic>-retro.md` and summarizes it in chat. A named session/range
narrows the review; an explicit chat-only request suppresses the saved report.
Unavailable history is a coverage limit, never an invented session assessment.
Historical transcripts and recaps remain unchanged; corrections belong in the
review, and proposed fixes must address future behavior.

Select proposed finding IDs to authorize fixes. Instruction edits require explicit
approval of their proposed wording and destination. Changes to scientific claims,
protocols, models, or product behavior use the existing spec/verifier/environment/
implementation boundaries; selecting a fix does not waive those prerequisites.
The report records selections, actual verification, and any blocked work.
Vault identity and report conventions come from the shared workflow. For
startup-loaded instruction edits, a successful mid-session replay is reported as
“verified in-session only”; normal startup behavior requires a fresh-session replay.

## What verification means

A verifier is designed **before** implementation and applied **afterward** to an
identifiable revision. Each criterion has a method, evidence source, decision
rule, and failure action. Missing or stale evidence is unresolved, not passed.
Subjective requirements identify a human reviewer and rubric rather than pretend
to be machine-checkable.

For an explicit independent-review request, `project-verifier` delegates to an
OMP `reviewer` task in a fresh context. The critic receives the fixed target,
criteria, authorized evidence, and read-only limits. The parent records findings
and evidence-backed dispositions; it does not simply count votes.

A different configured model is preferred when available. A fresh session using
the same model is agent-independent, not cross-model review. An unavailable
required critic remains unresolved; no plugin is installed automatically.

Use actual commands, data checks, rendered outputs, service observations, or
human judgments to establish results. Spec or verifier changes invalidate
dependent readiness and acceptance; historical evidence retains its revision.

Code quality and reproducibility remain distinct from scientific validity,
Bayesian diagnostics, data governance, and deployment readiness. Passing code
tests alone does not establish a scientific conclusion or production readiness.

## Persistent environment

`project-environment` has two modes:

- **Foundation:** prepare persistent instructions, knowledge, skills, tooling,
  and permission facts without requiring an increment.
- **Readiness:** verify capabilities for the current spec/design, keeping that
  revision binding separate from foundation status in the same environment record.

When authorized, foundation preparation adds missing verification-before-build
guidance to the existing instruction source, curates material into established
knowledge stores, and creates a narrow skill for an observed recurring workflow.
New skills must be loaded and replayed, not merely written. The knowledge index
points to real material; neither the index nor those files train the model.

External feedback checks inspect actual observations. For deployment, connectivity,
running revision, and application behavior are separate questions; HTTP 200 alone
is not proof of a successful deployment.

Actions are classified as **always permitted**, **ask first**, or **never
permitted**, within the user's authority. Prompt instructions are advisory.
Hard restrictions require real controls and checks covering the relevant access
routes. A hook on write/edit tools alone does not prevent shell or alternate-tool
writes. Unsupported controls are reported, not fabricated.

## Artifacts

Workflow documents use the canonical project in the **Agents Obsidian vault**,
following its identity, filename, frontmatter, and status conventions:

- `Plans/`: increment specifications.
- `Docs/`: verifier designs, persistent environment records, per-revision
  verification reports, and retrospective reviews.
- Declared authorized artifact stores: calibration records and raw evidence,
  referenced rather than duplicated in workflow documents.

The shared workflow resolves project identity with `agent-docs ensure <repo-path>`,
reusing a successful resolution for this repository in the current session.
Resolution may reuse, register, or create the canonical vault project; it does
not create repository-local workflow configuration or a second documentation tree.
Existing project notes are reused, and prior workflow artifacts can supply context
without authorizing new work. Skill definitions and evaluation fixtures themselves
are plugin product assets and remain in this repository.

## Probabilistic-model calibration

`project-calibration-repair` remains a conditional specialist for an **explicit
probabilistic model with an assessable inferential or predictive claim**.
Descriptive analysis, reporting-only notebooks, data engineering, and deterministic
transformations do not acquire artificial calibration requirements.

- The spec records the claim and constraints.
- Verifier design declares model-specific checks and decision rules.
- Implementation gathers fitted evidence, invokes the specialist, and persists
  each candidate's calibration record under finite repair authority.
- Verifier check inspects that record read-only and reports applicable findings;
  it does not regenerate the record or mutate the model.

Framework-specific guidance stays in applicable skills such as `pymc-modeling`,
`prior-elicitation`, `model-evaluation`, and `pymc-testing`. Calibration is not
model selection, proof of causal correctness, or deployment approval.

## Install

From the repository root:

```sh
omp plugin link .
```

Restart OMP to reload discovery, then verify:

```sh
omp plugin list
```

OMP discovers skills from `skills/<name>/SKILL.md` using `package.json`.
`.claude-plugin/plugin.json` also lists the same eight skill directories: four
workflow stages, calibration, `show-me`, `project-retro`, and hidden shared support.

## Version 0.2 cutover

This replaces the former setup → scoping → specification → task graph →
implementation → review pipeline:

- Goal discovery is part of `project-spec`.
- Workspace setup is part of `project-environment`.
- Review responsibilities move to `project-verifier` check mode, with evaluation
  criteria established in design mode before work begins.
- Implementation works from the bounded spec and verifier, not a separate
  approved task graph.

The retired entrypoints are removed rather than retained as aliases. Existing
user documents are not deleted or automatically migrated. Restart installed
sessions before invoking the new skills.

## Evaluations

`evals/evals.json` contains behavioral scenarios for the workflow, calibration
specialist, `show-me`, and `project-retro`. Scenarios are evaluation inputs and
expected behaviors, not proof that a model passed them. Retrospective cases cover
primary-evidence limits, archived instructions, selected-fix authority, scientific
routing, canonical-identity failures, in-session versus fresh-session instruction
verification, and a valid no-findings result. The visual-explanation cases cover
descriptive lineage, temporal leakage, incompatible model comparisons, uncertainty
and causal limits, and context-only notebook changes. `evals/fixtures/` holds
supplied evidence for integration scenarios. Run skills against the scenarios in
an isolated session; report actual observed behavior separately from
structural/discovery checks.

## Attribution

The previous implementation was derived from Matt Pocock's `mattpocock/skills`
repository under the MIT License, adapting setup, discovery, specification,
ticketing, implementation, and review skills for data science and oh-my-pi.
Retained safeguards build on that lineage. The current workflow is inspired by
the linked spec/verifier/environment video.

`show-me` is inspired by Dex / HumanLayer's
[compact visual explanation skill](https://www.humanlayer.com/blog/show-me-skill),
with original data-science guidance and examples for this plugin.

`project-retro` adapts Matt Pocock's
[`retro` skill](https://github.com/mattpocock/skills/tree/main/skills/engineering/retro)
for this workflow. Its upstream MIT copyright and permission notice are retained
in [`skills/project-retro/LICENSE`](skills/project-retro/LICENSE).
