# Conversational Memory

Conversational memory refers to mechanisms that enable AI agents to retain and retrieve information across extended, multi-session dialogues. Without persistent memory, LLM-based agents forget user preferences, repeat questions, and contradict previously established facts between sessions — fundamentally undermining user experience and trust.

## The Problem

Large language models are limited by fixed context windows. Even models with large context support (GPT-4 at 128K tokens, Claude 3.7 Sonnet at 200K tokens, Gemini at 10M+ tokens) face two practical limitations:

1. **Conversation history eventually exceeds any context limit** as meaningful human-AI relationships develop over weeks or months
2. **Real conversations rarely maintain thematic continuity** — a user might mention dietary preferences, discuss coding for hours, then return to food topics. Critical information becomes buried among irrelevant context.

Simply extending context length delays but does not solve the problem, as attention mechanisms degrade over distant tokens.

## Approaches

### Memory-Augmented Systems

These systems extract and store salient facts from conversations in a separate memory store, retrieving relevant memories at query time:

- **[[Mem0]]** — extracts dense natural language memories via an extraction/update pipeline with ADD/UPDATE/DELETE/NOOP operations. Achieves the lowest search latency (p50: 0.148s) among evaluated systems.
- **[[Mem0]]^g** — extends Mem0 with [[Graph-Based Memory]] for relational reasoning. Achieves the highest J score (68.44%) among memory systems on [[LOCOMO Benchmark]].
- **A-Mem** — agentic memory with autonomous evolution capabilities
- **MemGPT** — treats the LLM as an operating system with memory management
- **LangMem** — open-source memory architecture from LangChain
- **Zep** — temporal knowledge graph architecture preserving timestamp information
- **OpenAI Memory** — proprietary memory feature in ChatGPT

### [[Retrieval-Augmented Generation]]

RAG treats conversation history as a document collection, segments it into fixed-length chunks, embeds them, and retrieves relevant chunks at query time. This approach is simpler but retrieves raw text rather than distilled facts, leading to higher token consumption and lower precision.

### [[In-Context Learning]] / Full-Context

The entire conversation history is placed in the model's context window. This achieves the highest accuracy (J=72.90% on LOCOMO) but is impractical at scale due to extreme latency (p95: 17.1s) and token cost (~26K tokens per query). Cost and latency grow linearly with conversation length.

### Parametric Memory

Knowledge can be internalized into model parameters via fine-tuning approaches like [[LoRA]]. This is more suited to stable knowledge bases than dynamic conversational contexts, as it requires retraining for updates.

## Evaluation

The [[LOCOMO Benchmark]] is the primary evaluation framework for conversational memory, testing four question categories:

- **Single-hop** — facts from a single dialogue turn
- **Multi-hop** — synthesizing information across multiple sessions
- **Open-domain** — integrating conversational memory with external knowledge
- **Temporal** — reasoning about event sequences, dates, and durations

Evaluation metrics include F1 score, BLEU-1 for lexical overlap, and [[LLM-as-a-Judge]] (J) for semantic correctness — the latter being preferred as traditional metrics fail to capture factual accuracy in conversational contexts.

## Key Trade-offs

| Approach | Quality (J) | Latency (p95) | Token Cost | Scalability |
|----------|-------------|---------------|------------|-------------|
| Full-context | 72.90% | 17.1s | ~26K/query | Poor |
| Memory (Mem0^g) | 68.44% | 2.6s | ~14K stored | Good |
| Memory (Mem0) | 66.88% | 1.4s | ~7K stored | Good |
| RAG (best) | ~61% | ~5s | Varies | Moderate |
| OpenAI Memory | 52.90% | 0.9s | ~4.4K | Good |

Memory-based systems offer the best balance between accuracy and deployability, maintaining consistent performance regardless of conversation length while full-context approaches face exponentially growing costs.

## Related Concepts

- [[Catastrophic Forgetting]] — the broader problem of knowledge loss, which conversational memory systems must handle when updating stored facts
- [[Knowledge Memory Benchmarks]] — benchmarks for parametric (weight-based) memory, complementary to conversational memory evaluation
