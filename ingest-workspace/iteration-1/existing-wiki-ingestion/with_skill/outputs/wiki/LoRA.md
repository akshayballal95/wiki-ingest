# LoRA (Low-Rank Adaptation)

Low-Rank Adaptation (LoRA) is a [[Parameter-Efficient Fine-Tuning]] technique introduced by Hu et al. (2022) that freezes the pretrained model weights and injects trainable low-rank matrices into transformer layers. Instead of updating all parameters during fine-tuning, LoRA decomposes weight updates into two small matrices (A and B), dramatically reducing the number of trainable parameters while maintaining performance comparable to full fine-tuning.

## How It Works

For a pretrained weight matrix W, LoRA adds a low-rank update:

W' = W + BA

where B is a d×r matrix and A is an r×d matrix, with rank r << d. Only A and B are trained; the original W remains frozen. The rank r is the primary hyperparameter controlling the capacity-cost trade-off.

## LoRA as Knowledge Memory

Beyond its original use for task adaptation, LoRA modules can serve as **modular knowledge memory** — compact, swappable units that store factual knowledge in model parameters. This positions LoRA as a parametric memory mechanism complementary to [[Retrieval-Augmented Generation]] and [[In-Context Learning]].

Key properties of LoRA as knowledge memory:
- **Modular:** LoRA modules can be trained, stored, swapped, and composed independently
- **Parametric:** Knowledge is internalized in weights, not retrieved at inference time
- **Finite capacity:** Each module has a rank-dependent storage ceiling (see [[LoRA Memory Capacity]])
- **Composable:** Multiple modules can be combined via [[LoRA Merging Strategies]] or organized into [[Multi-LoRA Systems]]

LoRA is not a standalone replacement for RAG or ICL. Empirical evidence shows it works best as a **complementary axis of memory** — hybrid systems combining LoRA with ICL or RAG consistently outperform any single approach. LoRA's advantage is in reducing inference latency for repeated access to a stable knowledge base, since knowledge is baked into parameters rather than loaded into context each time.

## Optimizing Knowledge Storage

The effectiveness of LoRA as knowledge memory depends heavily on how training data is formatted. Raw text is an inefficient substrate for memorization. [[Synthetic Data for Knowledge Internalization]] — converting source material into QA pairs, summaries, and rewrites — dramatically improves knowledge retention. Mixing multiple synthetic formats yields the best results.

Placement also matters: applying LoRA to FFN (Feed-Forward Network) layers — the dense transformation blocks within each transformer layer — yields stronger memorization than attention-only placement, and early layers generally outperform late layers for knowledge storage.

## Variants

- **DoRA** (Weight-Decomposed Low-Rank Adaptation) — decomposes into magnitude and direction components
- **PiSSA** (Principal Singular Values and Singular Vectors Adaptation) — initializes LoRA from principal components

Empirical results on knowledge memory tasks show no consistent advantage from switching to DoRA or PiSSA over standard LoRA.

## Key References

- Hu et al., 2022 — Original LoRA paper
- Back et al., 2025 — "Understanding LoRA as Knowledge Memory: An Empirical Analysis"
