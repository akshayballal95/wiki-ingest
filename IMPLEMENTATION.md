# Knowledge Base — Implementation Plan

## Architecture: Pipeline Agent

The ingestion system is a four-stage pipeline. Each stage has a single job, a clear input, and a clear output. Stages run sequentially — the output of one feeds into the next.

```
Raw Source → [Chunker] → [Extractor] → [Planner] → [Writer]
```

---

### Stage 1: Chunker

**Job:** Split the source into processable pieces.

- For short sources (articles, blog posts, single papers): pass through as a single chunk.
- For large sources (books, reports, long PDFs): split by natural boundaries — chapters, sections, parts. Target roughly 20-40 pages per chunk.
- Chunks are processed in order. The pipeline runs stages 2-4 for each chunk before moving to the next, so the wiki accumulates knowledge sequentially.

**Input:** A raw file from `raw/`.
**Output:** An ordered list of text chunks with metadata (source name, chunk label like "Chapter 3", position in sequence).

---

### Stage 2: Extractor

**Job:** Read a chunk and extract structured information. Pure analysis — no file writing.

Extracts:
- Key concepts and ideas.
- Structured data — tables, benchmarks, comparisons, metrics, timelines.
- Processes, workflows, and step-by-step procedures.
- Named entities — people, organizations, models, tools.
- Claims, conclusions, and open questions.
- Relationships between extracted items.

**Input:** A single text chunk.
**Output:** A structured extraction (JSON or structured markdown) listing everything found, tagged by type.

---

### Stage 3: Planner

**Job:** Decide what changes to make to the wiki. Reads the extraction and the current wiki state, produces a concrete plan.

Steps:
1. Read `index.md` to see what concept pages already exist.
2. For each extracted item, determine: does this belong on an existing page, or does it need a new page?
3. For existing pages that need updates: read the current page content and decide what to add, revise, or flag as contradicted.
4. For new pages: decide the page title, scope, and initial content outline.
5. Determine cross-links — which pages should link to which.
6. Check for deduplication — is this new information, or a repetition of something already in the wiki?

**Input:** Extraction output + current `index.md` + relevant existing wiki pages.
**Output:** A plan listing:
- Pages to update (with what to add/change on each).
- New pages to create (with title, scope, and content outline).
- Cross-links to add.
- Contradictions to flag.

---

### Stage 4: Writer

**Job:** Execute the plan. Mechanical — no judgment calls, just writing.

Steps:
1. For each page update in the plan: read the existing page, merge in new content, preserve existing structure and tables, write it back.
2. For each new page: create the file with the planned content and cross-links.
3. Update `index.md` with any new pages and updated descriptions.
4. Write a source summary page to `wiki/sources/` recording what was extracted and which pages were touched.
5. Mark the raw file (or chunk) as processed.

**Input:** The plan from Stage 3.
**Output:** Updated wiki files.

---

## Folder structure

```
raw/                  — user drops source files here (read-only for the pipeline)
wiki/                 — concept pages (one per concept)
wiki/index.md         — master index of all concept pages
wiki/sources/         — source summary pages (one per ingested source/chunk)
```

---

## Processing tracking

The pipeline needs to know which raw files have been processed. Options:
- A `processed.json` manifest mapping filenames to content hashes. Hash-based tracking detects if a file was modified and needs re-ingestion.
- For large sources processed in chunks, the manifest tracks per-chunk progress so ingestion can resume if interrupted.
