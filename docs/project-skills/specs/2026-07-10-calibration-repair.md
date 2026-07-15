# Calibration Repair Project Spec

## Problem statement

Users of this plugin need a way to detect and address statistical misspecification in probabilistic models produced or modified during an agent-assisted data-science workflow. Compilation, finite samples, and ordinary code tests do not establish that a likelihood, prior, parameterization, or temporal/group structure can reproduce the observed data or predict unseen observations.

Add a `calibration-repair` skill that treats statistical adequacy as evidence from an explicitly selected Bayesian workflow. It must run a bounded diagnose → repair → re-check loop, return structured findings that guide the next modeling decision, and record unresolved limits instead of treating a runnable model as correct.

This direction is grounded in *Calibration, Not Compilation* (`2606.31630v1.pdf`): posterior predictive checks and held-out predictive density detect data-model mismatch that execution tests cannot; sampler diagnostics identify inference failures; simulation-based calibration (SBC) is expensive and cannot by itself detect model–data misspecification.

## Project class

**Analysis/modeling.** The deliverable is reusable workflow logic that governs diagnostic evidence, model-repair decisions, and review outputs across data-science projects. It is neither an exploratory notebook nor a deployed ML service.

## Users and stakeholders

- **Implementing agent:** receives structured evidence and a bounded next action when a probabilistic model needs diagnosis or repair.
- **Human model owner:** reviews the diagnostic interpretation, any repair, and recorded limitations.
- **Independent project reviewer:** verifies that a reported calibration verdict has sufficient evidence, reproducibility metadata, and no hidden data-governance failure.
- **Existing project skills:** `project-spec`, `implement`, and `project-review` delegate to `calibration-repair` when their stage involves a probabilistic model.

## Data sources

### Target-project data

- **Access path or prerequisite:** supplied by the project using the skill; the user/project must authorize access before calibration begins.
- **Schema or shape:** project-specific model inputs, observed outcomes, group/time identifiers, and the declared evaluation split or cross-validation scheme.
- **Refresh cadence:** project-specific.
- **Privacy, licensing, and governance:** diagnostic feedback contains aggregate evidence, summaries, and references to artifacts—not raw records—unless a project explicitly authorizes raw-data use.
- **Known quality issues:** leakage, data drift, nonrepresentative splits, missingness, censoring, and invalid outcome support can invalidate a calibration conclusion. The target project must document the relevant risks.

### Model revision and fitted-inference artifacts

- **Access path or prerequisite:** candidate model source, declared model target, fitted posterior/inference artifact, posterior predictive draws when applicable, and sampler diagnostics.
- **Schema or shape:** project-specific; a revision identifier and reproducible run metadata are mandatory.
- **Refresh cadence:** created for each calibration attempt.
- **Governance:** preserve references to artifacts and diagnostics rather than duplicating sensitive outputs in repair feedback.
- **Known quality issues:** an approximate or poorly mixed posterior can make sampler failures look like model misspecification; the result must distinguish the two when possible.

### Design reference

- **Source name:** `2606.31630v1.pdf`, *Calibration, Not Compilation: Detecting and Repairing Misspecified Probabilistic Programs Written by Language Models*.
- **Access path:** repository root.
- **Schema or shape:** research paper; not a runtime dataset.
- **Refresh cadence:** static local reference.
- **Governance:** do not copy its benchmark thresholds as universal acceptance criteria. Its results inform the design rationale only.
- **Known quality issues:** its benchmark is low-dimensional, uses a controlled family of model errors, and reports a reference-free detector that is useful but not a universal correctness certificate.

## Target or outcome definition

The outcome is one **calibration record per candidate model revision and evaluation protocol**, with a status of:

- `calibrated`: every applicable, predeclared evidence gate passed;
- `repair_required`: at least one gate failed and the record provides an actionable diagnostic;
- `unresolved`: the repair budget ended, evidence is infeasible or inadequate, or a limitation prevents a defensible verdict.

A `calibrated` status means the model passed its declared workflow evidence. It is not automatic model selection, causal validation, or production approval.

### Calibration-decision model specification

- **Purpose:** determine whether a candidate probabilistic-model revision is adequate for its declared predictive use under a leakage-safe project evaluation protocol.
- **Decision unit:** one tuple `(model revision, authorized project data, evaluation protocol, inference artifact)`.
- **Target quantity:** a structured adequacy verdict and evidence record, not a latent scientific quantity.
- **Latent structure:** none in the calibration skill itself. Latent variables belong to the target model and must be described by the target project.
- **Observed indicators:** sampler diagnostics, model-appropriate prior/posterior predictive discrepancies, and predictive validation results.
- **Likelihoods and priors:** selected by the target project; the skill requires their support and implications to be checked, not a universal likelihood family or prior.
- **Pooling, regularization, and time:** selected by the target project. The diagnostic selection must expose relevant group variation, dispersion, tails, zero inflation, links, transformations, temporal dependence, or latent-state behavior where applicable.
- **Decision rule (pseudocode):**

  ```text
  verdict(model, data, protocol) =
      preconditions_authorized_and_leakage_safe
      AND inference_health_adequate
      AND prior_predictive_implications_plausible
      AND posterior_predictive_checks_pass_for_declared_statistics
      AND predictive_validation_passes_for_declared_decision
  ```

  The project declares the applicable statistics, thresholds, and predictive protocol before the verdict. The skill must not invent universal numerical cutoffs.
