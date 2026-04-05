# Source: Understanding LoRA as Knowledge Memory

- **File:** `1_Understanding_LoRA_as_Knowledge_Memory.pdf`
- **Authors:** Seungju Back, Dongwoo Lee, Naun Kang, Taehee Lee, S.K. Hong, Youngjune Gwon, Sungjin Ahn
- **Affiliations:** KAIST, Samsung SDS, NYU
- **Date processed:** 2026-04-05

## Concepts Extracted

- LoRA as modular knowledge memory
- Memory capacity scaling with rank
- Parameter efficiency (non-monotonic with rank)
- Synthetic data formats (QA, summary, rewrite) for knowledge internalization
- Multi-LoRA systems: partitioning, routing, merging
- Merging strategies: TIES, DARE, linear averaging, CAT
- Hybrid memory approaches (LoRA + ICL, LoRA + RAG)
- Inference latency comparison
- Layer/module placement (FFN vs attention, early vs late)
- LoRA variants (DoRA, PiSSA) — no consistent advantage

## Wiki Pages Created

- [[LoRA]]
- [[LoRA Memory Capacity]]
- [[Multi-LoRA Systems]]
- [[LoRA Merging Strategies]]
- [[Synthetic Data for Knowledge Internalization]]
- [[Retrieval-Augmented Generation]]
- [[In-Context Learning]]
- [[Catastrophic Forgetting]]
- [[Parameter-Efficient Fine-Tuning]]
- [[Knowledge Memory Benchmarks]]

## Key Tables Preserved

- Synthetic data format comparison (Table 1)
- NarrativeQA/QuALITY performance across methods (Table 2)
- Merging strategy comparison
- Routing accuracy comparison (oracle vs embedding vs single)
