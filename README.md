# Risk-Aware Adaptive Cascade for Landfill-Candidate Classification in Aerial Imagery

Research repository for a reproducible, risk-aware inference pipeline for image-level landfill-candidate screening from aerial imagery. The current research direction, **RACE (Risk-Aware Adaptive Cascade)**, uses frozen ImageNet-pretrained ResNet-50 and Swin-Tiny backbones and adds confidence-based selective escalation and human referral.

## Current research direction: RACE-01

RACE treats the task as **landfill-candidate vs. non-candidate** screening. ResNet-50 is used as a faster first-stage model. When its confidence is below a threshold, Swin-Tiny is invoked. After escalation, an image is accepted automatically only when the two models agree and Swin-Tiny confidence is at least the same threshold; otherwise the case is referred for human review.

The threshold is selected **using validation data only** and is frozen before the held-out test set is evaluated.

### Primary held-out result

For RACE-01, the threshold selected on validation was **tau = 0.90**. The 2,607-image test set was then evaluated without changing the policy.

| Metric | RACE-01 |
|---|---:|
| Test images | 2,607 |
| Automatic coverage | **88.80%** |
| Selective accuracy | **96.41%** |
| Referral rate | **11.20%** |
| Automatic candidate-FN rate | **4.14%** |
| Swin-Tiny invocation rate | **19.26%** |
| Expected model-only latency | **3.94 ms/image** |
| Estimated compute saving vs. always running both | **48.79%** |

At the primary operating point, **2,315/2,607** test images were resolved automatically and **292/2,607** were referred. Swin-Tiny was invoked on **502/2,607** images.

The threshold-selection source was validation only; the test labels were not used to select tau.

### RACE operating frontier

The pre-specified sensitivity grid shows the coverage-risk-compute trade-off:

| tau | Coverage | Selective accuracy | Auto candidate-FN | Swin invoked |
|---:|---:|---:|---:|---:|
| 0.70 | 96.51% | 93.92% | 8.17% | 6.71% |
| 0.75 | 95.13% | 94.15% | 7.36% | 8.78% |
| 0.80 | 93.67% | 94.80% | 6.44% | 11.74% |
| 0.85 | 91.37% | 95.68% | 5.06% | 15.30% |
| **0.90** | **88.80%** | **96.41%** | **4.14%** | **19.26%** |
| 0.95 | 84.12% | 97.54% | 2.30% | 26.12% |

The **0.90 point is the primary result**, selected before test evaluation. Other thresholds are sensitivity analysis only.

## Frozen backbone baselines

RACE builds on a controlled comparison of the same frozen backbones:

| Metric | Swin-Tiny | ResNet-50 |
|---|---:|---:|
| Accuracy | 92.60% | 91.94% |
| Precision | 86.74% | **87.74%** |
| Recall | **91.83%** | 88.15% |
| F1-score | **89.21%** | 87.94% |
| ROC-AUC | **97.84%** | 97.13% |

The exact paired McNemar test gave **p = 0.2518**, so the backbone comparison is reported descriptively rather than as statistically established overall architectural superiority.

### Inference efficiency

Measured on a Tesla T4 with 224×224 input and batch size 16:

| Metric | Swin-Tiny | ResNet-50 |
|---|---:|---:|
| Parameters | 27.52M | 23.51M |
| Mean latency/image | 4.651 +/- 0.106 ms | 3.047 +/- 0.030 ms |
| Mean throughput | 215.1 +/- 4.8 img/s | 328.3 +/- 3.2 img/s |

RACE uses this measured efficiency difference to place ResNet-50 first and selectively escalate to Swin-Tiny.

## Experimental protocol

Locked split, seed 42:

- Train: 7,276
- Validation: 1,820
- Test: 2,607
- Working AerialWaste v3.0 release: 11,703 images
- Original test release kept untouched for final evaluation

The repository contains experiment metadata and analysis outputs, but **not the AerialWaste image dataset or model checkpoints**.

## RACE-01 reproducibility artifacts

The finalized RACE experiment is maintained on the [race-x-holdout branch](../../tree/race-x-holdout).

Key artifacts:

