# RACE-01 Holdout Experiment

Core RACE on the frozen AerialWaste Swin-Tiny and ResNet-50 backbones.

## Protocol
1. Stage A loads only the locked validation split.
2. Evaluate tau in {0.70, 0.75, 0.80, 0.85, 0.90, 0.95}.
3. Select the highest-coverage tau with automatically emitted candidate FNR <= 5% on validation.
4. Freeze tau and save race_policy_config.json.
5. Stage B loads the 2,607-image test split only after the policy is frozen.
6. Re-infer both frozen models once and evaluate primary RACE plus the pre-specified sensitivity grid.
7. Bootstrap the primary RACE metrics with 2,000 resamples.
8. Compare against single-model and static parallel reference policies.

## Core rule
ResNet-50 is Stage 1. If its confidence is below tau, invoke Swin-Tiny. Escalated samples are auto-accepted only when both models agree and Swin-Tiny confidence >= tau; otherwise refer to human review.

Do not tune tau from test labels. Do not overwrite the frozen backbone checkpoints.

The experiment code and final result artifacts will be added to this directory after the confirmed Colab run.
