# LoRA Merging Strategies

When [[Multi-LoRA Systems]] retrieve multiple modules for a query, those modules must be composed into a single set of parameters. The choice of merging strategy significantly impacts performance and robustness.

## Strategies

### TIES (Trim, Elect Sign, Merge)

Resolves interference by trimming small-magnitude parameters, resolving sign conflicts, and then merging. **Most robust strategy** in knowledge-memory settings — substantially improves over routed multi-LoRA baselines and reaches performance comparable to single LoRA.

### Linear Averaging

Simple element-wise averaging of adapter parameters. A strong baseline that is generally less lossy than DARE but less robust than TIES.

### CAT (Concatenation)

Concatenates adapter parameters to increase effective rank. Performs poorly — increasing rank by simple concatenation without alignment is not a reliable composition strategy.

### DARE (Drop And REscale)

Drops parameters stochastically and rescales the remainder. Underperforms linear averaging in memorization settings, likely because stochastic dropping discards critical stored information.

## Comparison

| Strategy | Robustness | Performance (PaperQA, Llama-3.1-8B) |
|----------|-----------|--------------------------------------|
| TIES | Best | 5.41 |
| Linear | Good | 4.92 |
| DARE | Moderate | 1.43 |
| CAT | Poor | 0.19 |

## Number of Merged Modules

Performance **peaks at N=1** (single retrieved module) and degrades monotonically as more modules are merged. Even when all merged modules contain relevant knowledge, additional modules dilute the signal and introduce interference.

**Implication:** Multi-module systems should balance recall (retrieving enough relevant modules) against merge robustness (not merging too many). Merging 1-2 high-confidence modules is preferred over broadly merging many candidates.

## Relationship to Routing

Merging can partially compensate for routing errors — retrieving top-3 and merging via TIES is more robust than committing to a single (possibly wrong) module. However, this benefit has diminishing returns and cannot substitute for accurate routing. See [[Multi-LoRA Systems]] for routing details.