- [Frozen validation policy](../../blob/race-x-holdout/experiments/RACE-01/validation/race_policy_config.json)
- [Primary held-out result bundle](../../blob/race-x-holdout/experiments/RACE-01/test/race_primary_result_bundle.json)
- [Held-out operating points](../../blob/race-x-holdout/experiments/RACE-01/test/race_test_operating_points.csv)
- [Policy comparison](../../blob/race-x-holdout/experiments/RACE-01/test/race_policy_comparison.csv)
- [Bootstrap 95% confidence intervals](../../blob/race-x-holdout/experiments/RACE-01/test/race_primary_bootstrap_ci.csv)
- [Risk-coverage figure](../../blob/race-x-holdout/experiments/RACE-01/figures/race_risk_coverage.png)
- [Compute-behavior figure](../../blob/race-x-holdout/experiments/RACE-01/figures/race_compute_behavior.png)

The exact experiment protocol is documented in [experiments/RACE-01/README.md](../../blob/race-x-holdout/experiments/RACE-01/README.md).

## Repository contents

### 1. Experimental results and analysis

- experiments/R50-01/ — ResNet-50 configuration, training history, predictions, and metrics.
- experiments/locked_split_seed42/ — locked train/validation/test CSV splits and split configuration.
- experiments/statistical_comparison/ — aligned predictions, bootstrap confidence intervals, and paired McNemar analysis.
- experiments/calibration_analysis/ — calibration summary and reliability-bin data for both models.
- experiments/efficiency_benchmark/ — paired Tesla T4 inference benchmark.
- experiments/error_analysis/ — structured disagreement and error analysis.
- experiments/RACE-01/ — finalized adaptive-cascade validation/test artifacts, currently on the race-x-holdout branch.

### 2. Results and reproducibility documentation

- results/RESULTS.md — summary of the frozen backbone findings.
- analysis/REPRODUCIBILITY.md — reproducibility and methodological notes.
- experiments/README.md — description of the frozen experimental record.
- security/SECURITY.md — public-repository security notes.
- CITATION.cff — citation metadata.

### 3. Manuscript source

- paper/latex/main.tex — current manuscript LaTeX source.
- paper/latex/references.bib — bibliography source.
- paper/latex/tables/ — manuscript table sources.
- paper/latex/figures/ — manuscript figures.
- paper/final/ — current manuscript PDF.

The manuscript package is being revised to reflect the RACE contribution and the RACE-01 held-out evaluation.

## Dataset provenance

The working **AerialWaste v3.0** release used in this project contains 11,703 images. The original AerialWaste dataset paper (Torres & Fraternali, *Scientific Data*, 2023) reports 10,434 images for the release described in that publication.

This repository does not silently treat those releases as identical. The exact dataset/version provenance used for each experiment should be taken from the corresponding experiment metadata and project documentation.

## Scientific scope and evidence boundary

This is an **image-level landfill-candidate screening** task. A positive prediction does not establish that a location is an illegal landfill or that a legal violation has occurred. The intended interpretation is prioritization for subsequent human inspection.

RACE-01 empirically evaluates the core adaptive cascade on the locked AerialWaste test population. Broader RACE-X extensions such as OOD detection, conformal uncertainty, metadata-aware routing, active learning, geographic/sensor external validation, and formal human-subject evaluation are **not claimed as experimentally validated by RACE-01**; they remain proposed or future extensions unless separately documented.

The current study does not establish cross-geographic, cross-sensor, or cross-domain generalization.

## Research status

**Current status: RACE-01 experimental results frozen and backed up; manuscript revision in progress.**

The original Swin-Tiny vs. ResNet-50 comparison remains a frozen baseline. The current research contribution is the risk-aware adaptive inference policy built on those frozen backbones.

Future repository updates are expected to focus on manuscript, supplementary material, citation metadata, and release/version information unless a new experiment is explicitly documented as a separate research version.

## Reproducibility

See:

- [Results summary](results/RESULTS.md)
- [Reproducibility notes](analysis/REPRODUCIBILITY.md)
- [Canonical project handoff](project_handoff/AerialWaste_Landfill_Research_Master_Handoff.docx)
- [Current manuscript source](paper/latex/main.tex)
- [RACE-01 holdout branch](../../tree/race-x-holdout)

The dataset, large model artifacts, archives, credentials, and secrets are intentionally excluded.

## Citation

A provisional citation record is provided in [CITATION.cff](CITATION.cff). Update publication metadata after acceptance/publication.

## License and reuse

No blanket license is granted for the manuscript text, figures, or research results in this repository. Dataset rights remain with their respective owners/providers. Contact the authors before redistributing modified manuscript or figure material.

## Authors

**Ashmit Iyer**, **Sarthak Sharma**  
Kalinga Institute of Industrial Technology (KIIT), Bhubaneswar, India
