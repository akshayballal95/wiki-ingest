# Mem0 Graph Memory (Mem0^g)

Mem0^g is an enhanced variant of [[Mem0]] that incorporates graph-based memory representations to capture complex relational structures among conversational elements. It stores memories as a directed labeled graph G = (V, E, L).

## Graph Structure

- **Nodes (V)** represent entities (e.g., people, locations, events), each containing:
  - Entity type classification (Person, Location, Event, etc.)
  - Embedding vector for semantic similarity
  - Creation timestamp metadata
- **Edges (E)** represent relationships between entities (e.g., LIVES_IN, PREFERS)
- **Labels (L)** assign semantic types to nodes (e.g., ALICE - Person, SAN_FRANCISCO - City)

## Extraction Pipeline

A two-stage pipeline transforms unstructured text into structured graph representations:

1. **Entity Extractor** — identifies entities and their types from input text by analyzing semantic importance, uniqueness, and persistence
2. **Relationship Generator** — derives meaningful connections between entities, producing relationship triplets (v_s, r, v_d) that capture the semantic structure

## Update Strategy

When integrating new information:
1. Compute embeddings for source and destination entities in each new relationship triple
2. Search for existing nodes with semantic similarity above a threshold *t*
3. Create new nodes or reuse existing ones based on matches
4. **Conflict detection** — identifies potentially conflicting existing relationships
5. **Update resolver** — an LLM determines if existing relationships should be marked as obsolete (marked invalid rather than deleted, enabling temporal reasoning)

## Retrieval

Mem0^g implements dual-approach retrieval:
- **Entity-centric** — identifies key entities in a query, locates corresponding graph nodes, then explores incoming and outgoing relationships to build a contextual subgraph
- **Semantic triplet** — encodes the query as a dense vector and matches against textual encodings of all relationship triplets, returning those above a similarity threshold

## Performance vs. Base Mem0

| Question Type | Mem0 J | Mem0^g J |
|---------------|--------|----------|
| Single-hop | 67.13 | 65.71 |
| Multi-hop | 51.15 | 47.19 |
| Open-domain | 72.93 | 75.71 |
| Temporal | 55.51 | **58.13** |
| Overall | 66.88 | **68.44** |

Mem0^g excels at temporal and open-domain questions where explicit relational context aids reasoning. For single-hop and multi-hop queries, the base [[Mem0]] performs comparably or better, as relational structure provides limited utility for single-turn retrieval and can introduce inefficiencies for multi-step reasoning.

## Token and Latency Cost

- **Token consumption:** ~3,616 tokens (roughly 2x the base Mem0's 1,764 tokens) due to graph nodes and relationship data
- **Search latency:** p50 = 0.476s, p95 = 0.657s (higher than Mem0 but still faster than most baselines)
- **Total latency:** p50 = 1.091s, p95 = 2.590s

## Implementation

Uses Neo4j as the underlying graph database. LLM-based extractors and update modules leverage GPT-4o-mini with function calling for structured extraction from unstructured text.
