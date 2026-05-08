---
name: nb-rust
description: Use for Rust coding, code review, bug fixes, refactors, and performance-aware implementation in existing codebases.
---

# NB Rust

## Goal

Help Codex make Rust changes in existing repositories with a simple design, a focused diff, and clear performance awareness.

## Implementation Rules

- Prefer the existing architecture, naming, helpers, and crate choices.
- Keep code and architecture simple. Add abstractions only when they clearly reduce complexity or match the surrounding code.
- Preserve public APIs unless the task requires changing them.
- Avoid unrelated refactors, broad rewrites, formatting-only churn, and speculative future-proofing.
- Do not run formatting commands such as `cargo fmt`, `rustfmt`, or other `fmt` tools unless the user explicitly asks.
- Do not add new dependencies, background tasks, global state, traits, macros, or builders without a clear reason.
- Keep implementation diffs narrow.
- Do not create unit tests by default.
- Add practical Chinese comments for important modules, functions, fields, invariants, concurrency assumptions, unsafe requirements, or non-obvious performance decisions.
- Do not comment obvious syntax or restate what each line already says.

## Rust Performance Guidance

- Treat ownership, allocation, locking, async boundaries, and thread boundaries deliberately.
- Avoid unnecessary `String`, `format!`, `to_string`, `clone`, temporary `Vec`, repeated parsing, repeated hashing, and repeated map lookups on hot paths.
- Prefer borrowed data, slices, iterators, local ownership, and reused buffers when this stays readable.
- Keep lock scope short, avoid nested locks, and do not hold locks across `.await` unless the code is explicitly designed for it.
- Prefer standard-library data structures first. Use specialized concurrent structures or new crates only when the need is clear.
- Do not make cold paths harder to read for minor performance gains.

## Review Guidance

For code review, prioritize:

1. Correctness issues, panics, lost errors, data races, deadlocks, and API contract violations.
2. Real performance risks such as allocation churn, clone-heavy hot paths, broad locks, blocking in async, and avoidable repeated work.
3. Maintainability issues that directly affect the current change.

For each finding, include the concrete risk and the smallest practical fix. Avoid generic Rust advice.

## Final Handoff

End with:

- What changed or what was found.
- What validation was run, or what remains to run.
- Any important performance, ownership, locking, or API assumption the user should verify.
