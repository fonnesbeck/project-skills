---
name: show-me
description: Use to explain data science work visually with compact data-lineage diagrams, model sketches, validation timelines, uncertainty tables, notebook maps, and analysis diffs. Use for "show me", "draw the pipeline", or a visual explanation of the current analysis.
---

# Show me

Make the current data-science question easier to understand with the smallest useful visual. Lead with the visual, not a preamble. Prefer one diagram, table, or sketch and a few sentences explaining its consequence; do not turn an explanation into a dashboard or a report.

Inspired by Dex / HumanLayer's [show-me](https://www.humanlayer.com/blog/show-me-skill). This skill applies the compact-visual approach to data, statistical reasoning, and analytical decisions rather than software architecture.

## Boundary

This is an explanation aid, not another workflow stage. It works before a spec exists and alongside any project skill. Preserve the active stage, authority, and stopping point; an illustration neither approves a proposal nor passes a verifier.

Default to a read-only answer in chat. Use the current conversation and inspect only relevant authorized source or existing results. Do not install dependencies, query restricted data, execute notebooks, fit models, change code, or create files merely to illustrate a point. A request for a real computed figure or saved artifact may authorize that work; honor its specific resource and data limits rather than silently expanding it.

When explaining workflow artifacts or saving an authorized explanation, read `skill://project-skills-shared/WORKFLOW.md` for canonical routing and stage boundaries. Do not require workflow records for an ordinary visual explanation. Return to the calling stage without starting a new handoff.

## Choose the question, then the view

Use context to resolve a bare "show me" as a visual restatement of the current topic. Ask a focused question only if there is no identifiable subject or materially different interpretations remain.

| What the reader needs to see | Smallest useful view | Include when relevant |
|---|---|---|
| Where data came from and what changed | Data-lineage flow or transformation tree | Observation unit, join keys/cardinality, filters, aggregation, missingness, known row counts |
| How variables and assumptions form a model | Generative tree, equations, or labeled dependency graph | Observed vs latent quantities, group/index structure, likelihood and target |
| What information is available at prediction time | Split timeline or fold sketch | Forecast origin, horizon, feature/label availability, entity boundaries, fitted preprocessing |
| How evidence supports a result | Compact results/diagnostic table or existing plot | Estimate, units, interval meaning, protocol, missing evidence, claim limits |
| How a notebook reaches its output | Cell-dependency or analysis tree | Inputs, transformations, outputs, reactive dependencies, expensive/stochastic steps |
| What changes between approaches | Focused `diff` or comparison table | Target, population, preprocessing, model, evaluation protocol, changed assumptions |
| How an analytical procedure works | Short pseudocode | Order of operations, what is fitted where, inputs and outputs |
| Where the relevant work lives | Shallow file tree | Only inspected paths and their analytical responsibilities |

Use fenced `text` for terminal-readable sketches and `diff` for before/after changes. Use Mermaid only for actual relationships when the surface supports it; otherwise use text. Tables are better than diagrams for exact values. Never use graphical complexity to hide missing evidence.

## Keep the visual scientifically honest

- Distinguish **observed/reported**, **proposed**, **schematic**, and **unknown** content next to the visual. Cite the source path, artifact/revision, or supplied conversation facts. Reading source establishes intended computation, not successful execution or measured results.
- Use actual labels and values when authorized evidence exists. Never invent row counts, fitted curves, scores, intervals, diagnostic outcomes, or improvements. If results are absent, show the structure and label results unavailable. Clearly labeled synthetic examples teach mechanics, not project performance.
- Preserve the unit of observation, denominator, units, time window, and population when they change the interpretation. Show meaningful exclusions, missingness, aggregation, and join multiplication rather than implying conservation without evidence.
- Distinguish deterministic data flow, probabilistic dependence, and causal assumptions. Label arrow semantics. A dependency diagram is not a causal identification argument; correlation, prediction, and parameter estimates do not establish intervention effects.
- For validation, show what was known at the decision time. Put learned preprocessing inside training folds; respect time ordering, group overlap, and label/publication delays where relevant. Explain an observed flaw without silently rewriting the protocol or declaring a proposed fix successful.
- Name uncertainty precisely: confidence interval, posterior credible interval, predictive interval, or variability across runs. Include the level and target if known. Do not relabel an interval or imply that parameter uncertainty captures outcome variation, selection bias, or all model uncertainty.
- Compare results only within a compatible target, population, split, horizon, units, and metric direction. Show protocol differences and unavailable uncertainty instead of inventing a winner. Sampler health, predictive adequacy, scientific validity, and deployment readiness remain different claims.
- Reference protected datasets through authorized schemas or safe aggregates. Do not request, print, embed, or export sensitive rows or identifiers to make a visual concrete.
- Do not force Bayesian models, calibration checks, or causal diagrams onto descriptive work. Load relevant installed domain guidance for substantive modeling or diagnostic interpretation; this skill does not replace it.

