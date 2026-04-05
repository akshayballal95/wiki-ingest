# LOCOMO Benchmark

LOCOMO (Long-term Conversational Memory) is a benchmark dataset designed to evaluate long-term [[Conversational Memory]] in dialogue systems. Introduced by Maharana et al. (2024), it provides a standardized framework for comparing memory architectures across diverse question types.

## Dataset Structure

LOCOMO comprises 10 extended conversations, each containing:
- Approximately **600 dialogues** on average
- Around **26,000 tokens** per conversation
- Conversations distributed across **multiple sessions**, capturing two individuals discussing daily experiences and past events

Each conversation is accompanied by approximately **200 questions** with corresponding ground truth answers.

## Question Categories

Questions are categorized into four types that test different aspects of memory:

| Category | What It Tests | Example |
|----------|---------------|---------|
| **Single-hop** | Locating a single fact within one dialogue turn | "What did Alice say about her birthday?" |
| **Multi-hop** | Synthesizing information scattered across multiple sessions | "How did Bob's opinion about the restaurant change over time?" |
| **Open-domain** | Integrating conversational memory with external world knowledge | Questions requiring both stored facts and general knowledge |
| **Temporal** | Reasoning about event sequences, relative timing, and durations | "When did they last discuss vacation plans?" |

The dataset originally included an adversarial category (designed to test recognition of unanswerable questions), but this was excluded from evaluation because ground truth answers were unavailable.

## Evaluation Metrics

### Performance Metrics

- **F1 Score (F1)** — token-level overlap between predicted and gold answers
- **BLEU-1 (B1)** — unigram overlap metric

Both lexical metrics have significant limitations for conversational memory evaluation. For example, "Alice is born in July" would score highly against "Alice is born in March" due to lexical overlap, despite containing a critical factual error.

- **[[LLM-as-a-Judge]] (J)** — a complementary metric using a separate LLM to assess response quality across factual accuracy, relevance, completeness, and contextual appropriateness. Due to stochastic variation, 10 independent runs are conducted and mean scores with +/- 1 standard deviation are reported. This metric aligns better with human judgment.

### Deployment Metrics

- **Token consumption** — number of tokens extracted during retrieval, measured using cl100k_base encoding
- **Search latency** — time to search memory/chunks (p50 and p95)
- **Total latency** — end-to-end time including retrieval and answer generation

## Key Results

On LOCOMO, [[Mem0]] and its graph-enhanced variant Mem0^g set new state-of-the-art results across most question types and metrics, outperforming established baselines including LoCoMo, ReadAgent, MemoryBank, MemGPT, A-Mem, LangMem, Zep, and OpenAI's memory feature.

Notable findings:
- Memory-based systems offer the best accuracy-efficiency trade-off
- Full-context processing achieves the highest quality (J=72.90%) but at prohibitive latency
- Graph-based memory ([[Mem0]]^g) particularly excels at temporal reasoning
- Dense natural language memory ([[Mem0]]) excels at single-hop and multi-hop retrieval

## Baselines Evaluated

The benchmark has been used to evaluate six categories of systems:

1. **Established benchmarks** — LoCoMo, ReadAgent, MemoryBank, MemGPT, A-Mem
2. **Open-source memory** — LangMem (Hot Path)
3. **[[Retrieval-Augmented Generation|RAG]]** — with varying chunk sizes (128-8192 tokens) and k values (1-2)
4. **Full-context** — entire conversation in context window
5. **Proprietary** — OpenAI ChatGPT memory
6. **Memory providers** — Zep (temporal knowledge graph)

## Related Concepts

- [[Knowledge Memory Benchmarks]] — benchmarks for parametric (LoRA-based) knowledge storage, complementary to LOCOMO's focus on conversational memory
- [[Conversational Memory]] — the capability that LOCOMO evaluates
- [[Mem0]] — system that achieves state-of-the-art results on LOCOMO
