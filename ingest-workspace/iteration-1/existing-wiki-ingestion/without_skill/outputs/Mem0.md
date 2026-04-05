# Mem0

Mem0 (pronounced *mem-zero*) is a scalable memory-centric architecture for AI agents that dynamically extracts, consolidates, and retrieves salient information from ongoing conversations. Introduced by Chhikara et al. (2025), it addresses the fundamental limitation of LLMs' fixed context windows for maintaining coherence across prolonged multi-session dialogues.

## Architecture

Mem0 follows an incremental processing paradigm with two phases:

### Extraction Phase

Upon ingestion of a new message pair (m_{t-1}, m_t), the system uses two complementary context sources:
1. **Conversation summary (S)** — an asynchronous summary of the entire conversation history, retrieved from the database
2. **Recent message sequence** — the last *m* messages from the conversation history (default m=10)

An LLM-based extraction function processes this combined context to extract a set of salient memories (candidate facts) for potential inclusion in the knowledge base.

### Update Phase

Each candidate fact is evaluated against existing memories to maintain consistency and avoid redundancy. The system:
1. Retrieves the top *s* semantically similar memories using vector embeddings (default s=10)
2. Presents candidate facts alongside retrieved memories to an LLM via function calling
3. The LLM selects one of four operations:
   - **ADD** — create new memory when no semantically equivalent memory exists
   - **UPDATE** — augment existing memory with complementary information
   - **DELETE** — remove memory contradicted by new information
   - **NOOP** — no modification needed

## Mem0^g (Graph Memory Variant)

[[Mem0 Graph Memory|Mem0^g]] extends the base architecture with graph-based memory representations, storing memories as a directed labeled graph where entities are nodes and relationships are edges. This enables more advanced reasoning across interconnected facts, particularly for temporal and relational queries.

## Performance

On the [[LOCOMO Benchmark]], Mem0 achieves state-of-the-art results across most question types:

| Question Type | F1 | BLEU-1 | J (LLM-as-Judge) |
|---------------|-----|--------|-------------------|
| Single-hop | 38.72 | 27.13 | 67.13 |
| Multi-hop | 28.64 | 21.58 | 51.15 |
| Open-domain | 47.65 | 38.72 | 72.93 |
| Temporal | 48.93 | 40.51 | 55.51 |

## Deployment Efficiency

Mem0 achieves the lowest search latency among all evaluated methods:
- **Search latency:** p50 = 0.148s, p95 = 0.200s
- **Total latency:** p50 = 0.708s, p95 = 1.440s
- **Token consumption:** ~1,764 tokens per conversation (7k tokens for complete memory store)
- **91% lower p95 latency** and **90%+ token savings** compared to the full-context approach

This efficiency stems from Mem0's selective memory retrieval mechanism, which dynamically identifies and retrieves only the most salient information rather than fixed-size chunks.

## Comparison with Other Memory Approaches

Mem0 consistently outperforms all [[Conversational Memory Baselines]] including established benchmarks (LoCoMo, ReadAgent, MemoryBank, MemGPT, A-Mem), open-source solutions (LangMem), [[Retrieval-Augmented Generation|RAG]] methods, and proprietary solutions (OpenAI, Zep). The full-context approach achieves the highest overall J score (~72.9%) but at prohibitive latency (p95: 17.1s).

## Key References

- Chhikara et al., 2025 — "Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory" (arXiv:2504.19413)
- Code: https://mem0.ai/research