- **Output schema:**

  ```text
  calibration_record = {
    model_revision,
    status,
    intended_use,
    data_authorization_and_evaluation_protocol,
    reproducibility: {seeds, sampler_configuration, artifact_references},
    applicability: {prior_predictive, sampler, ppc, predictive_validation, sbc},
    evidence: {diagnostic_definitions, thresholds, results},
    findings: [{failed_behavior, interpretation, supporting_evidence, next_action}],
    repair_history,
    unresolved_limitations
  }
  ```

- **Explicit non-goals:** causal identification, proof of a true generative mechanism, resolution of non-identifiability, and automatic model selection.

## Analysis or modeling approach

### Delegation and stage integration

Create `skills/calibration-repair/SKILL.md` as the single owner of the detailed diagnosis and bounded repair loop. Update:

- `skills/project-spec/SKILL.md` to require a calibration plan when a scoped project uses probabilistic modeling;
- `skills/implement/SKILL.md` to invoke calibration repair after model changes when relevant and prevent execution-only acceptance;
- `skills/project-review/SKILL.md` to inspect calibration records as part of scientific-validity and Bayesian-diagnostics review;
- `skills/shared/WORKFLOW.md` and `README.md` only where the new delegated capability must be discoverable.

The stage skills decide when to invoke the specialist skill. `calibration-repair` owns diagnostic selection, repair feedback, evidence recording, and its stopping rule.

### Applicability gate

Invoke `calibration-repair` only when the target project has an explicit probabilistic model and a declared inferential or predictive claim that calibration evidence can assess. Do not invoke it for descriptive analysis, reporting-only notebooks, data engineering, deterministic transformations, or other projects without an explicit model. The calling stage records why the gate applies; when it does not, its ordinary workflow remains sufficient.

### Bounded calibration-repair loop

1. Confirm authorized access, target outcome, leakage-safe split/cross-validation design, and the decision context before reading or fitting data.
2. Inspect the target model’s likelihood, support, priors, parameterization, and declared predictive target. Require a project-specific calibration plan; do not infer a distributional fix from a generic checklist.
3. Check prior predictive implications where the target model has priors. Invalid support or implausible prior predictions require repair before posterior interpretation.
4. Fit or reuse the inference artifact. Evaluate sampler health using the target framework’s applicable diagnostics: divergences, convergence/mixing, and effective sample size. Diagnose inference geometry separately from model-data mismatch.
5. Run posterior predictive checks using statistics chosen for the target likelihood and decision: for example, location/spread/tails for continuous outcomes, dispersion and zero mass for counts, calibration of predicted probabilities for binary outcomes, or autocorrelation for time series.
6. Run predictive validation using the declared leakage-safe protocol. Prefer held-out predictive performance when a genuine holdout is available; use appropriate pointwise predictive validation such as LOO/LOO-PIT only when its assumptions and diagnostics support it. For PSIS-LOO, record Pareto-$k$ reliability and use an appropriate remedy (such as moment matching or $K$-fold validation) when it is unreliable. Invoke `model-evaluation` for explicit LOO/ELPD comparison or stacking work.
7. Run SBC only when it is feasible and needed to validate inference implementation. Treat it as evidence about inference under the model’s own generative assumptions, not as sufficient evidence of model–data fit.
8. If an applicable gate fails, emit a structured, plain-language finding that identifies the failed observable behavior, distinguishes inference failure from likely specification failure where evidence permits, and proposes the next *kind* of modeling action without prescribing an unvalidated distributional change.
9. Repair one model revision, append its evidence to the calibration record, and re-run all affected evidence gates. Stop when the record is `calibrated`, an explicitly configured repair budget is exhausted, or a documented limitation makes further judgment indefensible.

### Bayesian implementation expectations

The skill must defer framework-specific procedures to the existing Bayesian skills rather than duplicate API guidance. For PyMC projects, this means using the current PyMC/ArviZ workflow: inspect the `xarray.DataTree` sampling result, examine divergences, `r_hat`, and ESS; generate posterior predictive samples; and compute log likelihood explicitly before LOO-based work. The skill must not mandate a sampler, draws, chains, or universal thresholds across target projects.

