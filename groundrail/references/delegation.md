# Delegation

Read this file before using a subagent, independent context, or background researcher. Delegate only when independence, parallel throughput, or context isolation justifies the coordination cost.

## Ownership

The main agent retains stage decisions, conflict resolution, authorization interpretation, Reviewer arbitration, final synthesis, and user delivery.

Delegate bounded, expensive work such as broad investigation, independent hypothesis testing, mechanical edits, isolated implementation, or independent verification. Keep small tasks, tightly coupled flows, immediate blockers, and low-cost work in the main context.

Researcher, Reviewer, and Executor are responsibilities, not fixed agent types. Do not delegate merely to satisfy a process shape.

## Delegation contract

Each delegation must specify:

- one verifiable objective and why it merits isolation;
- required inputs, repository root, repository instructions, and read boundary;
- an explicit write boundary; default to returning results without workspace writes;
- expected output, verification, completion criteria, and stop conditions; and
- known Facts and user decisions, without a conclusion the delegate is expected to prove.

Grant only the authority required for the subtask. Existing authorization does not cover new production writes, credentials, destructive operations, external side effects, or cost.

## Files and concurrency

- Assign one exclusive file or path before allowing a delegate to write.
- Do not overwrite unknown files or let delegates modify the same or tightly coupled files concurrently.
- Stop on ownership conflicts, scope expansion, or unverifiable results.
- Inspect returned artifacts, diffs, and command results; a success summary is not evidence.

## Responsibilities

### Researcher

Use external research only when a Fact depends on a contract, version, specification, or runtime state not established in the repository. Require first-party sources, adjacent citations, and a clear separation of sourced facts, inference, and unknowns.

Write research to one exclusive Markdown file. Prefer repository conventions; otherwise use `tasks/<task-slug>/research.md`. Research is Fact input, not an automatic diagnosis or Plan.

### Reviewer

Use a clean, read-only context. Build the contract package from [Review](review.md) and omit the main agent's expected conclusion. If independent context is unavailable, follow Review's fallback and stop rules.

### Executor

Delegate execution only from a reviewed, authorized Plan with independent file ownership and explicit verification. Provide allowed files, non-goals, protected contracts, commands, and stop conditions. Return material new facts to Fact; stop for a major approach change and reopen Plan.

## Return and integration

Require a concise return containing completed work, evidence or file locations, commands and results, unresolved gaps, and triggered stop conditions. Integrate from observable artifacts, not the delegate's confidence statement.
