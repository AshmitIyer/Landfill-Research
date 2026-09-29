# Experimental Record

This directory contains the frozen analysis artifacts used for the manuscript.

## Key directories

- `R50-01/` — ResNet-50 training configuration, history, and final locked-test results.
- `calibration_analysis/` — corrected binary calibration summaries and bins.
- `efficiency_benchmark/` — final interleaved paired Tesla T4 efficiency benchmark.
- `error_analysis/` — prediction disagreement, confidence-error, and metadata subgroup analysis.
- `locked_split_seed42/` — train/validation/test manifests and split configuration.
- `statistical_comparison/` — aligned predictions, bootstrap confidence intervals, and exact McNemar test.

## Frozen comparison

The final reported test results use:

- Swin-Tiny threshold: 0.45
- ResNet-50 threshold: 0.50
- Test set: 2,607 samples
- Exact McNemar p-value: 0.251822

Superseded benchmark outputs were removed from the public snapshot so that the repository does not present rejected timing measurements as final results.

The image dataset and model checkpoints are not stored here.
