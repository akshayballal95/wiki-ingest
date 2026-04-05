# Conversational Memory Baselines

A survey of memory-enabled systems evaluated on the [[LOCOMO Benchmark]], providing context for the performance of [[Mem0]] and [[Mem0 Graph Memory|Mem0^g]].

## Established Benchmarks

Previously published methods evaluated on LOCOMO:

- **LoCoMo** (Maharana et al., 2024) — the original benchmark baseline
- **ReadAgent** (Lee et al., 2024) — a human-inspired reading agent with gist memory
- **MemoryBank** (Zhong et al., 2024) — enhancing LLMs with long-term memory
- **MemGPT** (Packer et al., 2023) — LLMs as operating systems with memory management
- **A-Mem** (Xu et al., 2025) — agentic memory for LLM agents

## Open-Source Solutions

- **LangMem** (Hot Path) — demonstrated effectiveness in related conversational tasks but not previously evaluated on LOCOMO. Notable for extremely high search latency (p50: 17.99s, p95: 59.82s) making it impractical for real-time applications, despite competitive J scores.

## [[Retrieval-Augmented Generation|RAG]] Baselines

The entire conversation history is treated as a document collection, segmented into fixed-length chunks (128 to 8192 tokens), embedded using text-embedding-small-3, and retrieved by semantic similarity. Tested with k=1 and k=2 retrieved chunks. Best RAG configuration peaks at ~61% J score (k=2, 8192 tokens), compared to [[Mem0]]'s 66.88%.

## Full-Context Processing

Passes the entire conversation history (~26,000 tokens) within the LLM context window. Achieves the highest overall J score (~72.9%) but at extreme latency (p50: 9.87s, p95: 17.12s) and token cost.

## Proprietary Solutions

- **OpenAI** — ChatGPT's memory feature, evaluated using GPT-4o-mini. Memories generated via prompting and used as complete context for answering. Competitive on open-domain (J=62.29) but weak on temporal (J=21.71) due to missing timestamps.
- **Zep** (Rasmussen et al., 2025) — a temporal knowledge graph architecture for agent memory. Strong on open-domain (J=76.60, highest among all methods) but consumes 600k+ tokens for its memory graph (vs. 7k for [[Mem0]]). Suffers from operational delays — immediate memory retrieval often fails, requiring hours of background processing.

## Performance Summary (Overall J Score)

| Method | Overall J |
|--------|-----------|
| Full-context | 72.90 |
| **Mem0^g** | **68.44** |
| **Mem0** | **66.88** |
| Zep | 65.99 |
| RAG (best) | ~61 |
| LangMem | 58.10 |
| OpenAI | 52.90 |
| A-Mem | 48.38 |

## Key Insight

[[Mem0]] and [[Mem0 Graph Memory|Mem0^g]] offer the best balance of quality and efficiency — trailing full-context by only 4-6 J points while operating at 91% lower latency and 90%+ token savings. Zep achieves competitive accuracy but at 85x the token cost of Mem0.
