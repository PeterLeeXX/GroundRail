<p align="center">
  <img src="./groundrail.svg" width="180" alt="GroundRail logo" />
</p>

<h1 align="center">GroundRail</h1>

<p align="center">An evidence ground and lightweight rails for strong coding models.</p>

<p align="center">
  <a href="./README.md">简体中文</a> · <a href="./README_EN.md">English</a>
</p>

[![][github-forks-shield]][github-forks-link]
[![][github-stars-shield]][github-stars-link]
[![][github-issues-shield]][github-issues-link]
[![][agent-skills-shield]][agent-skills-link]

GroundRail is designed for strong foundation models such as Sol, Claude Opus 4.8, and GLM-5.3 that have already internalized much of the coding process. It does not reteach them how to read code, break down work, write tests, or implement a feature, and it does not try to replace model judgment with an exhaustive operating manual. What these models usually need is a small set of explicit, inspectable boundaries at the points where capable work still tends to derail.

**Ground** is a traceable, reproducible evidence base. **Rail** is the minimum constraint on stage order, decision ownership, and review boundaries. The rails define when work may proceed and which choices remain with the user; they do not prescribe how every line of code must be written. The model still owns repository comprehension, implementation choices, reuse, and routine engineering detail.

### 🛤️ Five common derailments

> [!IMPORTANT]
> GroundRail does not constrain how a model writes code. It brings the task back on track only when evidence, assumptions, decision ownership, review, or context begins to drift.

#### `01` Partial evidence, premature solution

**Failure mode** — The agent reads only part of the relevant code, then starts explaining the root cause or designing a solution.<br>
**Rail** — Establish the minimum sufficient `Fact` for the risk first, keeping every important conclusion traceable to code, configuration, runtime results, or external contracts.

#### `02` Assumptions drift and harden

**Failure mode** — A guess from investigation enters the Plan, then becomes a “fact” during Execute.<br>
**Rail** — Keep observations, evidence-backed conclusions, and actions separate. Unknowns remain evidence gaps instead of being silently laundered by the workflow.

#### `03` The agent crosses the decision boundary

**Failure mode** — The agent chooses product behavior, scope, cost, safety, or an irreversible risk tradeoff on the user's behalf.<br>
**Rail** — Leave consequential tradeoffs to the user while the model autonomously handles routine implementation details from evidence and repository conventions.

#### `04` Review loses focus

**Failure mode** — Review misses issues that threaten delivery while repeatedly debating style or low-value suggestions.<br>
**Rail** — Review only stable artifacts, in a clean context, across four axes; use P0/P1/P2 to control priority and noise.

#### `05` Context accumulates and distorts judgment

**Failure mode** — A long task keeps exploration history and implementation detail in the main conversation until stale context pulls later decisions off course.<br>
**Rail** — Externalize state through stable artifacts, result-oriented delegation, and safe Handoff, preserving the main agent's context for continuous judgment, conflict resolution, and final synthesis.

The core path is `Fact → Plan → Execute`, with Review crossing it at stable checkpoints. These are logical gates, not document rituals: a low-risk task may need only a few inline facts and plan notes, while complex, long-running, multi-agent, or cross-session work may persist artifacts.

The project and Skill are both named **GroundRail**. Repository investigation, planning, implementation, code-review, and handoff requests can route to it automatically. Invoke it explicitly with `$groundrail` in Codex or `/groundrail` in Claude Code. **Ground the agent in facts, then use the fewest rails that let a strong model keep exercising its ability.**

## What it deliberately does not do

GroundRail is not a heavyweight methodology for the entire software-development lifecycle. It does not default to mandatory brainstorming, one subagent per task, 2–5 minute task decomposition, strict TDD, worktrees, frequent review, or a fixed branch-finishing process. The model, repository rules, or specialist Skills can add those practices when the task calls for them.

Nor does it bounce ordinary implementation choices back to the user. Only decisions that alter product behavior, scope, acceptance, cost, safety, or irreversible state require user input. The model should resolve everything else from evidence and repository conventions. GroundRail constrains authority drift and evidence distortion, not capability.

## Installation

GroundRail follows the open Agent Skills structure, so the same `groundrail/SKILL.md` works in Codex and Claude Code. It uses only the standard `name`, `description`, and Markdown instructions. Optional Codex UI metadata lives separately in `groundrail/agents/openai.yaml` and does not affect Claude Code.

### Recommended: install for Codex and Claude Code

Create both hosts' user-level Skill directories, then install with the [Skills CLI](https://github.com/vercel-labs/skills):

```bash
mkdir -p "$HOME/.agents/skills" "$HOME/.claude/skills"
npx skills add PeterLeeXX/GroundRail \
  --skill groundrail \
  --agent codex \
  --agent claude-code \
  --global \
  --copy \
  --yes
```

To install for only one host:

```bash
npx skills add PeterLeeXX/GroundRail --skill groundrail --agent codex --global --copy --yes
npx skills add PeterLeeXX/GroundRail --skill groundrail --agent claude-code --global --copy --yes
```

