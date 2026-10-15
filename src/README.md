# Code organisation

The scientific code is retained separately from the private LaTeX projects.

| Area | Entry points / modules |
|---|---|
| Inputs and transformations | `data.py`, `preprocess.py`, `categorical_preprocess.py`, `missing_values.py` |
| Supervised and anomaly baselines | `main.py`, `main_transformer.py`, `models/`, `strategies/`, `evaluation/` |
| Selection and provenance | `experiment_protocol.py`, `ft_run_protocol.py`, `save_load.py` |
| Thresholds and transfer | `threshold_study.py`, `revision_thresholds.py`, `revision_transfer.py`, `cross_domain.py` |
| Compatible explanations | `shap_analysis.py`, `generate_revision_interpretability.py`, `revision_variant_shap.py`, `attention_analysis.py` |
| Prespecified controls and diagnostics | `revision_catboost_control.py`, `revision_paired_analysis.py`, `revision_missing_audit.py`, `revision_sampler_audit.py`, `revision_lgbm_diagnostics.py` |
| Revised frozen-evidence diagnostics | `supervisor_revision_audit.py`, `supervisor_threshold_diagnostics.py`, `supervisor_interpretability_diagnostics.py` |
| Dedicated input/evidence checks | `verify_datasets.py`, `article_release.py` |
| Dedicated figure replay | `article1_figures.py`, `article_interpretability.py` |
| Wider-study reporting modules | `generate_results.py`, `export_public_evidence.py` |

`revision_*` and `run_revision_*` also preserve the provenance checks and
orchestration used for the October correction. These source-specific queues
require the complete local archive and explicit preserved-model pins; they
are not a one-command recipe for reproducing this release from the compact
public export. `revision_thesis_assets.py` is retained because reporting QA
uses its integrity checks; its optional LaTeX-patch mode additionally requires
the private writing project. No private writing sources are shipped here.

The root README gives isolated fresh-training commands. Synthetic checks are
under `tests/` and `src/test_revision_*.py`; they do not retrain the dissertation
grid. Purely editorial PDF review utilities and superseded article plotting
scripts are excluded from the current public tree.

The `20261015_supervisor_r2` reporting policy uses expected credit for ties at
the top-k boundary (`expected_uniform_boundary_v1`) in `evaluation/metrics.py`.
Grouped average precision and frozen threshold decisions are unchanged. The
audit records historical positional values separately; only ULB RF ROS and
RF Class Weights top-k values differ. Reporting code does not infer a new
threshold from TEST labels.

`article_interpretability.py` replays original-feature global SHAP importance
as the mean absolute value of each case's signed group sum. It does not take
the absolute value before grouping. The former encoded absolute-mass summary
is a separately labelled sensitivity, not the primary importance measure.
`supervisor_interpretability_diagnostics.py` computes the six-condition
strategy summaries and SHAP/transfer contrasts from the pinned archive;
`supervisor_threshold_diagnostics.py` implements threshold transport,
fold/precision checks, paired AP and prior-adjusted Brier diagnostics.
These diagnostic entry points need the corresponding original archive
artefacts. Public aggregate tables are not substitutes for fitted models,
raw preprocessing inputs, validation scores or complete attribution arrays.

Historical `generate_revision_interpretability.py` and
`revision_variant_shap.py` preserve the earlier extraction and encoded-mass
reporting provenance. Their legacy summaries are not the primary signed-group
release. The release checker and the revised portable inputs determine the
current manuscript assets; do not overwrite them by running a historical
source-specific queue into `results/`. No model training was repeated for
this revision; the explicitly identified A/TEST diagnostic is one additional
inference pass with the preserved validation model.

Use `article_release.py` for this repository's inventory and frozen-score
recomputation. `export_public_evidence.py` is retained as a shared dependency
and author-side exporter; its wider-thesis verification command expects a
different release manifest and is not the dedicated article checker.
