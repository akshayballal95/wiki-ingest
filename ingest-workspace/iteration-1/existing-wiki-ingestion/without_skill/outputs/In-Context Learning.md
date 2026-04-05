# In-Context Learning (ICL)

In-Context Learning (ICL) is a method for providing knowledge to large language models by injecting information directly into the prompt at inference time, without modifying model parameters. First characterized by Brown et al. (2020), ICL leverages the model's ability to learn from examples and context provided in the input.

## How It Works

Knowledge (documents, examples, facts) is placed directly into the model's context window alongside the query. The model conditions its generation on this extended context. No training or weight updates are involved.

## Strengths

- **Immediate** — no training required; knowledge is available as soon as it's in the prompt
- **Flexible** — any information can be injected at any time
- **Global coherence** — provides continuous context, preserving narrative flow across a full document

## Limitations

- **Context window constraint** — limited by the model's maximum context length; quadratic computational cost with sequence length
- **Per-query cost** — must process the full context for every query, making repeated access expensive
- **No persistence** — knowledge exists only for the duration of the context; not stored between sessions

## ICL vs. LoRA vs. RAG

ICL is one of three axes of memory alongside [[LoRA]] and [[Retrieval-Augmented Generation]]. See the comparison table in [[Retrieval-Augmented Generation#RAG vs. LoRA vs. ICL]].

A key finding from hybrid experiments: ICL combined with [[LoRA]] yields greater performance improvements than RAG + LoRA. This is because ICL provides continuous, global context that restores the narrative coherence lost when knowledge is fragmented across [[Multi-LoRA Systems]], something that snippet-based [[Retrieval-Augmented Generation|RAG]] retrieval cannot fully achieve.

## Performance Context

On the PhoneBook benchmark with 64K tokens of knowledge, full-context ICL achieves only ~32.2% accuracy — it degrades under long-context, high-load conditions. Multi-LoRA with oracle routing reaches ~99.8% on the same task, demonstrating that parametric storage outperforms ICL when the knowledge load exceeds what context can reliably handle.

## Full-Context as Conversational Memory

Full-context ICL (passing the entire conversation history to the LLM) serves as an upper-bound baseline for [[Conversational Memory Architecture|conversational memory]]. On the [[LOCOMO Benchmark]], it achieves the highest overall J score (~72.9%) but at extreme latency (p50: 9.87s, p95: 17.12s) and token cost (~26,000 tokens per query). Memory-based approaches like [[Mem0]] achieve comparable quality (J=66.88) at 91% lower latency and 90%+ token savings, making them more practical for production deployments.
