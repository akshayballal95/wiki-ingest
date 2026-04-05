# Retrieval-Augmented Generation (RAG)

Retrieval-Augmented Generation (RAG) is a non-parametric approach to extending LLM knowledge by retrieving relevant external documents and conditioning generation on them. Introduced by Lewis et al. (2020), RAG has become a standard method for knowledge-intensive NLP tasks.

## How It Works

1. A query is encoded into an embedding
2. Similar documents are retrieved from an external store via embedding similarity (top-k selection)
3. Retrieved documents are injected into the model's context alongside the query
4. The model generates a response conditioned on both the query and the retrieved context

## Limitations

- **Context window constraint** — retrieved content must fit within the model's context budget, limiting how much evidence can be provided
- **Embedding similarity limitations** — top-k retrieval based on embedding similarity can fail to represent the information actually needed for a given query (Weller et al., 2025)
- **Fragmentation** — chunking documents for retrieval can fragment evidence, hindering holistic use of long documents (Kuratov et al., 2024)
- **Computational cost** — must process retrieved context tokens for every query, making repeated access expensive

## RAG vs. LoRA vs. ICL

RAG, [[LoRA]], and [[In-Context Learning]] represent three axes of memory for LLMs:

| Dimension | RAG | LoRA | ICL |
|-----------|-----|------|-----|
| Memory type | Non-parametric (external) | Parametric (weights) | Non-parametric (context) |
| Knowledge update | Update document store | Retrain adapter | Update prompt |
| Inference cost | Per-query retrieval + context processing | One-time module loading | Per-query context processing |
| Scalability | Scales with store size | Limited by module capacity | Limited by context window |
| Best for | Large, dynamic knowledge bases | Stable, repeated-access knowledge | Small, session-specific knowledge |

## Hybrid Approaches

RAG combined with [[LoRA]] outperforms either approach alone. In [[Multi-LoRA Systems]], adding RAG context at inference compensates for the fragmentation inherent in modularized parametric memory. However, [[In-Context Learning]] combined with LoRA tends to yield even greater improvements than RAG + LoRA, suggesting that continuous global context is more effective at restoring coherence than snippet-based retrieval.

## RAG for Conversational Memory

RAG is also used as a baseline approach for [[Conversational Memory Architecture|conversational memory]]. In this setting, conversation history is treated as a document collection, segmented into fixed-length chunks (128-8192 tokens), and retrieved by semantic similarity. On the [[LOCOMO Benchmark]], the best RAG configuration (k=2, 8192 tokens) achieves an overall J score of ~61%, substantially below [[Mem0]]'s 66.88%. RAG's disadvantage in conversational memory is that it retrieves raw text chunks rather than distilled facts, leading to higher token consumption and less precise context. See [[Conversational Memory Baselines]] for the full comparison.
