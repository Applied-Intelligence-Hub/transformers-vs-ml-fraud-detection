# Evidence supporting Article 1

This is a frozen, article-specific release for *Tree Ensembles and Tabular
Transformers for Financial Fraud Detection: Imbalance Sensitivity, Controlled
Transfer and Interpretation Boundaries*. It is not a training-output directory.

## Evidence areas

- `metrics/`: selected configurations, saved and full-precision metrics,
  including the common primary grid underpinning the imbalance comparison.
- `controls/`: predefined absence-code and categorical-aware sampling
  comparisons, with paired uncertainty summaries.
- `transfer/`: audited Base-DEV-profile-disjoint Variant cohorts, frozen-model
  performance and source identities; Base thresholds are not selected on Variants.
- `interpretability/`: compatible SHAP feature-set summaries and the separate
  final-layer, head-averaged FT-Transformer attention diagnostic.
- `tables/` and `figures/`: article-specific reporting assets. Display rounding
  does not imply identical unrounded values or exact ties.
- `provenance/`: release hashes, selected source identities and the relationship
  between corrected runs and preserved historical BAF models.
- Frozen score archives (`.npz`): labels, scores and thresholds for supported
  numerical recomputation, without distributing raw predictor CSVs or model weights.

The dedicated release manifest identifies the files actually provided; it does
not modify the original historical source manifests or turn preserved fits into
new training runs. It also identifies the exact manuscript PDF included in this
release. The reviewed 8 October 2026 PDF includes the dedicated-repository
links; this documentary update does not change the scientific evidence.

## What can be reproduced here

From the repository root:

```powershell
python src/article_release.py --verify
python src/article_release.py --recompute --output-dir runs/recomputed
```

The first command verifies the released evidence. The second computes metrics
from frozen scores, including the 70 supervised primary cells and two OCSVM
references; it writes only to a separate output directory. This distinguishes
checking saved predictions from independent refitting. Control and transfer
summaries retain their stated paired/bootstrap procedures and source identities;
do not infer a new interval from rounded table endpoints.

Fitted weights, raw datasets, the complete encoded SHAP matrices and attention
from every query, head and layer are not distributed. Plot-ready inputs retain
all 200,000 observations for the displayed beeswarm and final-layer, head-averaged
CLS attention, plus the six selected case matrices. Consequently this payload cannot recreate
all archived predictions, complete attribution extraction or historical timing
measurements. Those require the source-specific fitted artefacts, compatible
software and original execution conditions. Fresh training follows the root
README and is an independent rerun, not guaranteed bitwise replay.

Regenerate all seven article figures in a new directory:

```powershell
python src/article1_figures.py --output-dir runs/article1_figures
```

This replays the displayed saved evidence, not new attribution or model
inference. Rendering-library versions can change output bytes. The beeswarm
residual colour follows the original SHAP display policy documented in its
plot-input manifest; it is not an aggregate feature-value interpretation.

## Interpretation safeguards

Variant filtering establishes exact common-predictor separation from Base DEV,
not entity independence or separation between every evaluated TEST cohort.
SHAP top-10 agreement concerns membership, not rank order, equal magnitudes or
causality. Attention is a diagnostic, not output attribution. Conditional row
bootstrap intervals omit refitting and seed variability; overlapping intervals
or a paired interval containing zero do not establish equivalence.

Never overwrite this tree with a new fit or reporting run. Use an ignored
directory such as `runs/reproduction/` and an explicit fresh run manifest.
