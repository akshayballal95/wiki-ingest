# Multi-LoRA Systems

Multi-LoRA systems partition knowledge across multiple small [[LoRA]] modules rather than storing everything in a single large adapter. This approach exploits the high [[LoRA Memory Capacity#Parameter Efficiency Is Non-Monotonic|parameter efficiency]] of low-rank adapters to achieve greater total capacity from the same parameter budget.

## Architecture

A multi-LoRA system has three components:

1. **Knowledge partitioning** — dividing source material across modules (e.g., one LoRA per document), isolating knowledge to prevent [[Catastrophic Forgetting]] between adapters
2. **Routing** — selecting which module(s) to activate for a given query at inference time
3. **Merging/composition** — combining the selected modules' parameters before or during inference via [[LoRA Merging Strategies]]

## Oracle Routing vs. Practical Routing

Under **oracle routing** (perfect module selection), distributing a fixed parameter budget across multiple small LoRAs outperforms a single large LoRA. On the PhoneBook benchmark with 64K tokens, multi-LoRA (r=4, 8 modules) with oracle routing reaches ~99.8% accuracy vs. ~32.2% for full-context [[In-Context Learning]].

However, **practical routing** using embedding-based retrieval significantly underperforms the oracle. On PaperQA, embedding-based routing can even underperform a single LoRA trained on all documents, because misrouting activates an irrelevant module and produces worse results than having all knowledge in one place.

| Setting | PaperQA Score (Llama-3.1-8B) |
|---------|------------------------------|
| Oracle routing | 5.92 |
| Embedding routing | 4.94 |
| Single LoRA | 5.24 |

**Implication:** Routing accuracy is the primary bottleneck. The theoretical benefits of modularization only materialize with reliable module selection.

## Merging Strategies

When multiple modules are retrieved, they must be composed. See [[LoRA Merging Strategies]] for details. Key finding: **TIES** is the most robust merging method, and merging more than 1-2 modules degrades performance due to parameter interference.

## When Modularity Helps

Multi-LoRA works best when:
- Knowledge is naturally partitionable (e.g., one document per module)
- Tasks require localized evidence from a single source
- Routing signals are strong (clear topic boundaries)

It struggles when:
- Queries require cross-chunk synthesis (multi-hop reasoning across documents)
- Routing is ambiguous or embedding-based retrieval is unreliable
- The number of merged modules grows beyond 1-2

## Hybrid Approaches

Combining multi-LoRA with [[In-Context Learning]] or [[Retrieval-Augmented Generation]] at inference time consistently improves performance, compensating for the fragmentation inherent in modularized memory. Multi-LoRA Top-3 + ICL achieves the strongest results across NarrativeQA and QuALITY benchmarks.

## Latency

Multi-LoRA with pre-loaded modules achieves lower total processing time than ICL for repeated queries, since it eliminates the need to process thousands of context tokens per query. Dynamic loading adds significant overhead and should be avoided via pre-loading strategies.
