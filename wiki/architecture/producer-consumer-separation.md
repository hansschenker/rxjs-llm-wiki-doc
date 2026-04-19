---
title: Producer/Consumer Separation — Four Consequences
category: architecture
tags: [producer, consumer, latency, asynchronicity, overlap, backpressure, pressure-mismatch]
sources: [set-builder-notation.md]
updated: 2026-04-19
---

# Producer/Consumer Separation — Four Consequences

When a producer and consumer are decoupled — as they always are in RxJS — values stop being just data and become **runtime events that need policy**. The separation introduces four consequences, all of which can occur in a simple 1 Producer → 1 Consumer scenario. N Producers → 1 Consumer amplifies all four but does not create new categories.

## The Four Consequences

| Concept | Core question | Meaning | Typical RxJS response |
|---------|--------------|---------|----------------------|
| **Latency** | How long does delivery take? | Timing gap — value produced now, handled later | `delay`, `timeout` |
| **Asynchronicity** | Do they share a control flow? | Execution gap — producer and consumer run independently | `fromEvent`, `observeOn` |
| **Overlap** | Is old work still running when new arrives? | Simultaneous active work | `mergeMap`, `switchMap`, `concatMap`, `exhaustMap` |
| **Pressure mismatch** | Can the consumer keep up? | Speed gap — producer faster than consumer | `throttleTime`, `debounceTime`, `bufferTime`, `switchMap` |

## Mental Model: The Conveyor Belt

Think of packages moving on a conveyor belt:

- **Latency** — a package takes time to travel from sender to worker
- **Asynchronicity** — packages arrive on their own schedule, not only when the worker asks
- **Overlap** — the worker is still handling package A when package B arrives
- **Backpressure** — packages arrive faster than the worker can sustainably process them

## Three Kinds of Independence

Producer/Consumer separation introduces:

1. **Time independence** — they do not have to run at the same moment → *latency*
2. **Rate independence** — they do not have to run at the same speed → *pressure mismatch*
3. **Execution independence** — they do not share one control flow → *asynchronicity* and *overlap*

## Overlap vs Pressure Mismatch (Key Distinction)

These are related but not the same:

- **Overlap** = more than one unit of work is *alive* during the same time window. Can happen even if the producer is slow.
- **Pressure mismatch** = the producer delivers *faster* than the consumer can sustainably handle. Can happen even without meaningful overlap (e.g. synchronous flood).

Causal chain for overlap:
```
separation → independent timing → new arrival before old work ends → overlap → policy needed
```

RxJS does not have a built-in demand protocol for strict backpressure. Instead it provides operator policies to handle pressure: dropping (`throttleTime`), sampling (`auditTime`), switching (`switchMap`), queueing (`concatMap`), buffering (`bufferTime`).

## Why 1→1 Is Enough

A common misconception is that these problems only appear with multiple producers. They do not:

- A single `mousemove` can trigger expensive rendering faster than the renderer can keep up → pressure mismatch
- A single `click` starting an HTTP request with `mergeMap` → overlap with one producer

N Producers → 1 Consumer increases the *probability and intensity* of all four problems, not the *kind*.

## Related Pages

- [RxJS as Query and Orchestration Language](./rxjs-query-and-orchestration.md)
- [Flattening Operators as Concurrency Policies](../patterns/flattening-operators-concurrency-policies.md)
- [Subscription as Control Surface](./subscription-control-surface.md)
