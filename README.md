# Addressing Class Imbalance in LSTM-Based Early Hypoglycemia Prediction Using Continuous Glucose Monitoring Data

Code and per-fold results for the paper accepted at the IEEE BIBM 2026
Undergraduate and High School Symposium (UGHS).

One LSTM architecture, four imbalance-correction variants (none, class
weights, SMOTE, SMOTE + tuned threshold), plus the camera-ready additions
requested by the reviewers: Model A' (uncorrected LSTM with a tuned
threshold), Model A read on its ROC curve at the class-weighted model's
false-positive rate, and shallow baselines (class-weighted logistic
regression, logistic regression with a tuned threshold, HistGradientBoosting)
on the same windows and folds.

## Contents

- `hyper.ipynb`: the complete pipeline with its outputs, as run on Kaggle
  (Tesla T4, PyTorch 2.10). Data loading, patient screening, time-aware
  windowing, 6-fold leave-one-patient-out cross-validation on OhioT1DM,
  window- and episode-level metrics, statistical tests, figures, and the
  per-fold results printed after each fold. The last cells print the full
  comparison table and a drift check against the numbers in the paper.

## Data

- Training: HUPA-UCM, DiaData (HUPA-UCM rows removed via its `Database`
  column), AZT1D, T1D-UOM. All are public under CC BY 4.0 at the sources cited
  in the paper.
- Test: OhioT1DM (6 patients, 2018 release). OhioT1DM is available from Ohio
  University under a Data Use Agreement and **cannot be redistributed here**.

The notebook expects the datasets as Kaggle inputs under `/kaggle/input/` and
locates files by glob, so any dataset name works as long as the files are
attached.

## Protocol

60-minute glucose history, 30-minute prediction horizon, hypoglycemia defined
as glucose < 70 mg/dL. Each fold uses one OhioT1DM patient as test, one as
validation (threshold tuning and early stopping), and the remaining four plus
all 82 external patients for fitting. Normalisation and SMOTE are fit on the
training split only. Seed 42 throughout; the camera-ready re-run reproduced
every number in the original run.
