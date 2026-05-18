# DeepMind — Chinchilla scaling laws (2022)

## Problem

In 2020, Kaplan et al. (OpenAI) published scaling laws suggesting that for a fixed compute budget, the right move was to train bigger models on relatively less data. The community ran with this, producing very large models: GPT-3 (175B), Gopher (280B), Megatron-Turing NLG (530B), PaLM (540B). All trained on relatively modest token counts (~300-600B).

Hoffmann et al. (DeepMind, 2022) re-examined Kaplan's analysis with data size as a first-class variable. The finding: most large models were under-trained.

## Method

Train hundreds of models at different (parameters, tokens) combinations. Fit a power-law surface. Find the compute-optimal point — the combination that minimises loss for a given compute budget.

Result: the optimal ratio is roughly **20 tokens per parameter**. A 70B model wants ~1.4T tokens; a 175B model wants ~3.5T tokens.

Chinchilla itself was 70B parameters trained on 1.4T tokens. It outperformed Gopher (280B / 300B tokens) on essentially every benchmark while using the same training compute.

## Three load-bearing implications

1. **Smaller models can be better.** Bigger isn't automatically better; it's better when paired with proportionally more data.
2. **Inference economics matter.** A Chinchilla-class model that beats a 4x-bigger model is 4x cheaper to serve. The cost ratio compounds over the model's serving life.
3. **Data quality and quantity are first-class.** Pre-training pipelines need to source far more (high-quality) tokens than the 2020-2022 era assumed.

## What changed in 2023-2025

Many subsequent models embraced and extended Chinchilla:

- Llama 1 (7B / 1T tokens, 13B / 1T, 33B / 1.4T, 65B / 1.4T) is roughly Chinchilla-optimal.
- Llama 2 trained on 2T tokens.
- Llama 3 trained on 15T tokens — **well beyond Chinchilla-optimal**. Over-trained for inference economics.
- Mistral, Mixtral, and various open models follow the over-training pattern.

The 2024-2026 standard practice: choose model size by *inference cost target*; train as many tokens as you can afford to push past Chinchilla-optimal.

## What you should steal

- The framing: **training is one-time; inference is forever.** Skew toward smaller, longer-trained models if you expect heavy inference.
- The discipline of **fitting your own scaling law** for your domain. The published numbers are for pre-training on web-scale text; your fine-tune / domain-adapt curves may differ.

## Sources

- "Training Compute-Optimal Large Language Models," Hoffmann et al. (DeepMind, 2022).
- "Scaling Laws for Neural Language Models," Kaplan et al. (OpenAI, 2020).
- Llama 1, 2, 3 technical reports (Meta, 2023-2024).
- "Scaling Laws and Interpretability of Learning from Repeated Data," Hernandez et al. (Anthropic, 2022).
