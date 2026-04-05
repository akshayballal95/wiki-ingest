# Parameter-Efficient Fine-Tuning (PEFT)

Parameter-Efficient Fine-Tuning (PEFT) refers to a family of techniques that adapt large language models to new tasks or knowledge by training only a small subset of parameters, rather than updating the entire model. This reduces computational cost, memory requirements, and the risk of [[Catastrophic Forgetting]].

## Key Methods

- **[[LoRA]]** (Low-Rank Adaptation) — injects trainable low-rank matrices into transformer layers. The most widely adopted PEFT method. Can also serve as modular [[LoRA Memory Capacity|knowledge memory]].
- **DoRA** (Weight-Decomposed Low-Rank Adaptation) — decomposes LoRA updates into magnitude and direction components
- **PiSSA** (Principal Singular Values and Singular Vectors Adaptation) — initializes adapter matrices from principal components of the original weights
- **Adapters** — small bottleneck layers inserted between transformer blocks
- **Prefix tuning** — prepends trainable tokens to the input at each layer
- **Prompt tuning** — learns soft prompt embeddings in the input space

## LoRA Variants for Knowledge Memory

In knowledge-memory settings, standard [[LoRA]] performs comparably to DoRA and PiSSA. Empirical results on PaperQA and NarrativeQA show no consistent advantage from switching variants — gains are model-dependent and setting-dependent. Simply swapping to DoRA/PiSSA does not reliably yield large improvements for knowledge storage.

## Why PEFT Matters for Knowledge Updates

Full fine-tuning is impractical for continuous knowledge updates: it risks [[Catastrophic Forgetting]], requires full model retraining, and incurs large computational costs. PEFT methods — especially [[LoRA]] — enable modular, composable knowledge updates that can be trained independently and swapped at inference time, making them practical for [[Multi-LoRA Systems]] and knowledge management pipelines.
