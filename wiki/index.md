---
updated: 2026-04-19
---

# Wiki Index

Content catalog for the RxJS advanced topics wiki. Claude reads this first on every query to find relevant pages.

## Operators

| Page | Summary | Tags |
|------|---------|------|
| _(none yet)_ | | |

## Patterns

| Page | Summary | Tags |
|------|---------|------|
| [Flattening Operators as Concurrency Policies](./patterns/flattening-operators-concurrency-policies.md) | mergeMap/switchMap/concatMap/exhaustMap as overlap policies — decision table, marble diagrams, TypeScript patterns | flattening, overlap, concurrency, mergeMap, switchMap, concatMap, exhaustMap |

## Architecture

| Page | Summary | Tags |
|------|---------|------|
| [Query Lineage — Set-Builder to RxJS](./architecture/query-lineage.md) | How set-builder notation → Haskell → LINQ → Rx.NET → RxJS all express the same core move with a changing container | lineage, linq, rx-net, haskell, erik-meijer |
| [RxJS as Query Language + Orchestration Language](./architecture/rxjs-query-and-orchestration.md) | RxJS = dataflow (LINQ heritage) + execution policy (Rx model); which operators belong to each half | query, orchestration, dataflow, execution-policy |
| [Producer/Consumer Separation — Four Consequences](./architecture/producer-consumer-separation.md) | Latency, asynchronicity, overlap, pressure mismatch — all four arise from decoupling and can occur in 1:1 scenarios | producer, consumer, latency, backpressure, overlap |
| [Subscription as Control Surface](./architecture/subscription-control-surface.md) | Subscription as execution ownership, not just cleanup — ownership rule, cold/hot topology, Angular patterns | subscription, lifecycle, cancellation, ownership |

## Internals

| Page | Summary | Tags |
|------|---------|------|
| _(none yet)_ | | |

## Course Scripts

| Page | Summary | Tags |
|------|---------|------|
| _(none yet)_ | | |
