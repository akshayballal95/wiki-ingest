# LOCOMO Benchmark

LOCOMO (Maharana et al., 2024) is a benchmark designed to evaluate long-term conversational memory in dialogue systems. It is the primary evaluation benchmark used for [[Mem0]] and [[Mem0 Graph Memory|Mem0^g]].

## Dataset Structure

- **10 extended conversations**, each containing approximately 600 dialogues and 26,000 tokens on average
- Conversations are distributed across **multiple sessions**, capturing two individuals discussing daily experiences or past events
- Each conversation is accompanied by ~200 questions with ground truth answers

## Question Categories

Questions are categorized into four types, each testing different memory capabilities:

- **Single-hop** — locating a single factual span within one dialogue turn
- **Multi-hop** — synthesizing information dispersed across multiple conversation sessions
- **Temporal** — accurate modeling of event sequences, relative ordering, and durations
- **Open-domain** — integrating conversational memory with external knowledge

The dataset originally included an adversarial category (unanswerable questions), but this was excluded from evaluation due to unavailable ground truth answers.

## Evaluation Metrics

### Performance Metrics
- **F1 Score (F1)** — token-level overlap between generated and ground truth answers
- **BLEU-1 (B1)** — unigram precision metric
- **[[LLM-as-a-Judge]]** (J) — an LLM evaluates response quality across factual accuracy, relevance, completeness, and contextual appropriateness. Preferred over lexical metrics because F1 and BLEU can produce misleadingly high scores from token overlap despite factual errors

### Deployment Metrics
- **Token consumption** — number of tokens extracted during retrieval, measured using cl100k_base encoding
- **Search latency** — time to search memory or retrieve chunks (p50 and p95)
- **Total latency** — search + answer generation time (p50 and p95)

## Baseline Results

See [[Conversational Memory Baselines]] for the full comparison table of methods evaluated on LOCOMO.
