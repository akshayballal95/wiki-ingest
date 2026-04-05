# LoRA Memory Capacity

The memory capacity of a [[LoRA]] module — how much factual knowledge it can reliably store and retrieve — is finite, rank-dependent, and governed by non-obvious trade-offs between absolute capacity and parameter efficiency.

## Capacity Scales with Rank

Increasing the LoRA rank (r) consistently increases the amount of knowledge a module can memorize. On the CounterFact benchmark, efficacy rises steadily with rank across data sizes from 8K to 14K tokens. Rank effectively controls the parameter budget available for storing new knowledge.

## Finite Saturation Point

At any fixed rank, there is a **saturation point** — a knowledge load beyond which performance collapses. Higher ranks push this ceiling higher but cannot eliminate it. Lower ranks saturate earlier.

This has practical implications: memory budgeting is unavoidable. You must estimate how much knowledge you need to store and select a rank that provides sufficient capacity.

## Parameter Efficiency Is Non-Monotonic

A critical insight: the highest rank is **not** the most efficient choice. Efficiency — defined as the maximum storable knowledge (T_max) divided by the number of trainable parameters (N_params) — peaks at intermediate ranks and then declines.

| Metric | Behavior |
|--------|----------|
| Absolute capacity | Increases monotonically with rank |
| Parameter efficiency | Peaks at low-to-mid ranks, then declines |
| Saturation threshold | Shifts higher with rank |

This means simply maximizing rank wastes parameters. For memory-centric LoRA design, right-sizing the rank to the knowledge load is more efficient than defaulting to maximum rank.

## Practical Guidance

1. **Estimate your knowledge load** before choosing rank
2. **Prefer lower ranks** when the knowledge fits — they store more per parameter
3. **Use [[Multi-LoRA Systems]]** to scale beyond a single module's capacity rather than pushing rank to its limits
4. **Monitor for saturation** — performance degradation is the signal that capacity is exceeded

## Benchmarks Used

These findings are established on [[Knowledge Memory Benchmarks]] including PhoneBook (key-value memorization) and CounterFact (counterfactual knowledge editing), tested on Llama-3.1-8B and Qwen3-8B.