## Compact examples

These examples are schematic teaching shapes, not results from the current project. Select one relevant form; do not reproduce the whole gallery.

### Lineage and observation unit

```text
SCHEMATIC — arrows mean data transformations; counts not measured
appointments [one row / appointment]
  + clinic lookup [one row / clinic; clinic_id join must be many-to-one]
  -> eligible appointments [state exclusions and missing-status policy]
  -> clinic-month counts [one row / clinic / month]
  -> no-show rate = missed / eligible appointments
```

The denominator and exclusions determine the estimand; successful joins alone do not establish correct rates.

### Validation and leakage

```text
PROPOSED — rolling-origin forecast, not an executed evaluation
                         origin t       forecast window
training observations -------|--------------- future ------>
features available by t -----| predict y at t+h
fit preprocessing + model ---| apply unchanged to future inputs
                             | score only once target is observed
repeat at later origins; keep tuning separate from final assessment
```

Availability, not just event date, determines whether a feature can be used. Show actual label delays or entity constraints when supplied rather than inventing a universal split.

### Hierarchical model structure

```text
SCHEMATIC — generative dependence, not a causal graph
population parameters [latent]
  -> group parameters[j] [latent; partial pooling across groups]
     + predictors[i] [observed; observation i belongs to group j]
     -> likelihood for outcome[i] [observed]
posterior draws -> replicated outcomes [predictive, not observed data]
```

Use the actual distributions and dimensions when source is available. This sketch alone says nothing about fit, convergence, or causal effects.

### Analysis change

```diff
 PROPOSED — evaluation change, no performance claim
- fit imputation and scaling on all rows
- randomly split individual rows
+ split by the evaluation unit required by the intended use
+ fit imputation and scaling on training data within each fold
+ apply the fitted transforms to that fold's validation data
```

Name the actual evaluation unit and explain the leakage path once known; do not replace the project's protocol with an arbitrary time or group split.

### Evidence without invented certainty

| Question | Evidence available | What it establishes |
|---|---|---|
| Did computation finish? | Execution record, if supplied | Execution only |
| Is the model adequate for its claim? | Relevant diagnostics and predictive evidence, if supplied | Only the checked properties under that protocol |
| Is an intervention effective? | Identification assumptions and suitable evidence, if supplied | Not established by predictive accuracy alone |

Replace conditional entries with sourced results or explicit unknowns. Do not manufacture pass/fail thresholds.

## Computed figures and saved artifacts

Stay in chat unless the user requests an artifact or the authorized task already requires one. For a requested real plot, use the project's existing plotting stack and applicable installed guidance such as `evident-charts`. Compute marks from authorized data; label axes, units, interval semantics, transformations, and provenance. Render and inspect the actual output before claiming visual verification. If rendering is unavailable, report that limit rather than claiming to have seen it.

Use `marimo-notebook` when creating or editing a requested marimo notebook. Do not introduce HTML, a server, or interactive widgets when a static figure or chat diagram answers the question. Save requested explanatory documents through the established canonical routing; save product figures/notebooks in the project's authorized artifact locations. Do not publish or open external services without authority.

## Delivery

Give the visual, its evidence status/source, and the one or two implications the reader needs. Add an unresolved decision or next action only when material. Stop there; do not append a prose version of the whole diagram or an unrelated implementation plan.
