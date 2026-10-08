# Tree Ensembles and Tabular Transformers for Financial Fraud Detection

*Imbalance Sensitivity, Controlled Transfer and Interpretation Boundaries*

Dedicated research repository for **Article 1**, by Olavo Caixeiro, Maryam
Abbasi and Pedro Miguel de Oliveira Martins. The manuscript is unpublished
and in preparation for submission; no submission, acceptance or publication
status is asserted. This is an empirical research study, not a production
fraud-detection system.

[Read the manuscript](publications/article_1.pdf) · [Inspect the evidence](results/)
· [Explore the code](src/) · [Obtain the datasets](datasets/README.md)

This article uses the shared experimental foundation of the
[wider dissertation study](https://github.com/Olavo200100274/fraud-imbalance-classical-vs-transformers).
The [companion Article 2 repository](https://github.com/Applied-Intelligence-Hub/tabular-ml-dl-threshold-benchmark)
focuses on decision thresholds and alert workload. Shared code and evidence
are intentionally retained in both dedicated repositories; they are not
independent replications of one another.

## Article scope and findings

Five supervised families (LR, RF, LightGBM, CatBoost and a custom FT-Transformer)
and an OCSVM reference are studied on ULB credit-card transactions and the BAF
bank-account application suite. PR-AUC denotes average precision (AP).

- **Imbalance sensitivity:** primary BAF SMOTE reduces FT-Transformer AP from
  0.1769 to 0.1149 (about 35%) and CatBoost from 0.1796 to 0.1543 (about 14%).
  Different categorical representations prevent an architecture-only causal
  interpretation. The predefined FT SMOTE-NC control reaches 0.1157, not the
  baseline; its paired interval does not establish equivalence to primary SMOTE.
- **Controlled transfer:** Base-trained models, preprocessing and thresholds
  are frozen. After removal of exact common-predictor matches to Base DEV,
  LightGBM leads Variant I AP and CatBoost leads II–V by point estimate.
  All supervised models have lower Variant F2 than on Base at frozen thresholds.
- **Interpretation boundaries:** compatible LR/LGBM/CatBoost SHAP comparisons
  concern original-feature sets, not causality or equal rankings. The fixed
  LGBM retains its top-10 set across Base and filtered Variants, while orders
  and attribution magnitudes can change.
- **Separate attention diagnostic:** mean normalised final-layer,
  head-averaged FT-Transformer CLS attention entropy is 0.985. Diffuse
  attention is not faithful output attribution or an information ceiling.

The ULB split follows exact predictor-profile deduplication: 283,726 profiles,
including 473 frauds, with 56,746 TEST rows and 95 frauds. BAF Base TEST has
200,000 rows and 2,206 frauds; filtered Variant cohorts have 177,526–178,828 rows.
BAF months are pooled and transfer uses 30 common predictors, excluding the
extra `x1`/`x2` in Variants III/V. Exact-profile separation is not proof of
entity-level independence, mutual Variant independence or prospective transfer.

Baseline hyperparameters are reused across seven primary strategies.
Classical models use five-fold inner validation and FT-Transformer one internal
holdout. Corrected ULB searches and preserved BAF classical models have distinct
provenance. Single outer splits and conditional row-bootstrap intervals do not
measure training-seed variability; overlapping intervals do not prove equivalence.

## Contents

```text
datasets/       Official-source access instructions and exact raw-file hashes
notebooks/      Relevant exploratory analysis of ULB, BAF and BAF Variants
src/            Shared scientific pipeline and dedicated evidence checker
tests/          Synthetic protocol and integrity checks
results/        Metrics, controls, transfer, interpretation, assets and provenance
publications/   Article 1 manuscript PDF
```

The [evidence README](results/README.md) distinguishes published aggregates and
frozen score arrays from artefacts not distributed here. No raw dataset CSVs,
fitted model weights, complete encoded SHAP matrices or attention from every
query, head and layer are included. Plot-ready inputs preserve all 200,000
observations behind the displayed beeswarm and final-layer CLS diagnostic,
plus the six selected case matrices.
The shared source is retained to preserve dependencies; specialised correction
queues still require the author's full archive and are not a generic recipe.

The included PDF is the latest reviewed manuscript before the dedicated-repository
link update. Its scientific results are unchanged. An Overleaf-recompiled PDF
with the new Introduction and availability links will replace it; the release
manifest identifies the exact PDF version and hash.

## Environment and data

Use Python 3.12. The recorded correction environment was Python 3.12.14,
Windows 11, i5-13600KF, 16 GB RAM and RTX 3070 (8 GB); these are recorded
conditions, not certified minimum specifications.

```powershell
git clone https://github.com/Applied-Intelligence-Hub/transformers-vs-ml-fraud-detection.git
cd transformers-vs-ml-fraud-detection
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
$env:PYTHONUTF8 = "1"
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m pip check
```

Requirements pin the CUDA 12.1 PyTorch 2.5.1 build. For CPU-only use, instead
install the other pinned packages and the CPU wheel:

```powershell
$articlePackages = Get-Content requirements.txt | Where-Object { $_.Trim() -and $_ -notmatch '^(--|torch==)' }
python -m pip install $articlePackages
python -m pip install torch==2.5.1 --index-url https://download.pytorch.org/whl/cpu
```

Choose one installation route. Fresh installation and execution on another
computer have not been validated; hardware changes can affect fitted outputs.
Obtain the seven original CSVs following [datasets/README.md](datasets/README.md),
retain their byte contents and check them with `python src/verify_datasets.py`.
Dataset download is not needed to inspect or recompute the released scores.

## Reproducibility levels

### 1. Inspect and verify released evidence

```powershell
python src/article_release.py --verify
```

This checks released file identities and evidence consistency without training.
It does not independently reproduce the fitting process.

### 2. Recompute metrics from frozen scores

```powershell
python src/article_release.py --recompute --output-dir runs/recomputed
```

Frozen labels, scores and thresholds support recomputation of the 70 supervised
primary cells and two OCSVM references. See the evidence README for the scope
of the control and transfer payload. Recomputing metrics is not refitting models,
rerunning SHAP/attention extraction or reproducing historical hardware timings.

Regenerate the seven manuscript figures from the saved numerical and
plot-ready evidence, without fitting or model inference:

```powershell
python src/article1_figures.py --output-dir runs/article1_figures
```

Rendering-library versions can change output bytes; the figure inputs and
their interpretation limits are documented in the evidence README.

### 3. Run fresh experiments

Always use new output directories and an explicit manifest; never train into
the frozen `results/` tree. A small pipeline check is not an article result:

```powershell
python src/main.py --dataset ulb --models logreg --strategy none --sample 0.05 --n_trials 1 --bootstrap-iterations 0 --results-root runs/smoke --run-manifest runs/smoke/revision_manifest.json
```

For full reruns, fit the classical baselines with `src/main.py` and the
transformer baselines with `src/main_transformer.py` first. Reuse that fresh
manifest for the six non-baseline interventions (`--strategy all`).
Both support `--dataset ulb` and `--dataset baf_base`; consult `--help` for
the complete options. Classical `--models all` includes OCSVM, not FT-Transformer.

Frozen-model transfer uses `src/revision_transfer.py`; compatible explanations
and diagnostics are implemented in `src/generate_revision_interpretability.py`,
`src/revision_variant_shap.py`, `src/shap_analysis.py` and `src/attention_analysis.py`.
These stages require correctly pinned fitted models, partitions and preprocessing,
not only exported summaries. Follow [src/README.md](src/README.md), and do not
launch archive-specific queues against an unconfigured public checkout.
Fresh searches are independent reruns, not bitwise replay of preserved BAF fits.

Synthetic checks, without full-data retraining:

```powershell
python -m unittest discover -s tests -p "test_*.py"
```

## Citation and rights

> Caixeiro, O., Abbasi, M., and Martins, P. M. O. (2026). *Tree Ensembles and
> Tabular Transformers for Financial Fraud Detection: Imbalance Sensitivity,
> Controlled Transfer and Interpretation Boundaries*. Unpublished manuscript.

Identify this repository and the commit used when citing code or results.
Project code is distributed under [GPL-3.0](LICENSE), subject to applicable
third-party notices. This does not relicense provider datasets, the manuscript,
institutional assets or third-party material; their respective rights remain
with their owners. Cite original datasets and methods separately.
