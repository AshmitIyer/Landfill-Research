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

- `experiments/` — frozen experimental results, predictions, statistical analysis, calibration, efficiency benchmarking, and error analysis.
- `results/` — human-readable results summary.
- `analysis/` — reproducibility and methodological notes.
- `paper/latex/` — current manuscript LaTeX source, bibliography, and table source files.
- `paper/latex/figures/` — manuscript figure location; binary figure upload is the remaining GitHub-connector limitation for this session.
- `paper/final/` — manuscript PDF/DOCX archival location; binary upload is likewise pending manual GitHub upload.
- `project_handoff/` — canonical project handoff document.
- `security/` — public-repository security notes.

## Important dataset provenance note

The working **AerialWaste v3.0** release used in this project contains 11,703 images. The original AerialWaste dataset paper (Torres & Fraternali, *Scientific Data*, 2023) reports 10,434 images for the release described in that publication.

This repository does not silently treat those releases as identical. The manuscript and project handoff should be consulted for the exact provenance/version information used for the experiments.

## Scientific scope

This is an **image-level landfill-candidate screening** task. A positive prediction does not establish that a location is an illegal landfill or that a legal violation has occurred. The intended interpretation is prioritization for subsequent human inspection.

The study is limited to the evaluated AerialWaste release and does not establish cross-geographic, cross-sensor, or cross-domain generalization.

## Reproducibility

See:

- [Results summary](results/RESULTS.md)
- [Reproducibility notes](analysis/REPRODUCIBILITY.md)
- [Canonical project handoff](project_handoff/AerialWaste_Landfill_Research_Master_Handoff.docx)
- [Current manuscript source](paper/latex/main.tex)
- [Bibliography source](paper/latex/references.bib)

The dataset and large model artifacts are intentionally excluded. They must be obtained through their appropriate distribution channels.

## Paper status

**Current status: research results frozen; manuscript is a current draft and may still change before submission/publication.**

The LaTeX source, bibliography, and table source files are now included in the repository. The current draft PDF and PNG figures were uploaded in this ChatGPT session, but the connected GitHub interface available here cannot write binary blobs. Therefore, the **only remaining repository action is manual upload of the binary paper/figure files through GitHub's web interface**.

Once the manuscript is published, replace the draft PDF with the accepted/published version and update `CITATION.cff` with the DOI/publication metadata.

## Citation

A provisional citation record is provided in [CITATION.cff](CITATION.cff). Update publication metadata after acceptance/publication.

## License and reuse

No blanket license is granted for the manuscript text, figures, or research results in this repository. Dataset rights remain with their respective owners/providers. Contact the authors before redistributing modified manuscript or figure material.

## Authors

**Ashmit Iyer**, **Sarthak Sharma**  
Kalinga Institute of Industrial Technology (KIIT), Bhubaneswar, India
