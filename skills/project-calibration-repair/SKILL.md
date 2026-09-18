---
name: project-calibration-repair
description: >-
  Use when an explicit probabilistic model has a declared inferential or predictive
  claim and needs calibration-based diagnosis or bounded repair. Trigger for
  statistically misspecified Bayesian/probabilistic models, posterior or prior
  predictive checks, sampler-health failures, predictive calibration, or a model
  that runs but may fit data poorly. Do not use for descriptive analysis,
  reporting-only notebooks, data engineering, deterministic transformations, or
  projects without an explicit probabilistic model.
---

# Calibration Repair

Assess whether a candidate probabilistic-model revision has adequate evidence for
its declared inferential or predictive use. Compilation, finite samples, and unit
tests are implementation checks; they do not establish statistical adequacy.

This skill owns calibration-specific diagnosis, repair feedback, evidence
recording, and its bounded stopping rule. It does **not** take ownership of the
calling workflow stage: `project-spec`, `project-verifier`, and `project-implement`
decide when it applies and retain their own artifacts. Spec and verifier design
declare requirements without invoking assessment. Implementation invokes this
skill and persists candidate records; verifier check consumes those records
read-only, without invoking assessment or regenerating them.

## Applicability gate

Apply this skill only if both conditions hold:

1. The project has an **explicit probabilistic model**—a likelihood or other
   probability model linking observed data to uncertain quantities; and
2. The project has a declared **inferential or predictive claim** that can be
   assessed against data.

Do not apply it merely because a project is data science. It cannot be used for
descriptive analysis, reporting-only notebooks, data engineering, deterministic
transformations, or projects without an explicit probabilistic model. Identify
the absent condition and continue the calling stage's ordinary workflow. Do not
create or assign a calibration status, record, evidence-gate plan, diagnostic
requirements, repair budget, or repair loop.

If either condition is unclear after inspecting the project context, ask one
question: “What explicit probability model and inferential or predictive claim
would calibration assess?” Do not infer one from a dataset or code style.

## Required subject-matter guidance

Use the specialist guidance that matches the target model. Do not reproduce its
framework API instructions here.

- Use `pymc-modeling` for PyMC, PyTensor, ArviZ, Bayesian sampling, posterior
  prediction, or sampler diagnostics.
- Use `prior-elicitation` when checking prior implications or revising priors.
- Use `model-evaluation` for LOO, ELPD, stacking, LOO-PIT, or predictive model
  comparison.
- Use `pymc-testing` when writing tests that execute PyMC model code or sampling.

For other probabilistic-programming frameworks, use that framework's equivalent
prior-predictive, sampler-health, posterior-predictive, and predictive-validation
facilities.

## Preconditions

Before evaluating or repairing a model, establish all of the following:

1. **Intended use:** the decision, prediction, or inferential claim the model
   supports.
2. **Candidate revision:** identifiable model source/version and the relevant
   fitted inference artifact, if one exists.
3. **Authorized data access:** the project is permitted to use the relevant data
   for this purpose.
4. **Leakage-safe evaluation protocol:** a declared holdout, time split, group
   split, cross-validation procedure, or other protocol appropriate to the
   intended use. Preprocessing and model selection must be contained within it.
5. **Diagnostic plan:** model- and decision-specific discrepancies, predictive
   metrics, and thresholds or decision rules. Do not invent universal cutoffs.
6. **Reproducibility evidence:** exact random seeds, sampler/inference
   configuration, data/protocol version, model revision, and artifact references
   are available for the current evaluation.
7. **Repair authority and budget:** the calling workflow declares whether a
   revised model may be created and the maximum number of authorized revisions;
   zero is valid when diagnosis without repair is requested.

Do not request or echo raw records by default. Use aggregate diagnostics,
referenceable artifacts, and redacted summaries. If an authorized user explicitly
requires raw-data inspection, state the authorization and minimize exposure.

If any required precondition is absent, return `unresolved`; name the missing
prerequisite and do not issue a calibration verdict or modify a model.

## Calibration evidence

Choose evidence that targets the model's intended use and likely failure modes.
A model need not run every check below; it must document which checks apply,
which are infeasible, and why.

### Prior predictive implications

When the target model has priors, inspect whether simulated outcomes respect
support, units, scale, and scientifically plausible shapes before interpreting a
posterior. Invalid support or implausible prior predictions require repair before
posterior conclusions.

### Inference health

Inspect applicable sampler/inference diagnostics such as divergences,
convergence/mixing, and effective sample size. Treat evidence of poor geometry or
an unreliable posterior approximation as an **inference-health failure**. Do not
claim that it proves a wrong likelihood, prior, or data-generating structure.

### Posterior predictive checks

Select statistics that could expose the decision-relevant mismatch. Examples:

- continuous outcomes: location, spread, tails, skew, extrema, or residual shape;
- counts: dispersion, zero mass, tails, and rate variation;
- binary/probability outcomes: calibration of predicted probabilities and relevant
  class-conditional behavior;
- grouped models: between-group variation and partial-pooling implications;
- time series: autocorrelation, temporal residual structure, latent-state behavior,
  or volatility;
