# Mem0

Mem0 (pronounced "mem-zero") is a scalable memory-centric architecture for AI agents that dynamically extracts, consolidates, and retrieves salient information from ongoing conversations. It addresses the fundamental limitation of LLMs relying on fixed context windows, which restricts their ability to maintain coherence across extended, multi-session dialogues. Mem0 operates as a form of [[Conversational Memory]], providing persistent storage of user information across sessions.

## Architecture

Mem0 follows an incremental processing paradigm with two main phases: **extraction** and **update**.

### Extraction Phase

The extraction phase processes a new message pair (m_{t-1}, m_t) — typically a user message and an assistant response — to identify salient facts worth storing. To establish context, the system draws on two complementary sources:

1. **Conversation summary (S)** — an asynchronous summary of the entire conversation history, refreshed periodically by a separate module
2. **Recent message sequence** — the last *m* messages (m=10 in experiments), providing granular temporal context

An LLM-based extraction function processes these inputs together with the new message pair, producing a set of candidate facts for potential inclusion in the knowledge base.

### Update Phase

Each candidate fact is evaluated against existing memories to maintain consistency and avoid redundancy. The system retrieves the top *s* semantically similar memories (s=10) using vector embeddings, then presents them alongside the candidate fact to an LLM through a function-calling interface. The LLM selects one of four operations:

| Operation | When Used |
|-----------|-----------|
| **ADD** | No semantically equivalent memory exists |
| **UPDATE** | Existing memory needs augmentation with complementary information |
| **DELETE** | Existing memory is contradicted by new information |
| **NOOP** | Candidate fact requires no modification to the knowledge base |

This approach leverages the LLM's reasoning capabilities to determine the appropriate operation based on the semantic relationship between candidate facts and existing memories, rather than using a separate classifier.

## Mem0^g: Graph-Enhanced Variant

Mem0^g extends the base architecture with [[Graph-Based Memory]] representations. Memories are stored as a directed labeled graph G = (V, E, L) where:

- **Nodes (V)** represent entities (people, locations, events, etc.) with type classifications and embedding vectors
- **Edges (E)** represent relationships between entities as triplets (source, relation, destination)
- **Labels (L)** assign semantic types to nodes (e.g., Person, City, Event)

The extraction process in Mem0^g uses two LLM-based modules:
1. An **entity extractor** that identifies entities and their types from conversation text
2. A **relationship generator** that derives meaningful connections between entities as labeled triplets

The update phase employs a **conflict detector** to identify potentially conflicting relationships and an **update resolver** to determine whether existing relationships should be marked as obsolete.

### Retrieval in Mem0^g

Mem0^g implements dual retrieval:
- **Entity-centric retrieval** — identifies key entities in the query, locates corresponding graph nodes, and explores incoming/outgoing relationships to build a relevant subgraph
- **Semantic triplet retrieval** — encodes the query as a dense vector and matches against textual encodings of relationship triplets for broader conceptual queries

## Performance

Evaluated on the [[LOCOMO Benchmark]], Mem0 and Mem0^g consistently outperform existing [[Conversational Memory]] systems across most question categories.

### Accuracy (LLM-as-a-Judge Score)

| Method | Single-Hop | Multi-Hop | Open-Domain | Temporal | Overall J |
|--------|-----------|-----------|-------------|----------|-----------|
| LoCoMo | - | - | - | - | - |
| ReadAgent | - | - | - | - | - |
| MemGPT | - | - | - | - | - |
| A-Mem* | 39.79 | 18.85 | 54.05 | 49.91 | 48.38 |
| LangMem | 62.23 | 47.92 | 71.12 | 23.43 | 58.10 |
| Zep | 61.70 | 41.35 | 76.60 | 49.31 | 65.99 |
| OpenAI | 63.79 | 42.92 | 62.29 | 21.71 | 52.90 |
| Full-context | - | - | - | - | 72.90 |
| **Mem0** | **67.13** | **51.15** | 72.93 | 55.51 | 66.88 |
| **Mem0^g** | 65.71 | 47.19 | **75.71** | **58.13** | **68.44** |

Key findings:
- Mem0 excels at single-hop and multi-hop questions through efficient dense memory retrieval
- Mem0^g achieves the highest overall J score (68.44%) and strongest temporal reasoning (58.13%), as structured relational graphs aid chronological reasoning
- Full-context processing achieves the highest J score (72.90%) but at prohibitive latency cost
- Zep slightly edges out Mem0 on open-domain questions (76.60 vs 72.93) due to its extensive graph construction

### Latency and Efficiency

Mem0 achieves the lowest search latency among all methods:

| Method | Memory Tokens | Search p50 (s) | Search p95 (s) | Total p50 (s) | Total p95 (s) | J Score |
|--------|--------------|----------------|----------------|---------------|---------------|---------|
| A-Mem | 2,520 | 0.668 | 1.485 | 1.410 | 4.374 | 48.38 |
| LangMem | 127 | 17.99 | 59.82 | 18.53 | 60.40 | 58.10 |
| Zep | 3,911 | 0.513 | 0.778 | 1.292 | 2.926 | 65.99 |
| Full-context | 26,031 | - | - | 9.870 | 17.117 | 72.90 |
| **Mem0** | **1,764** | **0.148** | **0.200** | **0.708** | **1.440** | 66.88 |
| **Mem0^g** | 3,616 | 0.476 | 0.657 | 1.091 | 2.590 | 68.44 |

Mem0 achieves 91% lower p95 latency and saves over 90% of token cost compared to the full-context approach, offering a practical balance between accuracy and deployment efficiency.

### Token Efficiency

Mem0 encodes memories in natural language and uses only ~7K tokens per conversation. Mem0^g roughly doubles this to ~14K tokens due to graph node and relationship storage. In contrast, Zep's memory graph consumes over 600K tokens per conversation — about 20 times more than the raw conversation text (~26K tokens) — due to redundant abstractive summaries at every node.

## Comparison with [[Retrieval-Augmented Generation]]

While [[Retrieval-Augmented Generation|RAG]] retrieves fixed-size text chunks from conversation history, Mem0 extracts and stores only the most salient facts as concise memory representations. This key difference means Mem0 surfaces more precise cues to the LLM, leading to better answers as evaluated by [[LLM-as-a-Judge]]. RAG's best configuration (k=2, chunk size=8192) achieves an overall J of ~61%, compared to Mem0's 66.88%.

## Implementation Details

- All LLM operations use GPT-4o-mini as the inference engine
- Vector database uses dense embeddings (text-embedding-small-3) for similarity search
- Mem0^g uses Neo4j as the underlying graph database
- Hyperparameters: m=10 (recent messages), s=10 (similar memories for comparison)

## References

- Chhikara et al., 2025 — "Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory" (arXiv:2504.19413)
- Code: https://mem0.ai/research
