# 11 — MLOps and CI/CD cheat sheet

1. **Version five things:** code, data (Iceberg/Delta snapshot id), features, model binary, prompts.

2. **Aim for artefact reproducibility**, not bit-exact. Two runs same code + data + config -> same metrics within stderr.

3. **The model registry is the central primitive.** Promotion is a metadata change, not a binary copy.

4. **Pipeline orchestrators:** Dagster / Flyte for new builds, Airflow if you already run it, Kubeflow if you're deeply K8s-native.

5. **Deployment pipeline = CI -> train -> offline eval -> shadow -> canary -> full**. Each gate is automated.

6. **Shadow > canary for catching score distribution and calibration regressions.** Canary catches latency / error / engagement.

7. **Rollback must be a registry-tag operation** measurable in seconds. If it takes a retrain, you don't have rollback.

8. **Prompts are versioned artefacts.** PR review, tests, rollback. Same as code.

9. **Prediction log + model version + features used** is the audit trail. Build it day one.

10. **Owner field is mandatory** on every model, every feature, every dataset. No owner = haunted.
