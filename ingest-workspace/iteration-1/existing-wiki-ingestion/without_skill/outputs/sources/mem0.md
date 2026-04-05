# Source: Mem0 — Building Production-Ready AI Agents with Scalable Long-Term Memory

- **File:** `mem0.pdf`
- **Authors:** Prateek Chhikara, Dev Khant, Saket Aryan, Taranjeet Singh, Deshraj Yadav
- **Affiliations:** Mem0.ai
- **arXiv:** 2504.19413
- **Date processed:** 2026-04-05

## Concepts Extracted

- Mem0 architecture: extraction phase and update phase
- Memory operations: ADD, UPDATE, DELETE, NOOP via LLM function calling
- Mem0^g: graph-based memory variant with entity extraction and relationship generation
- Graph structure: directed labeled graph with nodes (entities), edges (relationships), labels (types)
- Conflict detection and update resolution for graph memories
- Dual retrieval: entity-centric and semantic triplet approaches
- LOCOMO benchmark for long-term conversational memory evaluation
- LLM-as-a-Judge evaluation metric
- Comparison with 6 baseline categories: established benchmarks, open-source, RAG, full-context, proprietary, memory providers
- Latency and token consumption analysis
- Conversational memory architecture patterns

## Wiki Pages Created

- [[Mem0]]
- [[Mem0 Graph Memory]]
- [[LOCOMO Benchmark]]
- [[Conversational Memory Baselines]]
- [[LLM-as-a-Judge]]
- [[Conversational Memory Architecture]]

## Wiki Pages Updated

- [[Retrieval-Augmented Generation]] — added comparison with Mem0
- [[In-Context Learning]] — added relationship to conversational memory
- [[index]] — added new page entries

## Key Tables Preserved

- Performance comparison across memory-enabled systems (Table 1)
- Latency and token consumption comparison (Table 2)
- Mem0 vs Mem0^g performance by question type
