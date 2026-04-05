# Knowledge Base — Product Spec

An AI-powered tool that maintains a personal wiki. Users drop raw files into a folder; an AI agent reads them, extracts concepts, and builds a structured, interlinked collection of markdown files.

---

## Phase 1: Ingestion (current focus)

### How it works

1. User places raw source files (articles, notes, PDFs, etc.) into a designated `raw/` folder.
2. An AI agent picks up unprocessed files from `raw/`.
3. **Extract.** For each file, the agent reads the source and extracts:
   - Key concepts and ideas.
   - Structured data — tables, benchmarks, comparisons, metrics, timelines.
   - Processes, workflows, and step-by-step procedures.
   - Named entities — people, organizations, models, tools.
   - Claims, conclusions, and open questions.
4. **Integrate.** The agent consults `index.md` and for each extracted item:
   - **Updates existing concept pages** with new information, preserving tables and structured data in their original form (markdown tables, lists, etc.).
   - **Creates new concept pages** for ideas not yet covered.
   - Adds cross-links between related concept files (`[[links]]`).
5. The agent writes a **source summary page** (in `wiki/sources/`) capturing what was extracted and which concept pages were updated. This provides traceability back to the original source.
6. Updates `index.md` with any new files and their one-line descriptions.
7. The agent tracks which raw files have already been processed so nothing is ingested twice.

### Handling large sources

For short sources (articles, blog posts, single papers), the full extract-then-integrate flow works in one pass. For large sources like books, reports, or lengthy documents, the agent chunks the source and processes it sequentially:

1. **Split by natural boundaries.** Chapters, sections, or parts — whatever structure the document provides. Each chunk should be small enough for the agent to process reliably (roughly 20-40 pages).
2. **Process chunks in order.** The agent ingests chapter 1, updates the wiki, then moves to chapter 2. Each chunk builds on a wiki that already reflects everything before it. This matters because later chapters often refine, contradict, or deepen earlier ideas.
3. **Revise as understanding evolves.** The agent should update existing concept pages when a later chunk changes the picture — not just append. If chapter 10 contradicts chapter 2, the wiki should reflect the book's final position, not a running log of every claim in sequence.
4. **Deduplicate.** Books revisit ideas. The agent should recognize when a chunk is reinforcing an existing concept vs. adding genuinely new information, to avoid bloating pages with repetition.
5. **One source summary per chunk.** Each chunk gets its own source summary page (e.g. `wiki/sources/book-title-ch03.md`), providing a breadcrumb trail of what was extracted from where.

### Page philosophy

Each file is a **wiki-style page** — one page per concept, but substantial enough to be useful on its own. A page on "Gradient Descent" should give the reader (human or AI) a complete picture of what the wiki knows about gradient descent. Pages are self-contained but densely cross-linked — the link graph is what makes the wiki navigable and what lets the agent discover relationships between concepts.

### Key behaviors

- **One page per concept.** Each markdown file covers a single concept, entity, or idea — scoped tightly but written thoroughly. Not one file per source, not one file per paragraph. "Machine Learning" is a page. "Gradient Descent" is its own page. They link to each other. Pages grow as more sources mention the concept.
- **Incremental updates.** New sources add to and refine existing pages rather than duplicating information. If a source mentions a concept that already has a page, that page gets updated — not replaced.
- **Dense cross-linking.** The agent connects related concepts with `[[links]]`. The wiki should read like a web of ideas, not a flat list. Every page should link to related concepts, and those relationships are what give the wiki its value.
- **Contradiction handling.** When new information conflicts with what's already in the wiki, the agent flags the contradiction rather than silently overwriting.
- **Index maintenance.** `index.md` is always kept current — every concept file listed with a short description. This is how the agent (and the user) navigates the wiki.

### Folder structure

```
raw/            — user drops source files here (read-only for the agent)
wiki/           — agent-maintained markdown files (one per concept)
wiki/index.md   — master index of all concept files
```

---

## Phase 2: CLI Tool

A command-line interface for interacting with the knowledge base.

- **Ingest command** — trigger processing of new files in `raw/`.
- **Status command** — show what's been ingested, what's pending, wiki stats (page count, link count, recent changes).
- **Lint command** — ask the agent to health-check the wiki: find orphan pages, missing links, stale information, gaps worth investigating.

---

## Phase 3: Search and Q&A

Query the wiki using natural language.

- **Search** — find relevant concept pages by keyword or semantic meaning.
- **Question answering** — ask a question, get an answer synthesized from wiki pages with citations back to specific concept files.
- **Filing answers** — valuable Q&A results can be saved back into the wiki as new concept pages, so exploration compounds into the knowledge base.
