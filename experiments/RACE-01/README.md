# RACE-01 Holdout Experiment

**RACE (Risk-Aware Adaptive Cascade)** evaluates a selective inference policy built on the frozen AerialWaste Swin-Tiny and ResNet-50 backbones.

## Core policy

ResNet-50 is Stage 1 because it has lower measured inference latency on the Tesla T4.

1. Run ResNet-50 on every image.
2. If ResNet-50 confidence is below the threshold tau, invoke Swin-Tiny.
3. On escalated images, automatically accept the prediction only when the two models agree and Swin-Tiny confidence is at least tau.
4. Otherwise refer the image for human review.

The threshold is selected from validation data only and is frozen before evaluating the held-out test set.

## Experimental protocol

### Stage A — validation-only policy selection

The locked validation split contains 1,820 images.

The fixed candidate threshold grid was:

{0.70, 0.75, 0.80, 0.85, 0.90, 0.95}

Selection rule:

> Select the highest-coverage threshold subject to automatic candidate-FN rate <= 5% on validation.

This selected and froze:

**tau = 0.90**

The resulting policy configuration is stored in race_policy_config.json.

### Stage B — held-out evaluation

The locked test split contains **2,607 images**:

- 869 candidate images
- 1,738 non-candidate images

The test set was loaded only after the validation policy was frozen. No test labels were used for threshold selection, and the frozen backbone checkpoints were not overwritten.

## Primary held-out RACE result

At the frozen **tau = 0.90** operating point:

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

Counts at the primary operating point:

- Automatically accepted: **2,315 / 2,607**
- Referred: **292 / 2,607**
- Swin-Tiny invoked: **502 / 2,607**
- Automatic candidate false negatives: **36 / 869**

## Sensitivity analysis

The pre-specified threshold grid gives the following held-out operating frontier:

| tau | Coverage | Selective accuracy | Auto candidate-FN | Swin invoked | Expected latency |
|---:|---:|---:|---:|---:|---:|
| 0.70 | 96.51% | 93.92% | 8.17% | 6.71% | 3.36 ms |
| 0.75 | 95.13% | 94.15% | 7.36% | 8.78% | 3.46 ms |
| 0.80 | 93.67% | 94.80% | 6.44% | 11.74% | 3.59 ms |
| 0.85 | 91.37% | 95.68% | 5.06% | 15.30% | 3.76 ms |
| **0.90** | **88.80%** | **96.41%** | **4.14%** | **19.26%** | **3.94 ms** |
| 0.95 | 84.12% | 97.54% | 2.30% | 26.12% | 4.26 ms |

The **0.90 operating point is the primary result**. The remaining thresholds are sensitivity analysis and are not used to retune the primary policy.

## Reference-policy comparison

RACE is compared with fixed reference policies evaluated on the same held-out test predictions:

| Policy | Coverage | Selective accuracy | Auto candidate-FN | Expected latency |
|---|---:|---:|---:|---:|
| ResNet-50 only | 100.00% | 91.94% | 11.85% | 3.05 ms |
| Swin-Tiny only | 100.00% | 92.60% | 8.17% | 4.65 ms |
| R50-only selective | 80.74% | 97.15% | 3.34% | 3.05 ms |
| Parallel agreement gate | 75.53% | 98.48% | 1.15% | 7.70 ms |
| **RACE core** | **88.80%** | **96.41%** | **4.14%** | **3.94 ms** |

The purpose of RACE is not to maximize any single metric. It targets a practical coverage-risk-compute trade-off with automatic referral of unresolved cases.

## Uncertainty quantification

The primary RACE result includes **2,000 bootstrap resamples**.

95% bootstrap confidence intervals:

| Metric | Estimate | 95% CI |
|---|---:|---:|
| Coverage | 88.80% | 87.53–89.99% |
| Selective accuracy | 96.41% | 95.66–97.13% |
| Auto candidate-FN rate | 4.14% | 2.89–5.53% |
| Referral rate | 11.20% | 10.01–12.47% |
| Swin invocation rate | 19.26% | 17.72–20.79% |
| Expected model-only latency | 3.94 ms | 3.87–4.01 ms |

## Reproducibility artifacts

- validation/race_policy_config.json — frozen validation-selected tau and selection rule.
- validation/race_validation_model_predictions.csv — validation predictions used for policy selection.
- validation/race_validation_operating_points.csv — validation operating grid.
- test/race_test_model_predictions.csv — aligned frozen-model predictions for all 2,607 test images.
- test/race_primary_result_bundle.json — primary held-out result and bootstrap information.
- test/race_primary_results.json — primary metric summary.
- test/race_test_operating_points.csv — held-out operating frontier.
- test/race_policy_comparison.csv — reference-policy comparison.
- test/race_primary_bootstrap_ci.csv — primary 95% bootstrap confidence intervals.
- test/race_primary_bootstrap_samples.csv — bootstrap samples.
- tables/race_operating_points.tex — LaTeX table source.
- figures/race_risk_coverage.png — risk/coverage figure.
- figures/race_compute_behavior.png — compute/latency figure.

## Evidence boundary

RACE-01 empirically evaluates the **core RACE cascade** on one locked AerialWaste test population.

Broader RACE-X extensions — including OOD detection, conformal uncertainty, metadata-aware routing, active learning, geographic/sensor external validation, and formal human-subject evaluation — are **not experimentally validated here** and should be treated as proposed or future extensions unless separately documented.

The image-level task is landfill-candidate screening. A positive classification does not establish that a site is an illegal landfill or that a legal violation has occurred; the intended use is prioritization for human inspection.

## Safety and reproducibility constraints

- Do not tune tau from test labels.
- Do not overwrite the frozen backbone checkpoints.
- Do not modify the locked train/validation/test split.
- Keep the original test release untouched.
- Keep the dataset and large checkpoint files out of the repository.
