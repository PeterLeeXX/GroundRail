<p align="center">
  <img src="./groundrail.svg" width="180" alt="GroundRail logo" />
</p>

<h1 align="center">GroundRail</h1>

<p align="center">
  <strong>An evidence ground and lightweight rails for strong coding models.</strong>
  <br />
  <sub>Constrain evidence, stages, and decision boundaries—not engineering judgment.</sub>
</p>

<p align="center">
  <a href="https://github.com/PeterLeeXX/GroundRail/tags"><img alt="Version" src="https://img.shields.io/github/v/tag/PeterLeeXX/GroundRail?label=Version&sort=semver&style=flat-square&color=2496ED" /></a>
  <a href="https://github.com/PeterLeeXX/GroundRail/stargazers"><img alt="GitHub Stars" src="https://img.shields.io/github/stars/PeterLeeXX/GroundRail?style=flat-square&logo=github&color=F5B700" /></a>
  <a href="./LICENSE"><img alt="License: MIT" src="https://img.shields.io/github/license/PeterLeeXX/GroundRail?style=flat-square&label=License&color=2EA44F" /></a>
  <a href="https://agentskills.io"><img alt="Agent Skills Compatible" src="https://img.shields.io/badge/Agent%20Skills-compatible-8B5CF6?style=flat-square" /></a>
</p>

<p align="center">
  <a href="./README.md">English</a> · <a href="./README_ZH.md">简体中文</a>
</p>

<p align="center">
  <code>Fact → Plan → Execute</code> · stable-artifact Review · safe Handoff
  <br />
  <sub>Codex · Claude Code · Agent Skills compatible hosts</sub>
</p>

## ⚡ Quick install

```bash
npx skills add PeterLeeXX/GroundRail --skill groundrail -a codex -a claude-code -g --copy -y
```

