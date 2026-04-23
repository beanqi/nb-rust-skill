---
name: nb-rust
description: coding, code review, and performance-oriented implementation workflow for codex-style tasks. use when working in a codebase to clarify requirements, propose a simple design, plan a minimal diff, and produce direct code or patch-ready edits for new features, bug fixes, reviews, refactors, or performance tuning. prioritize simplicity, readability, small diffs, no unit tests, necessary comments only, and high-performance rust with careful thread models, low lock contention, low allocation pressure, and minimal string churn.
---

# NB Rust

## Overview

Implement and review Rust code with the simplest correct design and the smallest practical diff.
Favor clear code, direct edits, and performance-aware decisions over abstraction, ceremony, or speculative extensibility.

## Default workflow

1. Clarify the requirement first.
   - Ask focused questions when behavior, performance targets, or boundaries are unclear.
   - Keep the questions short and concrete.
   - If the repository context is already enough, state the assumptions briefly and continue instead of blocking.

2. Propose a simple design before editing code.
   - Explain the chosen approach briefly.
   - Cover data flow, thread model, shared state, and hot-path concerns when relevant.
   - Mention why the simpler approach was preferred.
   - Do not design for hypothetical future extension.

3. Present a minimal change plan.
   - Name the files or modules that should change.
   - Reuse existing patterns, helpers, components, and conventions first.
   - Keep the diff small and avoid unrelated cleanup.

4. Implement directly.
   - Produce concrete code, patch-ready edits, or replacement snippets.
   - Add only the comments needed to explain non-obvious behavior, invariants, or concurrency assumptions.
   - Do not add unit tests unless the user explicitly asks.

## Output format

Use this default structure unless the user asks for something else.

### Requirement clarification
- Ask concise questions, or list brief assumptions if the task is already actionable.

### Simple design
- State the chosen approach.
- State the thread or concurrency model if relevant.
- State the main performance considerations if relevant.
- State what complexity was intentionally avoided.

### Minimal change plan
- List the smallest set of code changes needed.
- Call out any crate or abstraction additions explicitly before using them.

### Implementation
- Provide direct code or patch-ready edits.
- Keep prose shorter than the code whenever possible.

## Core rules

- Reuse existing code patterns before introducing new ones.
- Prefer concrete functions and structs over new traits, macros, builders, or generic layers.
- Ask the user before introducing a new abstraction, reusable helper layer, or new dependency, unless the need is already obvious from repeated code or existing project conventions.
- Preserve the current working architecture unless the user explicitly asks to refactor.
- Avoid speculative error handling, fallback logic, or extension points that the current task does not need.
- Keep naming stable. Avoid wide renames and format churn.
- Keep comments sparse and useful.
- Match the user's language for explanation. Keep code, APIs, and Rust syntax in English.

## Rust performance rules

- Choose the thread model deliberately before coding.
- Prefer ownership partitioning and local state over shared mutable state.
- Minimize lock count, lock scope, and lock hold time.
- Avoid nested locks and broad critical sections.
- Prefer message passing when it keeps the design simpler and reduces contention.
- For concurrent maps or shared key-value state, prefer sharded ownership patterns or `DashMap` over a single `Mutex<HashMap<...>>` when the shared concurrent access pattern is real and the crate is already available or clearly justified.
- For channels, prefer `crossbeam-channel` over the standard library channel types when low-latency message passing is needed and the crate is already available or clearly justified.
- Avoid unnecessary `Arc` cloning, `String` creation, `format!`, and temporary allocations in hot paths.
- Prefer borrowed data like `&str`, slices, iterators, or reused buffers when this keeps the code simple.
- Avoid repeated parsing, hashing, conversion, or lookup work inside loops when the result can be cached or hoisted once.
- Prefer straightforward data structures. Use the standard library when local, single-threaded ownership is enough.
- Do not add `unsafe` for speculative speedups. Use it only when truly necessary and explain the invariants clearly.

## Task-specific guidance

### For new features or bug fixes
- Start from the narrowest possible change that solves the actual problem.
- Fit the implementation into the existing module layout.
- Prefer simple control flow and explicit state transitions.

### For code review or optimization
- Identify correctness risks first, then latency or throughput issues, then maintainability issues.
- Point out lock contention, unnecessary allocation, clone-heavy paths, repeated string work, repeated map lookups, and overly broad async or threading boundaries.
- Recommend the smallest patch that materially improves the code.

### For design-only requests
- Stop after the design if the user asks only for design.
- Offer code only when the user asks for implementation or when code is clearly expected.

## What not to do

- Do not write unit tests by default.
- Do not add abstraction just because it might help later.
- Do not refactor unrelated modules.
- Do not replace working code with a broad rewrite when a local change is enough.
- Do not add a new crate or helper layer silently.
- Do not run code formatting commands (for example, cargo fmt) unless the user explicitly asks.
- Do not pad the answer with generic Rust advice that does not change the implementation.

## Finishing touch

When useful, end with a very short validation note such as:
- what to compile or inspect
- what hot path or concurrency assumption to verify
- what edge case the change now handles
