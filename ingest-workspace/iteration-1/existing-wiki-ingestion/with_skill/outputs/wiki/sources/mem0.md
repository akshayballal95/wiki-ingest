# Source: Mem0 — Building Production-Ready AI Agents with Scalable Long-Term Memory

- **File:** `mem0.pdf`
- **Authors:** Prateek Chhikara, Dev Khant, Saket Aryan, Taranjeet Singh, Deshraj Yadav
- **Affiliation:** Mem0 (research@mem0.ai)
- **arXiv:** 2504.19413v1
- **Date processed:** 2026-04-05

## Concepts Extracted

- Mem0 architecture (extraction + update pipeline)
- Mem0^g graph-enhanced variant (entity extraction, relationship generation, conflict detection)
- Conversational memory for AI agents
- Graph-based memory representations (directed labeled graphs)
- LLM-as-a-Judge evaluation methodology
- LOCOMO benchmark for conversational memory
- Memory operation taxonomy (ADD, UPDATE, DELETE, NOOP)
- Dual retrieval strategies (entity-centric + semantic triplet)
- Token efficiency analysis across memory systems
- Latency comparison across memory architectures

## Wiki Pages Created

- [[Mem0]]
- [[Graph-Based Memory]]
- [[Conversational Memory]]
- [[LOCOMO Benchmark]]
- [[LLM-as-a-Judge]]

## Wiki Pages Updated

- [[Retrieval-Augmented Generation]] — added RAG for conversational memory section with LOCOMO comparison
- [[In-Context Learning]] — added full-context as conversational memory baseline section

## Key Tables Preserved

- Table 1: Performance comparison of memory-enabled systems (F1, BLEU-1, J scores across 4 question types)
- Table 2: Latency and token consumption comparison (search/total latency at p50/p95, memory tokens, overall J)
