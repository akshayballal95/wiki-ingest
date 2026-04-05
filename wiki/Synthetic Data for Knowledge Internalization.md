# Synthetic Data for Knowledge Internalization

When using [[LoRA]] as a [[LoRA Memory Capacity|knowledge memory]], the format of training data critically determines how effectively knowledge is internalized. Raw text is an inefficient substrate — converting it into structured synthetic formats dramatically improves memorization and retrieval.

## Synthetic Formats

Three primary formats, compared against raw text (Original):

- **QA (Question-Answer)** — source material reformulated as question-answer pairs. Strongest single format. Task-aligned and provides explicit learning signal.
- **Summary** — condensed summaries of source content. Moderate improvement.
- **Rewrite** — paraphrased versions of the source. Weakest synthetic format alone, but complementary in mixtures.

## Performance Comparison

Tested on the [[Knowledge Memory Benchmarks|PaperQA benchmark]] (LLM judge scoring, 0-10 scale):

| Format | Llama-3.1-8B | Qwen3-8B | Avg Tokens |
|--------|-------------|----------|------------|
| Original (raw text) | 3.187 | 4.587 | 947 |
| QA40 | 5.893 | 5.409 | 1,401 |
| Summary8 + QA40 | 6.380 | **5.662** | 2,995 |
| Original + Summary8 + Rewrite4 + QA40 | **6.822** | 5.614 | 4,088 |

All synthetic formats substantially outperform raw text. Performance improves monotonically as the amount of synthetic supervision scales up.

## Mixing Formats

Combining multiple synthetic formats yields complementary benefits beyond any single format. The most comprehensive mixture (Original + Summary + Rewrite + QA) achieves the best results on Llama, while Summary + QA is optimal on Qwen. The key insight: supplementing the model with diverse views of the same content provides useful extra training signals.

## Generator Quality Matters

The quality of the model generating synthetic data directly impacts [[LoRA]] performance. GPT-4.1-generated QA data consistently outperforms Llama-3.1-8B-generated data across the same training pipeline. Superior generators produce more accurate, logically sound, and comprehensive training data.

## Implications

1. **Never train on raw text alone** when using LoRA for knowledge storage — always convert to structured synthetic formats
2. **QA format is the strongest single choice** — start with QA pairs
3. **Mix formats for best results** — diversity of representation matters
4. **Invest in generator quality** — better synthetic data generators yield better LoRA modules
5. These findings are relevant to any system that uses fine-tuning to internalize knowledge, beyond just LoRA
