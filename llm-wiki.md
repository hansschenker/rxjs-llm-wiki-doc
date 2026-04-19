# RxJS Advanced Topics Wiki

A personal knowledge base for advanced RxJS — operators, patterns, architecture, scheduler internals, and multicasting — maintained by Claude Code and browsed in Obsidian.

This document is the schema for the wiki: it tells Claude Code how the wiki is structured, what conventions to follow, and what workflows to run when ingesting sources, answering questions, or maintaining the wiki. It evolves as the wiki grows.

## The core idea

Instead of re-deriving knowledge from raw sources on every question (RAG-style), Claude Code **incrementally builds and maintains a persistent wiki** — a structured, interlinked collection of markdown files. When you add a new source, Claude reads it, extracts the key information, and integrates it into the existing wiki — updating operator pages, revising concept summaries, noting contradictions, strengthening the evolving synthesis. The knowledge is compiled once and kept current.

The wiki is a **persistent, compounding artifact.** Cross-references are already there. Contradictions have already been flagged. The synthesis reflects everything read so far. The wiki gets richer with every source and every question.

You never write the wiki yourself — Claude writes and maintains all of it. You source material, direct the analysis, and ask questions. Claude handles summarizing, cross-referencing, filing, and bookkeeping. In practice: Claude Code on one side, Obsidian on the other. Claude edits files; you browse the result in real time via Obsidian's graph view and follow links.

## Architecture

**Three layers:**

**Raw sources** — your curated collection of source documents. RxJS source code, GitHub issues, blog posts, conference talks, course notes, your own recordings. Immutable — Claude reads from them but never modifies them. Source of truth.

**The wiki** — a directory of Claude-generated markdown files organized by category (see below). Claude owns this layer entirely: creates pages, updates them when new sources arrive, maintains cross-references, keeps everything consistent.

**The schema** — this file (`llm-wiki.md`). Tells Claude how the wiki is structured, what conventions to follow, and what workflows to run. You and Claude co-evolve it over time.

## Wiki structure

```
wiki/
  index.md          ← content catalog, updated on every ingest
  log.md            ← append-only chronological record
  operators/        ← one page per operator (map, switchMap, debounceTime, …)
  patterns/         ← reusable RxJS patterns (MVU, Effects, polling, retry, …)
  architecture/     ← higher-order design (hot/cold, Subject types, sharing, …)
  internals/        ← scheduler mechanics, VirtualTimeScheduler, lift(), SafeSubscriber, …
  course-scripts/   ← narration scripts and lesson outlines for video courses
raw/
  articles/
  source-code/
  github-issues/
  talk-transcripts/
  course-notes/
```

The existing `C:/Users/HP/Web/Frontend/rxjs/rxjs-wiki/` is the reference implementation. New content follows the same conventions already established there.

## Page conventions

- **Frontmatter** on every page: `title`, `category`, `tags`, `sources` (list of raw filenames that informed the page), `updated` (date).
- **Marble diagrams** as ASCII or Marp-compatible code blocks for any time-based operator.
- **TypeScript examples** with explicit types, `pipe()` chains, `$`-suffix Observables, no `any`.
- **Cross-references** via standard `[text](../category/page.md)` links — never `[[wikilinks]]` (VitePress/Obsidian-incompatible in rendered output).
- Operator pages follow this structure: purpose → signature → marble diagram → key behaviours → common patterns → pitfalls → related operators.

## Source types

- **RxJS source code** — operator implementations, scheduler internals, `lift()` pattern
- **GitHub issues/PRs** — design decisions, deprecation rationale, edge-case discussions (rxjs/rxjs repo)
- **Blog posts** — Ben Lesh, André Staltz/Cycle.js, Michael Hladky, Netanel Basal
- **Conference talks** — ng-conf, RxJS Live, AngularConnect transcripts or notes
- **Your own course notes** — recordings from RxJS Deep Dive, RxJS Insights, Advanced RxJS 4-Layer Model

## Operations

### Ingest

When you drop a new source and ask Claude to process it:

1. Read the source in full.
2. Discuss key takeaways — confirm what to emphasize before writing.
3. Write or update pages in the appropriate `wiki/` category.
4. Update `wiki/index.md` with any new pages.
5. Update cross-references on related pages.
6. Append an entry to `wiki/log.md`: `## [YYYY-MM-DD] ingest | Source Title`.

A single source typically touches 5–15 wiki pages. Ingest one source at a time; stay involved — read the summaries and guide emphasis.

### Query

Ask questions against the wiki:

1. Read `wiki/index.md` first to find relevant pages.
2. Drill into those pages and synthesize an answer with citations.
3. **File valuable answers back into the wiki** as new pages (a comparison you asked for, a pattern you discovered, an architectural insight). Answers that disappear into chat history are wasted.

Output formats depending on the question:
- **Markdown page** — default for concepts, patterns, operator deep-dives
- **Marp slide deck** — for course content and presentations (file under `wiki/course-scripts/`)
- **Marble diagram** — for any time-based or ordering question
- **Comparison table** — for operator selection decisions

### Export to script

When you want to turn a wiki page or topic cluster into a course narration script:

1. Identify the relevant wiki pages.
2. Synthesize them into an insight-driven narration script (not an operator catalog — lead with the *why* and the mental model, not the API).
3. File the script under `wiki/course-scripts/` and link it from `wiki/index.md`.
4. Cross-reference the source pages so the script stays traceable.

Style for scripts: insight-driven, conversational, senior-developer register. Reference the RxJS Insights course scripts as the style benchmark.

### Lint

Periodically health-check the wiki:

- Contradictions between pages (e.g. two pages disagreeing on scheduler behaviour)
- Stale claims superseded by newer RxJS versions (check against current rxjs@7.8.x)
- Orphan pages with no inbound links
- Operators or patterns mentioned but lacking their own page
- Missing cross-references between related operators
- Data gaps that a targeted web search could fill

## Indexing and logging

**`wiki/index.md`** — catalog of every page: link, one-line summary, category, tag list. Updated on every ingest. Claude reads this first on every query to find relevant pages.

**`wiki/log.md`** — append-only record. Prefix format: `## [YYYY-MM-DD] operation | title` (ingest / query / lint / script). Grep-friendly: `grep "^## \[" wiki/log.md | tail -10` gives the last 10 entries.

## Tooling

- **Obsidian** — browse the wiki, graph view to see connections, Marp plugin for slide preview, Dataview plugin for frontmatter queries.
- **Obsidian Web Clipper** — clip articles to markdown for `raw/articles/`.
- **Claude Code** — all wiki writes happen here. Obsidian is read-only from Claude's perspective; Claude is the programmer, the wiki is the codebase.
- The wiki is a git repo — version history, branching, and diffing come for free.

## Why this works for RxJS

RxJS has a large, interconnected operator surface. The relationships between operators (switchMap vs exhaustMap vs concatMap, Subject vs BehaviorSubject vs ReplaySubject, hot vs cold) are what matter for expertise — not the individual APIs. A wiki that maintains those cross-references persistently is far more useful than re-deriving them from docs on every question. The scheduler internals, the lift() pattern, the multicasting story — these require synthesizing source code, design docs, and community knowledge. A compounding wiki handles this naturally; RAG does not.
