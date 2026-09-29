# Frozen Experimental Results

## Test-set comparison

| Metric | Swin-Tiny | ResNet-50 |
|---|---:|---:|
| Accuracy | 0.9260 | 0.9194 |
| Precision | 0.8674 | 0.8774 |
| Recall | 0.9183 | 0.8815 |
| F1 | 0.8921 | 0.8794 |
| ROC-AUC | 0.9784 | 0.9713 |

Confusion matrices:

- Swin-Tiny: `[[1616, 122], [71, 798]]`
- ResNet-50: `[[1631, 107], [103, 766]]`

## Paired statistical test

Exact McNemar test:

- discordant correctness table: `[[2308, 106], [89, 104]]`
- statistic: 89
- p-value: 0.251822

The manuscript therefore avoids describing the point-estimate differences as statistically established architectural superiority.

## Bootstrap 95% confidence intervals

The complete confidence-interval table is stored in:

`experiments/statistical_comparison/bootstrap_95CI_results.csv`

## Calibration

Corrected binary calibration analysis:

| Metric | Swin-Tiny | ResNet-50 |
|---|---:|---:|
| ECE | 0.0286 | 0.0293 |
| Brier score | 0.0553 | 0.0609 |
| Calibration slope | 0.6849 | 0.7148 |
| Calibration intercept | -0.2845 | 0.1285 |

These are reported descriptively; no significance test for the calibration differences is claimed.

## Efficiency

Interleaved paired Tesla T4 benchmark:

| Metric | Swin-Tiny | ResNet-50 |
|---|---:|---:|
| Mean latency/image | 4.6509 ms | 3.0465 ms |
| Mean throughput | 215.12 img/s | 328.27 img/s |
| Parameters | 27,520,892 | 23,512,130 |

## Error analysis

Test set: 2,607 samples.

- Swin-Tiny errors: 193
- ResNet-50 errors: 210
- Both correct: 2,308
- Swin correct / ResNet wrong: 106
- ResNet correct / Swin wrong: 89
- Both wrong: 104
- Model disagreement: 195 samples

For the candidate-location subgroup:

| True candidate location | Swin-Tiny | ResNet-50 |
|---|---:|---:|
| Errors | 71 / 869 | 103 / 869 |
| Error rate | 8.17% | 11.85% |

Subgroup analyses are descriptive and should not be generalized beyond this test population.
