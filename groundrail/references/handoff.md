# Handoff

Read this file when work must continue in another session or agent. A Handoff preserves the minimum state needed to resume safely; it is not a conversation summary and does not replace Facts, Plans, Reviews, issues, commits, or final deliverables.

## When to create one

Create a Handoff when the user requests it, another context must continue the task, or unfinished work must stop because of authorization, external state, verification failure, or a context boundary. Do not create one for completed work with no continuation action.

Before writing, inspect only the stable state needed to resume:

- confirmed goal, acceptance criteria, non-goals, and continuation focus;
- current Fact, Plan, Research, Review, issue, commit, diff, and verification state;
- repository, branch, base commit, relevant uncommitted changes, and unrelated user changes; and
- user decisions, modification authorization, pending decisions, blockers, and unknowns.

Fill only gaps that would misdirect the next agent. Do not re-investigate the task for completeness.

## File rules

- Create one Markdown file in the host-resolved operating-system temporary directory, outside the workspace.
- Do not hardcode `/tmp`, `$HOME`, usernames, or platform-specific paths.
- Prefer `groundrail-handoff-<task-slug>-<timestamp>.md`; keep the slug non-sensitive.
- Never overwrite an unknown file; choose a new name on collision.
- Return the absolute path and a one-line status. Do not paste the document into the final response.

## Content rules

- Reference existing paths, commits, diffs, issues, logs, or URLs instead of copying their contents.
- Record only evidence-backed completion. Mark unstable work as in progress.
- Record real commands as `PASS`, `FAIL`, or `NOT RUN`; include only the first actionable failure or a log reference.
- State the exact modification authorization and whether it remains valid. A materially changed Plan invalidates old authorization unless the user explicitly covered that change.
- Separate task changes from unrelated user changes that must be preserved.
- Timestamp volatile external observations and cite their source.
- Start next actions with one verifiable step and include conditions that require Fact, Plan, or user decision to reopen.

## Required structure

Adapt optional details to the task, but retain every top-level section.

```markdown
# Handoff — <task>

- Created: <timestamp with timezone>
- Continuation goal: <target outcome>
- Current stage/state: <Fact | Plan | awaiting authorization | Execute | Review | blocked>
- Stop reason: <reason, or planned handoff>

## Contract

- Acceptance: <confirmed acceptance criteria>
- Non-goals: <explicit exclusions>
- Continuation focus: <the user's requested focus>

## Decisions and authorization

- Confirmed decisions: <still-valid decisions>
- Modification authorization: <scope and validity, or None>
- Pending decisions: <boundary-changing decisions, or None>

## Current state

- Completed: <evidenced work>
- In progress: <unstable work, or None>
- Remaining: <unfinished outcome>
- Blockers / unknowns: <items and impact, or None>

## Authoritative artifacts

| Type | Path / commit / URL | Purpose and status |
| --- | --- | --- |
| <Fact, Plan, Research, Review, Diff, Issue> | <reference> | <why to read it> |

## Repository state

- Repository/worktree: <safe locator>
- Branch and base: <branch and commit, or None>
- Relevant changes: <task files and status>
- Unrelated user changes: <changes to preserve, or None>

## Verification

| Command or check | Result | Evidence / failure |
| --- | --- | --- |
| <actual command> | PASS / FAIL / NOT RUN | <result or reference> |

## Next actions

1. <first verifiable action>
2. <next minimal action>

- Stop and ask when: <authorization, product, fact, or approach boundary>

## Suggested skills

- <exact skill name and purpose, or None>

## Safety and context boundaries

- <sensitive data, external writes, destructive actions, scope, or ownership limits>
```

## Sensitive information

Never include secrets, tokens, cookies, private keys, full connection strings, or personal data. Refer only to a safe environment-variable name, secret-manager reference, or a need to reauthorize. Redact sensitive values from commands, errors, URLs, and paths. If an absolute path exposes identity, use a repository-relative path, generalized prefix, or repository URL.

A Handoff does not transfer or expand authority. The next agent must still obey the original scope and host permissions.

## Final check

- The file is outside the workspace, unique, and non-overwriting.
- Goal, stage, stop reason, authorization, remaining work, and first next action are explicit.
- Artifacts are referenced rather than copied.
- Verification status matches actual results.
- Task changes and unrelated user changes are separated.
- `Suggested skills` uses exact names or `None`.
- No sensitive value or identifying path remains.

Do not trigger an independent Review solely for a Handoff unless the user asks or the handoff concerns high-risk authorization, data, or production state.
