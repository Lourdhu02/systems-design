# Rabanser et al. — Failing Loudly (2019)

## Problem

The ML observability vendors all sell drift detection. Most teams implement some form of distribution shift detection. But: which methods actually work to detect harmful shifts (vs harmless ones)? Rabanser, Günnemann, and Lipton (NeurIPS 2019) ran the empirical study.

## Method

They tested combinations of:
- Dimensionality reduction (raw, PCA, learned representations)
- Two-sample tests (KS, MMD, chi-square, classifier-based)
- Single-feature vs multivariate detection

Against many kinds of synthetic and real shifts on image, text, and tabular data.

## Three findings

1. **No single method dominates.** Best technique depends on shift type, data type, and reference window size.
2. **Classifier-based tests** (train a discriminator to distinguish reference from production data; AUC > 0.5 is shift) work well across many domains; underrated relative to univariate tests.
3. **Univariate tests on raw features are weak for multivariate shifts.** A shift that affects the joint distribution but no single feature individually is invisible to per-feature KS.

## What this means for production

- A drift detector based purely on per-feature KS is incomplete. It catches single-feature shifts; multivariate shifts slip through.
- A discriminator-based detector (logistic regression: "given this row, is it from training or production?") is a strong default that vendors often skip.
- Different shifts need different detectors; an ensemble is reasonable.

## What you should steal

- The **discriminator-based drift detector** as a default. Cheap, multivariate, often beats per-feature tests.
- The discipline of **measuring detector quality before deploying it**. A drift detector you don't trust is worse than no detector.
- The framing: **drift detection is hypothesis testing**. You're asking "are these two samples from the same distribution?" Use the statistical machinery designed for that.

## Sources

- "Failing Loudly: An Empirical Study of Methods for Detecting Dataset Shift," Rabanser, Günnemann, Lipton (NeurIPS 2019).
- "A Unified Framework for Data Poisoning Attack to Graph-based Semi-supervised Learning," subsequent work.
- Open-source drift detection libraries: Evidently AI, NannyML, Alibi Detect.