## Assumptions and risks

- **Authorized, representative data:** calibration conclusions assume the project has lawful access to data and that its evaluation protocol represents the intended decision setting.
- **Leakage:** preprocessing, feature construction, time ordering, group splits, and model selection must be contained within the declared validation procedure. Leakage can make predictive validation falsely reassuring.
- **Diagnostic coverage:** posterior predictive checks only detect discrepancies in the selected statistics. The calibration plan must state why the chosen statistics are sensitive to the decision-relevant failure modes.
- **Inference versus misspecification:** divergences, poor convergence, and low ESS may arise from posterior geometry or approximate inference rather than a wrong likelihood. The record must not conflate these without evidence.
- **Over-wide predictions:** a model can pass coarse PPCs by predicting too broadly. Predictive validation and decision-targeted discrepancies mitigate but do not eliminate this risk.
- **Causality and structure:** predictive adequacy cannot identify causal direction, confounding, or a unique generative mechanism.
- **SBC feasibility:** repeated prior-generated refits can be too expensive for high-dimensional or costly models. Mark SBC as not applicable or infeasible with a rationale rather than silently omitting it.
- **Data limitations:** missingness, censoring, measurement error, domain shift, and small/imbalanced holdouts can prevent a reliable verdict. Record `unresolved` when needed.
- **Repair competence:** a structured diagnostic does not guarantee that an implementing agent can make a valid repair. Human review remains required for substantive model changes.

## Deliverables

- `skills/calibration-repair/SKILL.md`: trigger criteria, preconditions, calibration-record schema, diagnostics/repair workflow, stopping rule, limits, and delegation to relevant Bayesian skills.
- Updated `skills/project-spec/SKILL.md`, `skills/implement/SKILL.md`, and `skills/project-review/SKILL.md`: narrowly scoped delegation hooks and acceptance/review requirements.
- Updated shared workflow/discovery documentation only where required to register the specialist skill and its stage relationship.
- Updated `evals/evals.json` with calibration-repair and integration prompts.
- Skill-evaluation fixtures/results that demonstrate the requested workflow behavior before the extension is accepted.

## Testing and verification

### Skill-evaluation seams

Create realistic, independent evaluation prompts that assess observable behavior rather than source-text inclusion:

1. **Code-invisible count misspecification:** a Poisson model with evidence of over-dispersion or excess zeros. The skill identifies the relevant PPC/predictive discrepancy, requests an appropriate repair class, and re-checks rather than declaring the model valid because it ran.
2. **Inference-geometry failure:** a hierarchical/funnel-like model with divergences or weak mixing. The skill identifies inference health as a blocking issue and avoids falsely claiming that it proved likelihood misspecification.
3. **Already calibrated model:** sufficient evidence passes. The skill records the verdict and does not induce speculative rewriting.
4. **No authorized raw-data access or unsafe validation design:** the skill requests authorization or a leakage-safe protocol and does not expose/solicit raw records by default.
5. **Infeasible SBC or inadequate predictive data:** the skill records applicability and limitations, applies available evidence, and returns `unresolved` instead of fabricating a pass.
6. **Stage integration:** `project-spec`, `implement`, and `project-review` invoke or require `calibration-repair` at the correct stage without handing off stage ownership.

For each evaluation, verify the calibration-record fields, diagnostic interpretation, stopping behavior, privacy boundary, and absence of execution-only acceptance. Use with-skill versus baseline comparisons when evaluating the new skill under the repository’s skill-authoring workflow.

### Additional verification

- Confirm the plugin discovers `calibration-repair` through its configured skill directory.
- Run existing skill-evaluation checks plus new focused evaluations.
- Check that each edited stage skill retains its exclusive workflow ownership and does not duplicate the specialist workflow.
- Review the final diff for unsupported universal thresholds, raw-data disclosure defaults, or a claim that calibration proves causal correctness.

## Out of scope

- Implementing a generic probabilistic-program compiler, model search engine, or autonomous model-selection service.
- Adding a fixed library of likelihood replacements, automatic distribution choice, or a universal diagnostic threshold table.
- Running SBC universally or promising it for high-dimensional/expensive models.
- Treating passing unit tests, finite samples, or a calibration record as production approval.
- Establishing causal validity, recovering a unique data-generating process, or solving non-identifiability.
- Building a user-facing dashboard, production monitoring service, or a domain-specific model in this plugin.
- Copying the referenced paper’s benchmark, code, or performance claims into the plugin as normative acceptance criteria.

## Open questions

None that block task creation. Individual target projects will still need to declare their outcome, likelihood, priors, validation protocol, diagnostic statistics, thresholds, repair budget, and data-governance constraints before `calibration-repair` can issue a verdict.
