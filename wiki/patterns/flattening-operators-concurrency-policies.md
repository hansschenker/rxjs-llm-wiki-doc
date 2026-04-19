---
title: Flattening Operators as Concurrency Policies
category: patterns
tags: [mergeMap, switchMap, concatMap, exhaustMap, overlap, concurrency, flattening]
sources: [set-builder-notation.md]
updated: 2026-04-19
---

# Flattening Operators as Concurrency Policies

The four higher-order flattening operators are not primarily about transforming values — they are **policies for managing overlap**. Each one answers the question: *what happens when a new inner Observable arrives before the previous one has completed?*

## The Causal Chain

```
Producer/Consumer separation
  → independent timing
  → new value arrives before old work finishes
  → overlap
  → policy required
```

Flattening operators *are* that policy.

## The Four Policies

| Operator | Policy | When to use |
|----------|--------|-------------|
| `mergeMap` | Allow overlap — run all inner Observables concurrently | Order doesn't matter, all results needed |
| `switchMap` | Cancel old, start new — only the latest matters | Live queries, search typeahead, any "latest wins" scenario |
| `concatMap` | Queue — run inner Observables one at a time in arrival order | Ordered sequential operations, animations |
| `exhaustMap` | Ignore new while busy — first one wins | Form submit, login, actions that must not overlap |

## Marble Diagrams

### mergeMap — allow overlap
```
source:   --a------b----c--|
           mergeMap(x => timer(3))
output:   --a--b--ac---bc--c--|
```

### switchMap — cancel on new arrival
```
source:   --a------b----c--|
           switchMap(x => timer(3))
output:   --a--(b cancel a)--b--(c cancel b)--c--|
```

### concatMap — queue
```
source:   --a--b--c--|
           concatMap(x => timer(3))
output:   --aaa--bbb--ccc--|
```

### exhaustMap — ignore while busy
```
source:   --a--b--c--|
           exhaustMap(x => timer(3))
output:   --aaa--(b ignored)--(c ignored)--|
```

## TypeScript Pattern

```typescript
// switchMap: live search — cancel stale request on each keystroke
const results$ = searchInput$.pipe(
  debounceTime(300),
  distinctUntilChanged(),
  switchMap((query: string) => searchApi$(query))
)

// exhaustMap: form submit — ignore re-clicks while request in flight
const submit$ = submitClick$.pipe(
  exhaustMap(() => submitForm$())
)

// concatMap: sequential operations — preserve order
const saved$ = saveActions$.pipe(
  concatMap((action: SaveAction) => saveToApi$(action))
)

// mergeMap: parallel independent work — order irrelevant
const processed$ = fileList$.pipe(
  mergeMap((file: File) => processFile$(file))
)
```

## The Decision Rule

```
Need latest only?          → switchMap
Must not overlap?          → exhaustMap
Must preserve order?       → concatMap
Order irrelevant?          → mergeMap
```

## Related Pages

- [Producer/Consumer Separation](../architecture/producer-consumer-separation.md)
- [RxJS as Query and Orchestration Language](../architecture/rxjs-query-and-orchestration.md)
- [Subscription as Control Surface](../architecture/subscription-control-surface.md)
