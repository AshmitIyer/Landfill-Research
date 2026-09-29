# A Reproducible Comparative Evaluation of Swin-Tiny and ResNet-50 for Landfill-Candidate Classification in Aerial Imagery

Research repository for a controlled comparison of ImageNet-pretrained Swin-Tiny and ResNet-50 on the AerialWaste landfill-candidate classification task.

## Study

The study evaluates binary image-level classification of **landfill candidates vs. non-candidates** from aerial imagery. The experimental protocol uses a locked train/validation/test split and reports predictive performance, bootstrap confidence intervals, paired McNemar testing, calibration, inference efficiency, and structured metadata-based error analysis.

### Frozen test-set results

| Metric | Swin-Tiny | ResNet-50 |
|---|---:|---:|
| Accuracy | 92.60% | 91.94% |
| Precision | 86.74% | **87.74%** |
| Recall | **91.83%** | 88.15% |
| F1-score | **89.21%** | 87.94% |
| ROC-AUC | **97.84%** | 97.13% |

The exact paired McNemar test gave **p = 0.2518**. The study therefore reports the differences descriptively rather than claiming statistically established overall architectural superiority.

### Inference efficiency

Tesla T4, 224×224 input, batch size 16, interleaved paired benchmark:

| Metric | Swin-Tiny | ResNet-50 |
|---|---:|---:|
| Parameters | 27.52M | 23.51M |
| Mean latency/image | 4.651 ± 0.106 ms | 3.047 ± 0.030 ms |
| Mean throughput | 215.1 ± 4.8 img/s | 328.3 ± 3.2 img/s |

## Experimental protocol

Locked split, seed 42:

- Train: 7,276
- Validation: 1,820
- Test: 2,607
- Working AerialWaste v3.0 release: 11,703 images
- Original test release kept untouched for final evaluation

The repository contains experiment metadata and analysis outputs, but **not the AerialWaste image dataset or model checkpoints**.

## Repository contents

### 1. Experimental results and analysis

- `experiments/` — frozen experimental results and reproducibility artifacts.
  - `R50-01/` — ResNet-50 configuration, training history, test predictions, metrics, and confusion matrix.
  - `locked_split_seed42/` — locked train/validation/test CSV splits and split configuration.
  - `statistical_comparison/` — aligned predictions, bootstrap 95% confidence intervals, and paired McNemar analysis.
  - `calibration_analysis/` — final calibration summary and reliability-bin data for both models.
  - `efficiency_benchmark/` — final interleaved paired Tesla T4 inference benchmark.
  - `error_analysis/` — disagreement analysis, error groups, class-wise error rates, metadata-based subgroup analysis, high-confidence errors, and qualitative-example metadata.
  - `PROJECT_STATE_BEFORE_FULL_TRAINING.json` — project-state record from the controlled full-training stage.

### 2. Results and reproducibility documentation

- `results/RESULTS.md` — human-readable summary of the frozen experimental findings.
- `analysis/REPRODUCIBILITY.md` — reproducibility and methodological notes.
- `experiments/README.md` — description of the frozen experiment artifacts and which outputs were superseded/removed.
- `security/SECURITY.md` — public-repository security notes.
- `CITATION.cff` — provisional citation metadata.

### 3. Manuscript source

- `paper/latex/main.tex` — current manuscript LaTeX source.
- `paper/latex/references.bib` — editable bibliography source.
- `paper/latex/tables/` — manuscript table source files.
- `paper/latex/figures/` — final manuscript figures.
- `paper/final/` — current manuscript PDF.
- `paper/BINARY_ARTIFACTS_CHECKLIST.md` — record of the manuscript binary package.

### 4. Project documentation

- `project_handoff/AerialWaste_Landfill_Research_Master_Handoff.docx` — canonical project handoff containing the detailed project history, experiment state, constraints, and continuation notes.

## Current manuscript package

The current manuscript package has now been uploaded to the repository.

### Current draft paper

`paper/final/A_Reproducible_Comparative_Evaluation_of_Swin_Tiny_and_ResNet_50_for_Landfill_Candidate_Classification_in_Aerial_Imagery.pdf`