Verify that both hosts recognize the Skill:

```bash
npx skills list --global --agent codex --agent claude-code
```

If the CLI reports `Agents: not linked`, or GroundRail is not visible in a host, use the native-directory installation below. This also avoids linking differences between CLI versions.

### Native-directory installation

Clone the repository first:

```bash
git clone --depth 1 https://github.com/PeterLeeXX/GroundRail.git
```

Codex uses `~/.agents/skills/` for user-level Skills:

```bash
mkdir -p "$HOME/.agents/skills/groundrail"
cp -R GroundRail/groundrail/. "$HOME/.agents/skills/groundrail/"
```

Claude Code uses `~/.claude/skills/` for user-level Skills:

```bash
mkdir -p "$HOME/.claude/skills/groundrail"
cp -R GroundRail/groundrail/. "$HOME/.claude/skills/groundrail/"
```

For project-level installation, use the same structure under `.agents/skills/groundrail/` and `.claude/skills/groundrail/` at the repository root.

Codex normally detects Skill changes automatically. If GroundRail does not appear, restart Codex, check `/skills`, or invoke `$groundrail`. Claude Code watches existing Skill directories; if this installation created the top-level `.claude/skills/` directory, restart Claude Code and invoke `/groundrail`. Both hosts can also trigger GroundRail from a matching natural-language request, and unrelated conversations are left alone.

## Usage

The following requests do not need to name Codex's `$groundrail` or Claude Code's `/groundrail` explicitly:

Investigate without modifying code:

```text
Investigate this repository issue. Stop after Fact and do not modify code.
```

Plan from an existing fact artifact:

```text
Review ./facts.md. If it passes, create plan.md. Do not implement.
```

Execute an existing plan:

```text
Review ./plan.md. Confirm the facts and plan, then request my authorization to implement it.
```

Review an existing diff:

```text
Review the current diff. Report P0 / P1 / P2 findings. Read-only; do not fix.
```

Hand off the current task:

```text
Hand this task to the next session, focusing on the failing verification, and create a safe handoff document.
```

## Five rails

A complex implementation task follows this full path:

```text
Fact → Review → Plan → Review → user approval → Execute → Review
```

The path contracts with risk instead of running mechanically. A low-risk Fact can stay inline and be covered by Plan Review; a simple change does not need a directory of task documents; and the user may request only Fact, Plan, or a read-only Review. GroundRail requires real stage boundaries, not a file from every stage.

### 1. Intuition cannot substitute for facts

Fact contains only goal-relevant observations that are observable and locatable. Evidence depth matches risk: bugs need a signal that reaches the symptom, external behavior needs the actual version and primary contract, and UI, security, and asynchronous work need runtime, trust-boundary, and progress evidence respectively. Negative results name their search boundary, while unknowns stay gaps rather than plausible guesses.

### 2. A plan cannot launder assumptions

Plan follows a completed Fact and separates observations, evidence-backed conclusions, and hypotheses still awaiting proof. It describes the smallest coherent change, existing reuse points, and minimum sufficient verification without prewriting large implementation blocks. Product behavior, scope, acceptance, cost, safety, and irreversible tradeoffs go to the user; naming, function boundaries, and ordinary repository-aligned choices stay with the model.

### 3. Execution cannot cross the authorization boundary

Execute starts only after the plan is reviewed and the user authorizes it. Within that boundary the model implements, verifies, and simplifies touched code autonomously, without opportunistic scope expansion. New facts that materially change requirements or safety reopen Fact; a major approach change reopens Plan and Review. The rail constrains what may change without scripting every step of how to code it.

### 4. Review must target a stable artifact

Review is not a fixed stage and does not run after every save. A clean-context Reviewer examines a stable Fact, Plan, or representative diff without inheriting the implementer's conclusions, across Contract / Intent, Correctness / Safety, Repository Shape / Code health, and Verification:

| Priority | Meaning | Handling |
| --- | --- | --- |
| P0 | Blocking issue, major defect, critical logic error, security or data-integrity problem | Fix and re-review during implementation; report only in a read-only review |
| P1 | Important defect or risk, clear omission, duplicate implementation, requirement or plan mismatch | Fix and re-review during implementation; report only in a read-only review |
| P2 | Non-critical validation, limited robustness, style, or low-impact improvement | Report at most three; apply selectively during implementation and never modify in a read-only review |

The Reviewer supplies evidence; the main agent arbitrates each finding. P2 is capped at three so style advice cannot bury real risk. If the host cannot create an independent Reviewer, GroundRail labels the fallback as degraded and stops at the checkpoint instead of presenting same-context self-review as an independent pass.

### 5. Long tasks must externalize state

The main agent retains continuous judgment, conflict resolution, and final synthesis. It delegates only clearly bounded heavy work such as time-consuming research, wide repository scans, independent hypotheses, mechanical changes, or independent verification. Subagents return results by default rather than pouring their entire exploration history into the controller conversation.

Across sessions, Handoff keeps only the state needed to resume and references existing specs, plans, ADRs, issues, commits, diffs, and research artifacts instead of duplicating them. It is written to the OS temporary directory and redacts sensitive data, so a context snapshot does not become permanent repository noise.

