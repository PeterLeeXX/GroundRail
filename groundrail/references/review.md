# Review

Read this file only for a Fact, Plan, or Code Review. Review is an adversarial, read-only check of a stable artifact; the main agent still owns the final judgment.

## When to review

Review only:

- an independently delivered, persisted, or high-risk Fact;
- a complete Plan, including a user-supplied plan that may be executed; or
- a stable, representative diff after relevant verification.

A low-risk inline Fact may be reviewed with the following Plan. Reading a README, repository instructions, source documentation, blank templates, or intermediate saves does not trigger Review.

Use a clean context without the main agent's reasoning history. The Reviewer stays read-only. If the host cannot provide independent context, label any isolated recheck as a fallback, stop at a mandatory checkpoint, and do not claim that Review passed.

## Review package

Provide only the minimum complete contract:

- the original request, acceptance criteria, non-goals, and user decisions;
- the review type, stable artifact, and boundary;
- relevant Facts and sources; for code, the reviewed Plan or equivalent contract;
- repository root, repository instructions, and permitted adjacent-code scope; and
- commands actually run, their results, and known verification gaps.

Do not provide the main agent's diagnosis, suspected defects, expected conclusion, or proposed fix.

## Review axes

- **Contract / Intent:** Does the artifact satisfy the request, acceptance criteria, scope, and user decisions?
- **Correctness / Safety:** Are logic, boundaries, failure modes, regressions, security, and data integrity sound?
- **Repository Shape / Code health:** Does it reuse existing work and avoid duplication, needless abstraction, coupling, and scope drift?
- **Verification:** Was evidence actually produced, does it exercise the target behavior, and does it support the claimed scope?

Add type-specific checks:

- **Fact:** traceable sources, bounded negative findings, explicit gaps, and sufficient evidence for the next stage.
- **Plan:** conclusions supported by Facts, causes separated from hypotheses, reuse points, actionable steps, focused verification, and unresolved user decisions.
- **Code:** conformance to the reviewed Plan, regressions in touched areas, and undeclared changes to public contracts, data, dependencies, storage, or concurrency.

## Findings

- `P0`: blocking correctness, security, authorization, or data failure.
- `P1`: important defect, concrete regression risk, critical omission, or unnecessary reinvention.
- `P2`: low-impact robustness, maintainability, or style improvement; report at most three.

Report only evidenced, actionable findings. Missing context is not automatically a defect.

```text
Adversarial read-only review. Determine where ARTIFACT fails against CONTRACT; do not summarize or modify it.
You may inspect referenced and adjacent code within the supplied repository boundary. Report only evidenced, actionable findings.
For each finding provide: severity, review axis, location/evidence, impact, smallest repair direction, and confidence.
If no issue is found, state that no P0/P1 was found. For Fact Review, also list covered and uncovered areas and whether any gap blocks the target stage.
```

## Arbitration

The main agent classifies every finding as `accept`, `reject`, `needs-context`, or `user-decision`, with a short reason. Resolve accepted P0/P1 items within the existing authorization; report P2 by default. A read-only review never fixes code.

After a material repair, review the new stable artifact once. Do not re-review an unchanged artifact. Review passes only when no P0/P1 remains unresolved, every `needs-context` item is classified, and boundary-changing `user-decision` items are decided.