- survival/censored outcomes: support, censoring pattern, and tail behavior.

Explain why each statistic is relevant. A PPC only detects discrepancies it is
sensitive to; matching a few moments is not a general pass.

### Predictive validation

Use the declared leakage-safe holdout protocol when genuine held-out observations
are available. When pointwise validation such as PSIS-LOO or LOO-PIT is suitable,
use `model-evaluation`, record its reliability diagnostics, and follow the
appropriate remedy when importance sampling is unreliable. Do not use predictive
fit as proof of a causal mechanism or unique generative structure.

### Simulation-based calibration

Use SBC only when repeated prior-generated refits are feasible and when
validation of the inference implementation is needed. SBC assesses inference
under the model's own generative assumptions; it cannot by itself detect a model
that is internally coherent but mismatched to real data. Mark it `not_applicable`
or `infeasible` with a rationale rather than silently omitting it.

## Calibration record

Create one immutable record per tuple of candidate model revision, fitted inference
run, authorized project data, and evaluation protocol. The calling stage persists
the record in the project artifact location declared by the calibration plan and
reports its `calibration_record_reference`; preserve raw outputs there and put
only references and aggregate evidence in the record.

```text
calibration_record = {
  model_revision,
  inference_run_id,
  calibration_record_reference,
  intended_use,
  status: calibrated | repair_required | unresolved,
  data_and_protocol: {
    authorization,
    split_or_validation_design,
    data_version_or_artifact_reference
  },
  reproducibility: {
    random_seeds,
    inference_configuration,
    artifact_references
  },
  applicability: {
    prior_predictive,
    inference_health,
    posterior_predictive,
    predictive_validation,
    sbc
  },
  evidence: {
    diagnostic_definitions,
    decision_rules_or_thresholds,
    aggregate_results
  },
  findings: [
    { failed_behavior, interpretation, supporting_evidence, next_action }
  ],
  repair_history,
  unresolved_limitations
}
```

Use exactly one of these statuses:

- **`calibrated`:** every applicable, predeclared evidence gate passed. This means
  the revision passed its declared workflow evidence; it is not automatic model
  selection, production approval, causal validation, or proof of a true
  data-generating process.
- **`repair_required`:** inference health is adequate but at least one
  model–data or predictive evidence gate failed. The record identifies the
  observable behavior, supporting evidence, and the kind of modeling decision
  that needs reconsideration.
- **`unresolved`:** evidence is missing, infeasible, unreliable, contradicted by
  an unresolved inference failure, or the repair budget ended without a
  defensible verdict.

## Diagnose → repair → re-check

Use a bounded assessment loop. The calling workflow declares repair authority and
the repair budget before the first evaluation; do not invent an unlimited retry
policy. This skill never mutates model source itself.

1. Evaluate the current revision against every applicable, predeclared evidence
   gate and update its calibration record.
2. If all gates pass, return `calibrated` without speculative rewriting.
3. If evidence is inadequate or an inference-health failure prevents a reliable
   conclusion, return `unresolved` with the specific prerequisite, limitation, or
   inference issue. Do not disguise it as a likelihood failure.
4. If model–data or predictive evidence fails while inference health is adequate,
   return `repair_required` with plain-language feedback naming the misfit—for
   example, under-captured count dispersion, excess zeros, a posterior predictive
   tail mismatch, or omitted temporal dependence.
5. Propose the next **class of modeling action**—revisit likelihood family,
   observation support, prior implications, parameterization, group/time
   structure, validation protocol, or inference method—without asserting an
   unvalidated distributional fix.
6. Return control and the `calibration_record_reference` to the calling stage. If
   it has authority and remaining budget, it may create exactly one new candidate
   revision and invoke this skill again; append the rationale and the new
   evaluation evidence to `repair_history`.
7. Stop at `calibrated`, `unresolved`, or the declared repair budget. Never
   replace an unresolved result with “the code runs” or “all unit tests pass.”

## Output format

For an applicable model, report concisely:

```markdown
Calibration record
- Revision: <identifier>
- Intended use: <claim or decision>
- Status: calibrated | repair_required | unresolved
- Applicability: <which evidence gates ran, were skipped, or were infeasible>
- Reproducibility: <seeds, inference configuration, protocol, artifact references>

Findings
- <failed behavior or passed evidence>: <interpretation>; evidence: <aggregate result>; next action: <action class or None>

Limitations
- <limitation or None>
```

For a project that fails the applicability gate, state:

```markdown
Calibration repair: not applicable
Reason: <no explicit probabilistic model or no assessable inferential/predictive claim>
Continue with: <the calling stage's ordinary workflow>
```

## Limits

Calibration evidence is necessary for trustworthy probabilistic-model work but is
not a universal correctness certificate. Do not claim that it:

- establishes causal direction, resolves confounding, or proves a unique
  generative mechanism;
- resolves non-identifiability or guarantees reliable inference for every hard
  model class;
- replaces project-specific data governance, substantive model review, or human
  approval;
- automatically selects the best model or authorizes deployment.
