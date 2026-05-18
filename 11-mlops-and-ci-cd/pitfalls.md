# 11 — MLOps and CI/CD pitfalls

1. **No model registry.** Promotion is a file copy; rollback is a retrain. Slow, error-prone.

2. **Bit-exact reproducibility as a goal.** Engineering effort enormous; value approximately zero.

3. **One pipeline for code + data + model.** Can't change one axis without re-running everything.

4. **Skipping shadow deploy.** Canary catches some regressions; shadow catches score-distribution and calibration regressions earlier.

5. **Auto-rollback that's never been tested.** Confidence in untested infra is false confidence. Game-day it.

6. **Prompts living in code comments / Notion / Slack threads.** No version, no diff, no rollback.

7. **Pipeline orchestrator switch without a multi-quarter plan.** Months of broken pipelines.

8. **No owner field.** Every artefact eventually becomes haunted.

9. **Manual model promotion in production.** SRE asleep at the wheel for the breaking change.

10. **No lineage from prediction back to data.** Audit / regulator / debugging questions become impossible.
