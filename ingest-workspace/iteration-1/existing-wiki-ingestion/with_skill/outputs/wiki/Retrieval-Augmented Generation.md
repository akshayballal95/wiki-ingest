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

## RAG for Conversational Memory

RAG can be applied to [[Conversational Memory]] by treating conversation history as a document collection, segmenting it into fixed-length chunks, embedding them, and retrieving relevant chunks at query time. On the [[LOCOMO Benchmark]], RAG was evaluated with varying chunk sizes (128-8192 tokens) and retrieval counts (k=1 and k=2).

The best RAG configuration (k=2, chunk size=8192) achieves an overall [[LLM-as-a-Judge]] score of approximately 61%, substantially trailing memory-based approaches like [[Mem0]] (66.88%) and Mem0^g (68.44%). This gap exists because RAG retrieves raw text chunks rather than distilled, salient facts — resulting in more noise and less precise context for the LLM. Memory-based systems that extract and consolidate facts before storage consistently surface more relevant information.

## Hybrid Approaches

RAG combined with [[LoRA]] outperforms either approach alone. In [[Multi-LoRA Systems]], adding RAG context at inference compensates for the fragmentation inherent in modularized parametric memory. However, [[In-Context Learning]] combined with LoRA tends to yield even greater improvements than RAG + LoRA, suggesting that continuous global context is more effective at restoring coherence than snippet-based retrieval.
