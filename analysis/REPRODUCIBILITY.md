# Reproducibility Notes

## Experimental design

The final comparison uses the same locked train/validation/test partition for Swin-Tiny and ResNet-50.

Split seed: **42**

| Split | Images |
|---|---:|
| Train | 7,276 |
| Validation | 1,820 |
| Test | 2,607 |

The original test release was kept untouched for final evaluation.

## Models

- Swin-Tiny: ImageNet-pretrained `swin_tiny_patch4_window7_224`
- ResNet-50: ImageNet-pretrained ResNet-50
- Input resolution: 224x224

## Evaluation

The final test metrics are computed once on the locked test set at the validation-selected decision thresholds:

- Swin-Tiny threshold: 0.45
- ResNet-50 threshold: 0.50

Reported analyses include accuracy, precision, recall, F1, ROC-AUC, bootstrap 95% confidence intervals, exact paired McNemar testing, calibration, inference efficiency, and metadata-based error analysis.

## Statistical comparison

The prediction files are aligned by sample identifier and ground-truth agreement is checked before paired testing.

The exact McNemar result is:

- statistic: 89
- p-value: 0.251822

This does not support a claim of statistically established overall superiority of one architecture over the other.

## Efficiency benchmark

The publication efficiency result uses an interleaved paired benchmark on a Tesla T4 GPU, with the same 224x224 tensor input and batch size of 16. Data loading and preprocessing are excluded from the reported GPU inference timing.

## Reproducibility limitations

The working AerialWaste v3.0 release and the release described by the original 2023 dataset paper have different reported image counts. Exact dataset provenance/version should therefore be stated when reproducing the study.

The full 11,703-image decode audit was interrupted before completion; this repository does not claim that a complete decode audit passed.

No cross-domain validation was performed.

## Repository scope

Large dataset files and model checkpoints are not included in this public repository. The experiment outputs under `experiments/` are the frozen analysis record.
