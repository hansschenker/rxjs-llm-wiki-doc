---
title: RxJS as Query Language + Orchestration Language
category: architecture
tags: [rxjs, linq, orchestration, dataflow, execution-policy, concurrency]
sources: [set-builder-notation.md]
updated: 2026-04-19
---

# RxJS as Query Language + Orchestration Language

RxJS is two languages fused into one:

```
RxJS = dataflow (LINQ heritage) + execution policy (Rx model of time/cancellation/scheduling)
```

Neither half alone is sufficient. The query half describes *what* values to derive; the orchestration half describes *when* and *how* work runs.

## The Query Half (from LINQ)

Inherited from the comprehension/LINQ lineage. These operators describe which values pass and how they are shaped:

| Purpose | Operators |
|---------|-----------|
| Filter | `filter`, `distinctUntilChanged`, `take`, `skip` |
| Transform | `map`, `scan`, `reduce`, `pluck` |
| Combine | `combineLatest`, `zip`, `withLatestFrom`, `merge` |
| Group/Partition | `groupBy`, `partition`, `window` |
| Flatten | `mergeMap`, `switchMap`, `concatMap`, `exhaustMap` |

## The Orchestration Half (from Rx model)

Added by Rx when time entered the picture. These operators answer: *when* are values allowed to move, *what happens* when new work arrives, *who owns* the execution:

| Concern | Operators |
|---------|-----------|
| Timing / pacing | `debounceTime`, `throttleTime`, `auditTime`, `sampleTime`, `delay` |
| Cancellation | `switchMap`, `takeUntil`, `timeout` |
| Scheduling | `observeOn`, `subscribeOn` |
| Sharing | `shareReplay`, `publish`, `multicast` |
| Concurrency policy | `mergeMap`, `switchMap`, `concatMap`, `exhaustMap` |

## Operators That Sit in Both Worlds

Some operators are simultaneously query and orchestration:

- `combineLatest` — combines sources (query) but also gates on time (orchestration)
- `bufferTime` / `windowTime` — batches values (query) driven by a clock (orchestration)
- `mergeMap` — flattens inner sequences (query) while defining overlap policy (orchestration)

## The Formula

```
RxJS = dataflow + execution policy
```

- **Dataflow** = the LINQ/comprehension heritage: filter, project, combine
- **Execution policy** = what Rx.NET added: when values move, how overlapping work is handled, on which scheduler, with what cancellation semantics

A pipeline that only uses `filter` and `map` is a query. A pipeline that uses `switchMap` and `debounceTime` is a policy engine. Most real RxJS code is both.

## Related Pages

- [Query Lineage — Set-Builder to RxJS](./query-lineage.md)
- [Producer/Consumer Separation](./producer-consumer-separation.md)
- [Flattening Operators as Concurrency Policies](../patterns/flattening-operators-concurrency-policies.md)
