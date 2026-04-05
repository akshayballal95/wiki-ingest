# LLM-as-a-Judge

LLM-as-a-Judge (J) is an evaluation metric that uses a separate, more capable LLM to assess response quality across multiple dimensions, including factual accuracy, relevance, completeness, and contextual appropriateness. It is the primary evaluation metric used for [[Mem0]] on the [[LOCOMO Benchmark]].

## Motivation

Traditional lexical metrics like F1 Score and BLEU-1 exhibit significant limitations when evaluating factual accuracy in conversational contexts. For example, given a ground truth "Alice was born in March" and a generated answer "Alice is born in July," lexical metrics would assign relatively high scores due to token overlap ('Alice,' 'born,' etc.) despite a critical factual error in the birth month. LLM-as-a-Judge addresses this by evaluating semantic correctness rather than surface-level overlap.

## Implementation

The judge model analyzes:
1. The question
2. The ground truth answer
3. The generated answer

It provides a short explanation and a CORRECT/WRONG label. Due to the stochastic nature of evaluations, 10 independent runs are conducted on the entire dataset, reporting mean scores with +/-1 standard deviation.

## Usage in Memory Systems

In the context of conversational memory evaluation ([[LOCOMO Benchmark]]), J scores provide a more reliable signal than F1 or BLEU for comparing memory architectures. For instance, [[Mem0]] achieves a 26% relative improvement in J over OpenAI's memory, a gap that lexical metrics alone would not fully capture.
