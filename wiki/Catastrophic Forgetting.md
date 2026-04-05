# Catastrophic Forgetting

Catastrophic forgetting is the phenomenon where a neural network, upon learning new information, abruptly loses previously learned knowledge. First characterized by Kirkpatrick et al. (2017) in the context of continual learning, it remains a central challenge in updating large language models with new knowledge.

## The Problem

When an LLM is fine-tuned on new data, gradient updates to shared parameters can overwrite representations that encode existing knowledge. Full retraining or supervised fine-tuning on narrow datasets is particularly prone to this effect.

## Mitigation Approaches

- **[[Parameter-Efficient Fine-Tuning]]** — methods like [[LoRA]] freeze the base model and only train small adapter modules, limiting the scope of parameter changes and reducing forgetting risk
- **[[In-Context Learning]]** — avoids parameter modification entirely by injecting knowledge at inference time
- **[[Retrieval-Augmented Generation]]** — externalizes knowledge to a retrieval store, keeping model weights untouched
- **Continual learning techniques** — regularization, rehearsal, and architectural methods designed to preserve old knowledge while learning new information
- **Modular approaches** — [[Multi-LoRA Systems]] isolate new knowledge in separate modules, preventing interference with existing adapters

## Relationship to LoRA Memory

[[LoRA]] mitigates catastrophic forgetting by design: the base model weights are frozen, and new knowledge is stored in low-rank adapter matrices. However, LoRA modules themselves have finite [[LoRA Memory Capacity|capacity]], and overloading a module can still lead to degraded retrieval of stored facts.
