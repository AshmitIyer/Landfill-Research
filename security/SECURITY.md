# Security and public-repository policy

This repository is intended to be public research infrastructure.

## Excluded material

Do not commit:

- API keys, access tokens, passwords, or credentials
- `.env` files or private configuration
- AerialWaste image archives or other large dataset copies
- trained model checkpoints unless explicitly intended for release
- unrelated personal or institutional files

The repository `.gitignore` contains common patterns for datasets, archives, checkpoints, and secrets.

## Before future pushes

Run a secret scan over both the working tree and Git history before publishing sensitive changes. GitHub's secret scanning and push-protection features should also remain enabled where available.

If a credential is ever committed, treat it as compromised: revoke/rotate it and then remove it from the repository history using an appropriate history-rewrite procedure.

## Reporting

For suspected security issues or accidentally exposed credentials, contact the repository maintainers privately rather than opening a public issue containing the secret.