## Scope

GroundRail is most useful where capability is already strong but boundaries still drift:

- existing-repository changes spanning several modules, configuration files, or data sources;
- work where versions, runtime state, or external contracts can invalidate assumptions;
- tasks that should stop after an investigation or implementation plan;
- work handed across sessions or agents where context pollution matters;
- tasks where product, scope, and risk decisions must remain with the user.

It does not replace or suppress specialist security, performance, testing, UI, or release Skills. Small, low-risk tasks with clear facts do not need documents created solely for process compliance; high-risk work or stricter repository rules can layer on the relevant specialist constraints.

## Comparison with Superpowers

[Superpowers](https://github.com/obra/superpowers) is a complete development methodology covering brainstorming, worktrees, planning, TDD, subagent-driven development, code review, and branch delivery. It fits teams that want an agent to follow a detailed development method. GroundRail starts from a different assumption: the model already knows most of the coding workflow and needs rails primarily around evidence, stages, authorization, review, and context.

**🧠 Starting assumptions differ.** GroundRail assumes the model is already a strong coder and treats evidence distortion and boundary drift as the main risks. Superpowers turns development into explicit, fine-grained operating steps through a complete methodology.

**🚦 Activation and starting points differ.** GroundRail routes automatically for investigation, planning, implementation, code review, or handoff intents; it remains explicitly available as `$groundrail` in Codex and `/groundrail` in Claude Code, and starts from a fact baseline. Superpowers checks and routes related Skills from the beginning of a conversation, classifies the task, and clarifies a design through brainstorming.

**📝 Planning granularity differs.** GroundRail records concise locations, reuse points, key logic, and verification without prewriting large code blocks. A Superpowers architectural plan can use 2–5 minute tasks and include more complete implementation instructions.

**⚙️ Implementation orchestration differs.** GroundRail keeps execution of the reviewed plan with the main agent by default and delegates only clearly bounded independent heavy work. Superpowers may dispatch a new subagent per task, followed by separate specification and code-quality reviews.

**🧪 Testing constraints differ.** GroundRail selects the minimum sufficient evidence for the task's specific risk. Superpowers defaults to strict TDD for features, fixes, refactors, and behavior changes.

**🔍 Review cadence differs.** GroundRail reviews stable Facts, Plans, representative diffs, and final states so frequent review does not create noise. Superpowers emphasizes earlier and more frequent review across task, major-feature, and pre-merge boundaries.

**🙋 User gates differ.** GroundRail requires approval of the reviewed plan or explicit advance authorization before code changes. Superpowers begins plan-driven implementation after design approval.

Superpowers provides broader coverage when a project wants a complete methodology, mandatory TDD, worktrees, and branch finishing. GroundRail fits Sol, Claude Opus 4.8, GLM-5.3, and similar strong models when the goal is to preserve autonomous engineering judgment while reducing missed evidence, unauthorized decisions, context pollution, and unproductive review.

This comparison uses `obra/superpowers` commit [`b36e082`](https://github.com/obra/superpowers/tree/b36e0829c6d0140e93cfef2ca599b1b07d4a7797), accessed 2026-08-18. Refer to its latest documentation for current behavior.

## Published layout

Only the following runtime and explanatory files are needed for publication. `upstream/` and design notes are development references, not Skill runtime content.

```text
groundrail/
├── README.md
├── README_EN.md
├── groundrail.svg
└── groundrail/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

## Design references

- [Agent Skills standard](https://agentskills.io)
- [Anthropic Skills](https://github.com/anthropics/skills)
- [Vercel Agent Skills](https://github.com/vercel-labs/agent-skills)
- [Hugging Face Skills](https://github.com/huggingface/skills)
- [Thoughtworks: Agent instruction bloat](https://www.thoughtworks.com/radar/techniques/agent-instruction-bloat)
- [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)

## Contributing

Open an [Issue][github-issues-link] for evidence-coverage gaps, incorrect gates, or review loops that add no value. Useful proposals include a reproducible task or clear before/after behavior. New standing rules should solve an observed failure rather than make the workflow look more complete.

[github-forks-shield]: https://img.shields.io/github/forks/PeterLeeXX/GroundRail?style=flat-square&logo=github&color=2D565E
[github-forks-link]: https://github.com/PeterLeeXX/GroundRail/network/members
[github-stars-shield]: https://img.shields.io/github/stars/PeterLeeXX/GroundRail?style=flat-square&logo=github&color=2D565E
[github-stars-link]: https://github.com/PeterLeeXX/GroundRail/stargazers
[github-issues-shield]: https://img.shields.io/github/issues/PeterLeeXX/GroundRail?style=flat-square&logo=github&color=2D565E
[github-issues-link]: https://github.com/PeterLeeXX/GroundRail/issues
[agent-skills-shield]: https://img.shields.io/badge/Agent%20Skills-compatible-2D565E?style=flat-square
[agent-skills-link]: https://agentskills.io
