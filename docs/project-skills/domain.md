# Domain context

## Project direction

- This repository extends an Oh My Pi-native plugin for developing data-science projects from initial scoping through review.
- **Project class:** analysis/modeling. The proposed extension adds reusable statistical-diagnostic workflow requirements rather than a research notebook or production ML product.

## Calibration workflow boundary

- For probabilistic models, the extension will require a bounded **diagnose → repair → re-check** loop when calibration evidence fails.
- The loop accepts a model only after the required evidence passes, or records an unresolved limitation; successful execution is not an acceptance criterion.

## Diagnostic consumers

- Calibration findings must be actionable for the implementing agent and intelligible to the human model owner and independent reviewer.
- Findings identify the observed misfit and relevant evidence, rather than returning an opaque pass/fail status.

## Extension boundary

- Add a dedicated `project-calibration-repair` skill for calibration-based diagnosis and bounded repair of projects with an explicit probabilistic model and a declared inferential or predictive claim.
- `project-spec`, `project-implement`, and `project-review` will delegate to that skill only when this applicability gate is met; projects without an explicit model retain their ordinary stage workflows.

## Explicit non-goals

- The extension does not automatically accept models, select a model, or perform unbounded autonomous rewriting.
- Predictive calibration does not establish causal or generative correctness, resolve non-identifiability, or guarantee reliable inference for computationally difficult models.
- When the evidence is inadequate, the workflow records an unresolved limitation rather than issuing a pass verdict.
