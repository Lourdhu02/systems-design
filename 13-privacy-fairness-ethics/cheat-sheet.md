# 13 — Privacy, Fairness, Ethics cheat sheet

1. **Data minimisation first.** Don't collect what you don't need; the rest of privacy follows.

2. **Anonymisation usually doesn't work** (Sweeney 1997 onward). Don't promise it.

3. **Differential privacy** is the formal guarantee; DP-SGD is the standard for ML. Costs accuracy and compute.

4. **Federated learning** is for cross-device or cross-org scenarios where centralisation is impossible.

5. **Fairness impossibility theorem** (Chouldechova 2017): you cannot satisfy calibration + equal TPR + equal FPR simultaneously when base rates differ. Pick which two matter.

6. **EU AI Act** (2024-2026 phased): high-risk systems require model cards, bias eval, human oversight, audit logs.

7. **GDPR right-to-erasure** interacts non-trivially with trained models. Procedural deletion of input data is the minimum; machine unlearning is still maturing.

8. **Audit logs are a system requirement, not a feature.** Build them on day one.

9. **Red-teaming is layered.** Golden adversarial set + automated mutation + human red team.

10. **Compliance is a system property** that decomposes into pipeline artifacts: data card, model card, audit log, deletion mechanism, human-in-loop, incident reporting. Each is an engineering deliverable.
