# Data and evidence context

## Calibration evidence policy

- Probabilistic-model acceptance uses a tiered calibration gate: sampler diagnostics, model-specific posterior predictive checks, and predictive validation.
- Simulation-based calibration is optional and feasibility-gated. It validates inference under the model’s own generative assumptions but cannot, alone, establish model–data fit.
- Diagnostic thresholds and test statistics must be appropriate to the likelihood, data structure, and decision target; the extension must not apply universal fixed thresholds copied from a benchmark.

## Reproducibility evidence

- Every calibration verdict records the data split or evaluation protocol, random seeds, model revision, sampler configuration, diagnostic definitions and thresholds, and raw or referenceable results.

## Project-data safeguards

- Calibration repair requires authorized project-data access and a documented leakage-safe evaluation design.
- Repair feedback contains diagnostic evidence rather than raw records by default; any use of raw data requires an explicit project-specific authorization.