This installs GroundRail for Codex and Claude Code. See the [full installation guide](#installation) for single-host, project-level, and native-directory options.

## 🚧 Why GroundRail?

GroundRail is designed for strong foundation models such as Sol, Claude Opus 4.8, and GLM-5.3 that have already internalized much of the coding process. It does not reteach them how to read code, break down work, write tests, or implement a feature, and it does not try to replace model judgment with an exhaustive operating manual. What these models usually need is a small set of explicit, inspectable boundaries at the points where capable work still tends to derail.

**Ground** is a traceable, reproducible evidence base. **Rail** is the minimum constraint on stage order, decision ownership, and review boundaries. The rails define when work may proceed and which choices remain with the user; they do not prescribe how every line of code must be written. The model still owns repository comprehension, implementation choices, reuse, and routine engineering detail.

### 🛤️ Five common derailments

GroundRail does not constrain how a model writes code. It brings the task back on track only when evidence, assumptions, decision ownership, review, or context begins to drift.

> #### `01` Partial evidence, premature solution
>
> **Failure mode** — The agent reads only part of the relevant code, then starts explaining the root cause or designing a solution.<br>
> **Rail** — Establish the minimum sufficient `Fact` for the risk first, keeping every important conclusion traceable to code, configuration, runtime results, or external contracts.

> #### `02` Assumptions drift and harden
>
> **Failure mode** — A guess from investigation enters the Plan, then becomes a “fact” during Execute.<br>
> **Rail** — Keep observations, evidence-backed conclusions, and actions separate. Unknowns remain evidence gaps instead of being silently laundered by the workflow.

> #### `03` The agent crosses the decision boundary
>
> **Failure mode** — The agent chooses product behavior, scope, cost, safety, or an irreversible risk tradeoff on the user's behalf.<br>
> **Rail** — Leave consequential tradeoffs to the user while the model autonomously handles routine implementation details from evidence and repository conventions.

> #### `04` Review loses focus
>
> **Failure mode** — Review misses issues that threaten delivery while repeatedly debating style or low-value suggestions.<br>
> **Rail** — Review only stable artifacts, in a clean context, across four axes; use P0/P1/P2 to control priority and noise.

> #### `05` Context accumulates and distorts judgment
>
> **Failure mode** — A long task keeps exploration history and implementation detail in the main conversation until stale context pulls later decisions off course.<br>
> **Rail** — Externalize state through stable artifacts, result-oriented delegation, and safe Handoff, preserving the main agent's context for continuous judgment, conflict resolution, and final synthesis.

The project and Skill are both named **GroundRail**. Repository investigation, planning, implementation, code-review, and handoff requests can route to it automatically. Invoke it explicitly with `$groundrail` in Codex or `/groundrail` in Claude Code. **Ground the agent in facts, then use the fewest rails that let a strong model keep exercising its ability.**

## 🧭 How it works

GroundRail separates what was observed, what was concluded, and what may be changed:

```text
Fact → Review → Plan → Review → user approval → Execute → Review
```

This is a risk-aware path, not a document factory. A low-risk task may keep Fact and Plan inline; a complex, long-running, multi-agent, or cross-session task may persist them as stable artifacts.

### 1. Fact — establish what is true

Collect the minimum sufficient evidence for the risk. Important observations stay traceable; negative results name their search boundary; unknowns remain gaps instead of becoming plausible answers.

### 2. Plan — turn evidence into the smallest coherent change

Separate evidence-backed conclusions from hypotheses, identify existing code to reuse, and define focused verification. Product behavior, scope, cost, safety, and irreversible tradeoffs stay with the user; routine engineering choices stay with the model.

### 3. Execute — act inside an authorized boundary

Implementation starts from a reviewed and authorized Plan. The model may code, verify, and simplify touched areas autonomously, but it does not expand scope. Material new facts reopen Fact; a major approach change reopens Plan and authorization.

### 4. Review — challenge stable artifacts, not every save

A clean-context Reviewer checks Contract / Intent, Correctness / Safety, Repository Shape / Code health, and Verification. Findings are bounded by severity: `P0` blocks, `P1` identifies important defects or omissions, and `P2` is limited to three low-impact improvements. The Reviewer supplies evidence; the main agent arbitrates it.

### 5. Handoff — externalize state without copying the conversation

The main agent retains judgment and synthesis while bounded heavy work can be delegated. Cross-session Handoff references existing plans, diffs, commits, and research instead of duplicating them, preserving the next context for decisions rather than stale implementation history.

## 🪶 What GroundRail leaves to the model

GroundRail is not a heavyweight methodology for the entire software-development lifecycle. It does not default to mandatory brainstorming, one subagent per task, 2–5 minute task decomposition, strict TDD, worktrees, frequent review, or a fixed branch-finishing process. The model, repository rules, or specialist Skills can add those practices when the task calls for them.

Nor does it bounce ordinary implementation choices back to the user. Only decisions that alter product behavior, scope, acceptance, cost, safety, or irreversible state require user input. The model should resolve everything else from evidence and repository conventions. GroundRail constrains authority drift and evidence distortion, not capability.

<a id="installation"></a>

## ⚙️ Installation

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

## 💬 Usage

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

## 🎯 Where it fits

GroundRail is most useful where capability is already strong but boundaries still drift:

- existing-repository changes spanning several modules, configuration files, or data sources;
- work where versions, runtime state, or external contracts can invalidate assumptions;
- tasks that should stop after an investigation or implementation plan;
- work handed across sessions or agents where context pollution matters;
- tasks where product, scope, and risk decisions must remain with the user.

It does not replace or suppress specialist security, performance, testing, UI, or release Skills. Small, low-risk tasks with clear facts do not need documents created solely for process compliance; high-risk work or stricter repository rules can layer on the relevant specialist constraints.

## ⚖️ GroundRail vs Superpowers

[Superpowers](https://github.com/obra/superpowers) is a complete development methodology covering brainstorming, worktrees, planning, TDD, subagent-driven development, code review, and branch delivery. It fits teams that want an agent to follow a detailed development method. GroundRail starts from a different assumption: the model already knows most of the coding workflow and needs rails primarily around evidence, stages, authorization, review, and context.

### Choose GroundRail when

- the model is already a strong coder and should retain routine engineering judgment;
- evidence gaps, authority drift, review noise, and context pollution are the main risks;
- plans should describe coherent changes rather than script every implementation step;
- testing and delegation should scale with the task's actual risk.

### Choose Superpowers when

- you want a complete, prescriptive development methodology;
- brainstorming, fine-grained plans, strict TDD, worktrees, and frequent subagent review should be defaults;
- the process itself should guide most implementation behavior.

The two can inform each other, but they optimize for different control surfaces: Superpowers specifies more of the development method; GroundRail protects the boundaries around a capable model's judgment. Refer to the latest upstream documentation for current Superpowers behavior.

## 📦 Repository layout

Only the following runtime and explanatory files are needed for publication. `upstream/` and design notes are development references, not Skill runtime content.

```text
groundrail/
├── README.md
├── README_ZH.md
├── groundrail.svg
└── groundrail/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

## 🧠 Design references

- [Agent Skills standard](https://agentskills.io)
- [Anthropic Skills](https://github.com/anthropics/skills)
- [Vercel Agent Skills](https://github.com/vercel-labs/agent-skills)
- [Hugging Face Skills](https://github.com/huggingface/skills)
- [Thoughtworks: Agent instruction bloat](https://www.thoughtworks.com/radar/techniques/agent-instruction-bloat)
- [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)

## 🤝 Contributing

Open an [Issue](https://github.com/PeterLeeXX/GroundRail/issues) for evidence-coverage gaps, incorrect gates, or review loops that add no value. Useful proposals include a reproducible task or clear before/after behavior. New standing rules should solve an observed failure rather than make the workflow look more complete.

## 📄 License

GroundRail is available under the [MIT License](./LICENSE).
