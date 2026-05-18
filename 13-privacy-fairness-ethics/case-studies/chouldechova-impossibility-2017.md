# Chouldechova — Fairness impossibility (Big Data 2017)

## Problem

In 2016, ProPublica published an analysis of COMPAS, a risk-assessment tool used in US criminal sentencing. The investigation found that Black defendants were assigned high-risk scores at higher rates than white defendants, and false-positive rates differed across groups. COMPAS's vendor (Northpointe) responded that the scores were calibrated — predictive parity held.

Both sides appeared correct. Chouldechova (Big Data 2017) explained why.

## The finding

When base rates of the outcome differ across groups, no single risk score can simultaneously satisfy:

1. **Calibration / predictive parity:** at any score level, the rate of positive outcome is equal across groups.
2. **Equal false-positive rate:** among true negatives, the fraction predicted positive is equal across groups.
3. **Equal false-negative rate:** among true positives, the fraction predicted negative is equal across groups.

The math: each criterion is a constraint on (TP, FP, TN, FN) per group. With different base rates, the constraints are jointly infeasible.

Kleinberg, Mullainathan, Raghavan (ITCS 2017) proved the same result in a more general form.

## What this means in practice

- **Fairness has multiple definitions** that disagree mathematically.
- **You must choose** which criterion matters for your domain.
- A model that's "fair" by one definition can be correctly described as "unfair" by another.

Domain-specific choices:

| Domain | Often the dominant criterion |
|--------|------------------------------|
| Criminal justice | Equal FPR (don't over-detain a group) |
| Lending | Calibration (don't make overconfident promises) |
| Hiring | Demographic parity (representation) — but contested |
| Medical screening | Equal FNR (don't miss disease in a group) |

The choice has policy and ethical content, not just technical. The engineering job is to **make the trade-off explicit, measure all the relevant metrics, and let stakeholders pick**.

## What you should steal

- The discipline of **measuring multiple fairness metrics** per protected group. No one number captures fairness.
- The honesty of **documenting which definition you optimised** and which you didn't.
- The instinct to **resist the framing "make it fair"** — instead, "make these specific metrics meet these thresholds for these groups." Fairness is plural.

## Sources

- "Fair prediction with disparate impact: A study of bias in recidivism prediction instruments," Alexandra Chouldechova (Big Data, 2017).
- "Inherent Trade-Offs in the Fair Determination of Risk Scores," Kleinberg, Mullainathan, Raghavan (ITCS 2017).
- "Machine Bias," Angwin, Larson, Mattu, Kirchner (ProPublica, 2016).
- "Fairness and Machine Learning," Barocas, Hardt, Narayanan (online textbook, ongoing).