This is the **current research manuscript/draft version** as of the September 29, 2026 project update. It should not be treated as the final published version.

### Current manuscript figures

The following final figures are included under `paper/latex/figures/`:

1. `Fig1_Experimental_Workflow_Final.png`
2. `Fig2_Performance_Confusion_Final.png`
3. `Fig3_Performance_Efficiency_Final.png`
4. `Fig4_Error_Analysis_Final.png`
5. `Fig5_Bootstrap_CI_Final.png`
6. `Fig6_Representative_TP_TN_FP_FN_AerialWaste.png`

The manuscript source, bibliography, tables, current PDF, and figure package are therefore represented in the repository together.

## Important dataset provenance note

The working **AerialWaste v3.0** release used in this project contains 11,703 images. The original AerialWaste dataset paper (Torres & Fraternali, *Scientific Data*, 2023) reports 10,434 images for the release described in that publication.

This repository does not silently treat those releases as identical. The manuscript and project handoff should be consulted for the exact provenance/version information used for the experiments.

## Scientific scope

This is an **image-level landfill-candidate screening** task. A positive prediction does not establish that a location is an illegal landfill or that a legal violation has occurred. The intended interpretation is prioritization for subsequent human inspection.

The study is limited to the evaluated AerialWaste release and does not establish cross-geographic, cross-sensor, or cross-domain generalization.

## What will be updated later

The repository is intentionally being maintained as a **research record that evolves with the publication process**. The experimental results are frozen; later updates are expected to be publication and release metadata rather than new experiments.

When the timing is appropriate, the following may be uploaded or updated:

1. **Accepted/final manuscript**
   - Replace the current draft PDF with the accepted or officially published manuscript version, as permitted by the venue.
   - Keep the manuscript source synchronized with the final paper where appropriate.

2. **Publication metadata**
   - Update `CITATION.cff` with the final publication information and DOI.
   - Update the README citation section once the paper has an official bibliographic record.

3. **Final supplementary material**
   - Add supplementary figures, tables, appendices, or additional reproducibility material if the target venue permits or requires public release.

4. **Release/version information**
   - Update the repository with the final paper/release version and any permanent archival identifier if one is created.
   - If an official dataset/release citation or provenance document becomes available, link to it rather than copying the dataset into this repository.

5. **Publication-specific repository cleanup**
   - Replace files explicitly marked as draft/current with their final versions.
   - Preserve the frozen experimental artifacts needed to reproduce the reported results.

These future updates are **not new model experiments** unless explicitly documented as a separate research version. The current reported comparison and test-set results remain frozen.

## Reproducibility

See:

- [Results summary](results/RESULTS.md)
- [Reproducibility notes](analysis/REPRODUCIBILITY.md)
- [Canonical project handoff](project_handoff/AerialWaste_Landfill_Research_Master_Handoff.docx)
- [Current manuscript source](paper/latex/main.tex)
- [Bibliography source](paper/latex/references.bib)
- [Binary artifact checklist](paper/BINARY_ARTIFACTS_CHECKLIST.md)

The dataset and large model artifacts are intentionally excluded. They must be obtained through their appropriate distribution channels.

## Paper status

**Current status: experimental results frozen; current manuscript package uploaded; publication/submission status may change as the paper moves through the review process.**

The repository now contains the manuscript source, bibliography, tables, current PDF, figures, experimental results, statistical analyses, calibration analysis, efficiency benchmark, and structured error analysis.

After acceptance/publication, the draft PDF and provisional citation metadata should be updated at the appropriate time. The repository should continue to avoid storing the full dataset, model checkpoints, archives, credentials, or secrets.

## Citation

A provisional citation record is provided in [CITATION.cff](CITATION.cff). Update publication metadata after acceptance/publication.

## License and reuse

No blanket license is granted for the manuscript text, figures, or research results in this repository. Dataset rights remain with their respective owners/providers. Contact the authors before redistributing modified manuscript or figure material.

## Authors

**Ashmit Iyer**, **Sarthak Sharma**  
Kalinga Institute of Industrial Technology (KIIT), Bhubaneswar, India
