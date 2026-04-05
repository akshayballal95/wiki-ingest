# Conversational Memory Architecture

Conversational memory architecture refers to the design of persistent memory mechanisms that enable AI agents to maintain coherence across extended, multi-session dialogues. This is distinct from parametric knowledge storage (e.g., [[LoRA]]) and retrieval-based approaches (e.g., [[Retrieval-Augmented Generation]]) in that it specifically targets the problem of remembering and utilizing information from ongoing user interactions.

## The Problem

LLMs are fundamentally limited by fixed context windows. Even models with large context windows (128K-200K+ tokens) face two critical issues:
1. **Conversation history exceeds context limits** — meaningful human-AI relationships spanning weeks or months inevitably overflow any context budget
2. **Thematic discontinuity** — real conversations rarely maintain thematic continuity; critical information (e.g., dietary preferences) may be buried among thousands of tokens of unrelated discussion

Simply extending context windows delays rather than solves these problems. Longer contexts also do not guarantee effective retrieval, as attention mechanisms degrade over distant tokens.

## Approaches

### Memory-Based Systems
Systems like [[Mem0]] and [[Mem0 Graph Memory|Mem0^g]] extract and maintain concise, structured memories from conversations. They selectively store important information, consolidate related concepts, and retrieve relevant details when needed. This mirrors human cognitive processes of memory formation and recall.

### RAG-Based Systems
[[Retrieval-Augmented Generation|RAG]] approaches treat conversation history as a document collection, chunking and indexing it for retrieval. These are simpler to implement but less efficient — they retrieve raw text chunks rather than distilled facts, leading to higher token consumption.

### Full-Context Processing
Passing the entire conversation history to the LLM at each turn. Highest accuracy but impractical at scale due to extreme latency and token cost.

### Graph-Based Memory
Systems like [[Mem0 Graph Memory|Mem0^g]] and Zep use knowledge graphs to capture entity relationships. These excel at temporal and relational reasoning but introduce additional token overhead and construction latency.

## Trade-offs

| Approach | Accuracy | Latency | Token Cost | Scalability |
|----------|----------|---------|------------|-------------|
| Full-context | Highest | Very high | Very high | Poor |
| Memory-based ([[Mem0]]) | High | Low | Low | Good |
| Graph memory ([[Mem0 Graph Memory|Mem0^g]]) | Highest (non-full-context) | Moderate | Moderate | Good |
| RAG | Moderate | Moderate | Moderate | Good |

## Relationship to Other Memory Types

Conversational memory is complementary to other forms of LLM memory:
- **[[LoRA]]** provides parametric knowledge storage for stable, repeatedly-accessed knowledge
- **[[In-Context Learning]]** provides ephemeral, session-scoped knowledge injection
- **[[Retrieval-Augmented Generation]]** provides non-parametric access to external knowledge bases
- **Conversational memory** ([[Mem0]]) provides persistent, evolving storage of user interaction history
