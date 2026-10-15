# Evidence supporting Article 1

This is the frozen, article-specific `20261015_supervisor_r2` release for *Tree Ensembles and Tabular
Transformers for Financial Fraud Detection: Imbalance Sensitivity, Controlled
Transfer and Interpretation Boundaries*. It is not a training-output directory.

## Evidence areas

- `metrics/`: selected configurations, saved and full-precision metrics,
  including the common primary grid underpinning the imbalance comparison.
- `controls/`: predefined absence-code and categorical-aware sampling
  comparisons, with paired uncertainty summaries.
- `transfer/`: audited Base-DEV-profile-disjoint Variant cohorts, frozen-model
  performance and source identities; Base thresholds are not selected on Variants.
- `interpretability/`: compatible SHAP feature-set summaries, signed grouping
  and magnitude sensitivity, and the separate final-layer, head-averaged
  FT-Transformer attention diagnostic.
- `tables/` and `figures/`: article-specific reporting assets. Display rounding
  does not imply identical unrounded values or exact ties.
- `provenance/`: release hashes, selected source identities and the relationship
  between corrected runs and preserved historical BAF models.
- Frozen score archives (`.npz`): labels, scores and thresholds for supported
  numerical recomputation, without distributing raw predictor CSVs or model weights.

The dedicated release manifest identifies the files actually provided; it does
not modify the original historical source manifests or turn preserved fits into
new training runs. It also identifies the exact manuscript PDF included in this
release. The reviewed 15 October 2026 revision 2 PDF has 21 pages and SHA-256
`ffd52e00984fea74f4d0d96bb74dbb936233bad2f432efacf715049940b09184`.
The revision preserves fitted models, frozen primary predictions and thresholds;
reporting policies and supplementary diagnostics are versioned separately.

## Revised reporting evidence

- `tables/supervisor/` contains the supplementary manuscript tables. All seven
  planned strategies remain visible; strategy means and population standard
  deviations count six effective conditions because audited SMOTE+Tomek
  duplicates SMOTE. This describes the recorded runs, not algorithmic
  equivalence in general.
- `interpretability/shap_aggregation_logreg.csv`,
  `shap_aggregation_lgbm.csv` and `shap_aggregation_catboost.csv` compare the
  primary original-feature importance `mean_i(abs(sum_j_in_group(phi_ij)))`
  with the historical encoded absolute mass
  `mean_i(sum_j_in_group(abs(phi_ij)))`. Signed sums preserve the additive
  original-feature net contribution; this post-hoc grouping is not a new
  group-Shapley game. Explainer references still prevent comparing absolute
  magnitudes directly between different models.
- `interpretability/shap_magnitude_contrasts.csv` records the complete frozen
  LightGBM feature vectors, ranks and changes relative to Base. `customer_age`
  increases by about 71–72% and moves from rank 17 to 11 on Variants II–V,
  despite unchanged top-10 membership. Jaccard alone cannot show this change.
- `transfer/transfer_overlap_pairs.csv` contrasts filtered and unfiltered
  populations with fitted function and threshold held fixed.
  `transfer/transfer_alert_workload.csv` reports alert rates and counts.
  The filtered cohorts remain primary. FT checkpoint changes are kept
  distinct from population filtering; neither comparison justifies reviving
  an old unfiltered model ordering as the controlled-transfer result.

The threshold transport diagnostic identifies A/validation, A/TEST and B/TEST
separately, with B remaining the primary full-DEV model and no TEST-based
threshold reselection. The additional A/TEST scores come from one inference
pass with preserved A, not a new fit. Prior-adjusted Brier results are float64
diagnostics; a prior adjustment does not constitute validated probability
calibration. A validation precision target does not guarantee TEST precision.

The top-k audit uses `expected_uniform_boundary_v1` to avoid positional
tie breaking at the cut. Only two audited cells change (ULB RF ROS and RF Class
Weights); grouped average precision, threshold decisions and confusion counts
are preserved. Saved historical metrics remain identifiable rather than being
silently overwritten. The paired CatBoost-minus-LightGBM BAF AP contrast
(about +0.0028, 95% interval [-0.0017, +0.0072]) describes conditional TEST-row
uncertainty, not training variability or established superiority.

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

Fitted weights, raw datasets, full validation-score archives, the complete
encoded SHAP matrices and attention
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
