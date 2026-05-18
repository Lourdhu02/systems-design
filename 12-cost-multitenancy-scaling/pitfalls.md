# 12 — Cost, Multi-tenancy, Scaling pitfalls

1. **No per-request cost telemetry.** You discover the bill is 3x your forecast at the end of the month.

2. **"Free" product decisions.** Every "show ML on more pages" doubles a real bill that nobody attributed.

3. **Whole-GPU per replica when 30% utilised.** MIG or shared scheduling would halve the bill.

4. **Spot training without async checkpointing.** Lost work approaches savings.

5. **One pool, mixed workloads.** Online serving and training fight for the same GPUs; one starves the other.

6. **Multi-tenant with no chargeback.** Tenants are blind to the cost they're imposing; usage grows without discipline.

7. **No noisy-neighbour alerting.** One tenant degrades latency for everyone; the others' tickets pile up.

8. **Treating ML as the answer to every problem.** A 91% ML model replaces an 85% rules baseline; the 6 pp doesn't justify the 0.5-1 FTE.

9. **Over-investing in pre-training a Chinchilla-optimal model that's serving-cost prohibitive.** Over-train smaller models for inference economics.

10. **Cloud-locked architecture with no portability.** When the vendor jacks pricing 20%, you have no leverage.
