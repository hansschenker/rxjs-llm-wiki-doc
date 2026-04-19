# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repo is the schema and seed document for an RxJS advanced topics wiki — a persistent, compounding knowledge base maintained by Claude Code and browsed in Obsidian. `llm-wiki.md` is the master schema: it defines the wiki structure, conventions, and workflows (ingest, query, lint, export-to-script).

## Wiki layout (when instantiated)

```
wiki/
  index.md          ← read this first on every query
  log.md            ← append-only log, prefix ## [YYYY-MM-DD] operation | title
  operators/        ← one page per RxJS operator
  patterns/         ← reusable patterns (MVU, Effects, polling, retry …)
  architecture/     ← hot/cold, Subject types, sharing, multicasting
  internals/        ← scheduler mechanics, lift(), SafeSubscriber
  course-scripts/   ← narration scripts and lesson outlines
raw/                ← immutable sources, never modified
```

Reference implementation: `C:/Users/HP/Web/Frontend/rxjs/rxjs-wiki/`

## Core workflows

**Ingest** — read source → discuss takeaways → write/update wiki pages → update `index.md` → update cross-references → append to `log.md`. One source touches 5–15 pages. Ingest one at a time.

**Query** — read `index.md` first → drill into relevant pages → synthesize with citations → file valuable answers back into the wiki as new pages.

**Export to script** — synthesize a topic cluster into an insight-driven narration script filed under `wiki/course-scripts/`. Style benchmark: RxJS Insights course scripts (insight-first, not operator-catalog).

**Lint** — check for contradictions, stale claims (verify against rxjs@7.8.x), orphan pages, missing cross-references, data gaps.

## Page conventions

- Frontmatter: `title`, `category`, `tags`, `sources`, `updated`
- Operator pages: purpose → signature → marble diagram → key behaviours → common patterns → pitfalls → related operators
- Links: standard `[text](../category/page.md)` — never `[[wikilinks]]`
- TypeScript examples: explicit types, `pipe()` chains, `$`-suffix Observables, no `any`

## Skills available

- `wiki-ingest` — guided ingest workflow
- `wiki-query` — query with index-first search
- `wiki-expand` — add new pages to an existing category
- `wiki-lint` — health-check pass
