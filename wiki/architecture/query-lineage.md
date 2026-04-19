---
title: Query Lineage — Set-Builder to RxJS
category: architecture
tags: [lineage, linq, rx-net, haskell, erik-meijer, observable, comprehension]
sources: [set-builder-notation.md]
updated: 2026-04-19
---

# Query Lineage — Set-Builder to RxJS

RxJS is the latest step in a lineage of notations that all express the same core move: **declaratively derive a new collection from a source by selection and transformation rules**. What changes at each step is the container — the kind of thing being queried.

## The Lineage

| Step | Notation | Container | Core idea |
|------|----------|-----------|-----------|
| Set-builder | `{x ∈ X \| p(x)}` | Mathematical set | Membership defined by a property, not order |
| Haskell list comprehension | `[f x \| x <- xs, p x]` | List | Same shape, but ordered and finite |
| LINQ query syntax | `from x in xs where p(x) select f(x)` | IEnumerable<T> | Works over any sequence-like domain |
| Rx.NET | `observable.Where(p).Select(f)` | IObservable<T> | Sequence arriving over time |
| RxJS | `source$.pipe(filter(p), map(f))` | Observable<T> | Same as Rx.NET, in JavaScript |

## What Each Step Changes

- **Set-builder → Haskell**: property membership becomes ordered list generation
- **Haskell → LINQ**: generalized from lists to any monadic sequence; Erik Meijer explicitly designed LINQ from Haskell's monad comprehensions
- **LINQ → Rx.NET**: the source is no longer a pre-existing in-memory collection but a *live sequence of events arriving over time* — `IObservable<T>` is the dual of `IEnumerable<T>`
- **Rx.NET → RxJS**: syntactic adaptation to JavaScript (fluent `pipe()` instead of C# query syntax), same conceptual model

## The Invariant Across All Forms

Every notation in this family has the same skeleton:

```
source       — where values come from
binder       — names each value (x, x <- xs, from x in xs)
predicate    — keeps only matching values (p(x), where, filter)
projection   — shapes the output (f(x), select, map)
result       — new container of derived values
```

The deepest formulation: **Rx is what happens when the comprehension/query tradition is lifted from collections to values moving over time.**

## Why This Matters for RxJS

Understanding this lineage explains why RxJS operators look like query operators (`filter`, `map`, `scan`, `groupBy`, `zip`). They are query operators — inherited from LINQ algebra. The orchestration layer (`switchMap`, `debounceTime`, `shareReplay`) is what Rx.NET added on top of LINQ when time entered the picture.

## Related Pages

- [RxJS as Query and Orchestration Language](./rxjs-query-and-orchestration.md)
- [Producer/Consumer Separation](./producer-consumer-separation.md)
