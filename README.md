# wiki-ingest

An AI skill that turns raw source material into a structured, interlinked wiki knowledge base. Drop PDFs, markdown files, or paste URLs — an AI agent reads them, extracts concepts, and builds wiki pages with dense cross-links.

## What it does

- Reads PDFs, markdown files, and web URLs
- Extracts concepts, structured data (tables, benchmarks), processes, entities, and claims
- Creates one wiki page per concept — substantial, self-contained, densely linked
- Updates existing pages with new information instead of duplicating
- Maintains an index (`index.md`) and source summaries for traceability
- Tracks processed files so nothing is ingested twice
- Handles large sources (books, reports) by chunking into chapters/sections

## Installation

### Using the Skills CLI

```bash
npx skills install akshayballal95/wiki-ingest
```

See [skills](https://www.npmjs.com/package/skills) for more details.

### Claude Code plugin

```bash
/plugin install wiki-ingest@akshayballal95/wiki-ingest
```

### Manual installation

Clone the repo and copy `skills/ingest/SKILL.md` into your project's skill/prompt directory:

```bash
git clone https://github.com/akshayballal95/wiki-ingest.git
```

- **Claude Code:** copy to `.claude/skills/ingest/SKILL.md`
- **Cursor:** copy the content into `.cursorrules`
- **Windsurf:** copy the content into `.windsurfrules`
- **Other AI tools:** paste the SKILL.md content as a system prompt or context file

## Usage

### Ingest files from a folder

```
ingest the new papers I added
```

```
process the PDFs in my raw folder
```

On first run, the skill asks where your raw files and wiki live. Defaults to `raw/` and `wiki/`.

### Ingest a URL

```
add this to my wiki: https://example.com/article
```

### Ingest a specific file

```
ingest research-paper.pdf
```

## How it works

The skill follows a four-stage pipeline for each source:

1. **Chunk** — split large sources by chapters/sections (20-40 pages each)
2. **Extract** — pull out concepts, tables, findings, entities, relationships
3. **Plan** — consult the existing wiki index, decide what to create vs update
4. **Write** — create/update wiki pages, update index, write source summary

## Folder structure

After ingestion, your project will have:

```
raw/                  — source files (PDFs, markdown, text)
wiki/                 — concept pages (one per concept)
wiki/index.md         — master index of all concept pages
wiki/sources/         — source summary (one per ingested source)
processed.json        — tracks ingested files by content hash
ingest.config.json    — configured paths (created on first run)
```

## Wiki page style

Pages are written as wiki-style articles:

- **One page per concept** — substantial enough to stand alone
- **Dense `[[cross-links]]`** — Obsidian-compatible wiki links throughout
- **Structured data preserved** — tables and benchmarks stay in tabular form
- **No inline citations** — clean prose with natural references
- **Jargon defined** — technical terms explained on first use

## Works with

- [Claude Code](https://claude.ai/code) — CLI, desktop app, IDE extensions
- [Cursor](https://cursor.com) — via `.cursorrules`
- [Windsurf](https://codeium.com/windsurf) — via `.windsurfrules`
- [Obsidian](https://obsidian.md) — wiki links resolve natively
- Any AI coding tool that supports system prompts or context files

## License

MIT
