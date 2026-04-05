# Graph-Based Memory

Graph-based memory is an approach to storing and retrieving information for AI agents using directed labeled graphs, where entities are represented as nodes and their relationships as edges. This structured representation enables complex relational reasoning that flat text-based memory cannot easily support, particularly for temporal reasoning and multi-entity queries.

## Structure

A graph-based memory system represents knowledge as a directed labeled graph G = (V, E, L):

- **Nodes (V)** — entities such as people, locations, events, objects, and concepts. Each node typically contains an entity type classification, an embedding vector for semantic search, and metadata (e.g., creation timestamp).
- **Edges (E)** — relationships between entities, stored as triplets (source_node, relation, destination_node). Relations are labeled (e.g., "lives_in", "prefers", "happened_on").
- **Labels (L)** — semantic type assignments for nodes (e.g., Person, City, Event).

## Construction Pipeline

Building a graph-based memory from conversational or textual input typically involves:

1. **Entity extraction** — an LLM-based module identifies entities and their types from text, analyzing semantic importance, uniqueness, and persistence
2. **Relationship generation** — a second LLM-based module examines entity pairs and determines meaningful connections, assigning relationship labels
3. **Conflict detection** — when new information arrives, potentially conflicting existing relationships are identified
4. **Update resolution** — an LLM determines whether existing relationships should be marked as obsolete or updated

This pipeline is exemplified by [[Mem0]]'s graph-enhanced variant (Mem0^g).

## Retrieval Strategies

Graph-based memory supports two complementary retrieval approaches:

1. **Entity-centric retrieval** — identifies key entities in the query, locates corresponding graph nodes via semantic similarity, then explores incoming and outgoing edges to construct a relevant subgraph. Best for targeted, entity-focused questions.
2. **Semantic triplet retrieval** — encodes the query as a dense vector and matches it against textual encodings of all relationship triplets, returning those above a similarity threshold. Best for broader conceptual queries.

Combining both approaches enables handling of both targeted entity lookups and broader associative queries.

## Advantages Over Flat Memory

- **Temporal reasoning** — explicit relational structure helps model event sequences and chronological relationships. On the [[LOCOMO Benchmark]], [[Mem0]]^g's graph memory achieves a temporal J score of 58.13 compared to 55.51 for flat Mem0.
- **Relational queries** — queries involving multiple entities and their connections benefit from explicit graph traversal
- **Conflict tracking** — relationships can be marked as obsolete rather than deleted, preserving temporal history

## Limitations

- **Higher token cost** — graph representations roughly double the token footprint compared to flat natural language memories (~14K vs ~7K tokens per conversation in Mem0)
- **Higher latency** — graph retrieval adds search overhead (Mem0^g search p50: 0.476s vs Mem0: 0.148s)
- **Not always beneficial** — for simple single-hop or multi-hop questions, graph structure provides limited or no advantage over dense natural language memory
- **Construction overhead** — some systems like Zep consume over 600K tokens per conversation for graph construction, with significant background processing delays

## Implementations

- **[[Mem0]]^g** — uses Neo4j as the graph database, with GPT-4o-mini for entity extraction and relationship generation. Achieves the highest overall J score (68.44%) on [[LOCOMO Benchmark]]
- **Zep** — a temporal knowledge graph architecture that caches abstractive summaries at every node. Achieves strong open-domain performance but at extreme token cost (>600K tokens per conversation)

## Related Concepts

- [[Conversational Memory]] — the broader problem that graph-based memory helps address
- [[Retrieval-Augmented Generation]] — an alternative approach using chunk-based retrieval rather than structured graphs
- [[Knowledge Memory Benchmarks]] — benchmarks for evaluating memory systems, though primarily focused on parametric (LoRA-based) memory
