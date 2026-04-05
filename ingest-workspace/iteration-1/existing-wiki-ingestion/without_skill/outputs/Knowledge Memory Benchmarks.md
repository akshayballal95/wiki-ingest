# Knowledge Memory Benchmarks

Specialized benchmarks for evaluating how well models can memorize, store, and retrieve factual knowledge — particularly relevant to assessing [[LoRA]] as a knowledge memory mechanism.

## PhoneBook

A synthetic key-value memorization benchmark. Contains fictional name-to-phone-number pairs, programmatically generated to ensure zero overlap with pretraining data.

- **Task:** Given a name, recall the exact phone number
- **Metric:** Exact Match (EM) — character-for-character accuracy
- **Scalable:** Dataset size can be precisely controlled by token count (1K to 20K tokens)
- **Use:** Tests pure memorization capacity, storage scaling, and parameter efficiency of [[LoRA]] modules

## CounterFact (CF)

A counterfactual knowledge editing benchmark (Meng et al., 2022). Contains edits that conflict with pre-trained beliefs (e.g., "Paris is in Italy").

- **Task:** Produce updated factual responses after fine-tuning
- **Metric:** Efficacy score — how reliably the model produces the counterfactual answer
- **Use:** Tests targeted revision of established facts and [[LoRA Memory Capacity|capacity]] under knowledge load

## PaperQA

A novel benchmark introduced by Back et al. (2025) for evaluating knowledge internalization over complex, novel information.

- **Construction:** 450 QA pairs from 15 recent papers (NeurIPS 2024, ICLR 2025, ICML 2025)
- **Question types:** Key Information Recall, Contextual Comprehension, Logical Structure Inference (10 each per paper)
- **Metric:** LLM-as-judge rubric scoring (0-10 scale via GPT-4.1)
- **Use:** Tests ability to internalize and reason over new, complex academic content — more realistic than PhoneBook's synthetic key-value pairs

## Benchmark Selection Guidance

| Benchmark | Tests | Complexity | Realism |
|-----------|-------|-----------|---------|
| PhoneBook | Pure memorization | Low | Synthetic |
| CounterFact | Knowledge editing | Medium | Semi-realistic |
| PaperQA | Complex internalization | High | Realistic |

PhoneBook and CounterFact isolate specific memory properties (capacity, editing). PaperQA evaluates end-to-end knowledge internalization in a more realistic setting. For comprehensive evaluation of [[LoRA]]-based memory, all three provide complementary signal.

## Other Benchmarks Used

- **NarrativeQA** — reading comprehension over full narratives; used to test [[Multi-LoRA Systems]] on long-context, multi-hop reasoning
- **QuALITY** — question answering with long input texts; used alongside NarrativeQA for evaluating multi-LoRA and hybrid approaches
