# Dataset inputs for Article 1

Raw CSVs are not distributed. Obtain the original files from their providers,
preserving their contents and following their access, attribution and licence terms.

| Input | Provider | Local filename |
|---|---|---|
| ULB credit-card transactions | [ULB on Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud) | `creditcard_2013.csv` (rename the downloaded `creditcard.csv`) |
| BAF suite | [Feedzai documentation](https://github.com/feedzai/bank-account-fraud), [Kaggle download](https://www.kaggle.com/datasets/sgpjesus/bank-account-fraud-dataset-neurips-2022) | `Base.csv`, `Variant I.csv` through `Variant V.csv` |

Place the decompressed inputs in this directory. [manifest.json](manifest.json)
records the exact byte sizes and SHA-256 hashes used in the study. Changes in
line endings, numeric formatting or column order change those hashes.

```powershell
python src/verify_datasets.py
```

Raw data are necessary for fresh training, not for verifying the release,
recomputing metrics from frozen predictions or replaying the distributed
plot-ready explanation data. Dataset terms remain those of the providers;
the repository code licence does not grant a new dataset licence.

ULB distinct-profile deduplication and BAF common-feature filtering are pipeline
operations, not edits to these inputs. Transfer excludes exact Variant profiles
matching Base DEV, omits `x1`/`x2` in Variants III/V and pools months; it is not
entity-level separation or prospective temporal evaluation.
