<p align="center">
  <img src="./groundrail.svg" width="180" alt="GroundRail logo" />
</p>

<h1 align="center">GroundRail.Skill</h1>

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

## 🚧 Why GroundRail?

> Strong coding models rarely need another tutorial on how to code. They need a reliable ground for what is true, plus lightweight rails around when to proceed, what they may decide, and how stale context is kept out of the next judgment.

GroundRail is built for strong foundation models such as Sol, Claude Opus 4.8, and GLM-5.3 that have already internalized much of the coding process. It does not reteach repository reading, task decomposition, testing, or implementation, and it does not replace model judgment with an exhaustive operating manual.

**Ground** is the traceable evidence beneath a decision. **Rail** is the minimum constraint around stage order, decision ownership, review, and context. Everything between those boundaries—repository comprehension, implementation design, reuse, and routine engineering detail—stays with the model.

### ⭕ Five common derailments

Five failures remain common even when the code itself is strong:

> **`01` · Partial evidence.** A slice of code becomes the whole story. **Rail:** establish a traceable, risk-matched `Fact` before proposing.

> **`02` · Hardened assumptions.** A guess crosses stages until it looks certain. **Rail:** separate observation, conclusion, and action; keep unknowns unknown.

> **`03` · Decision drift.** Engineering autonomy becomes product or risk authority. **Rail:** the model owns implementation; the user owns consequential tradeoffs.

> **`04` · Review noise.** Style debates hide delivery-critical defects. **Rail:** review stable artifacts in clean context and bound findings with P0/P1/P2.

> **`05` · Context drag.** Old exploration steers new judgment. **Rail:** externalize state through artifacts, bounded delegation, and Handoff.

GroundRail steps in only at these failure boundaries. Repository investigation, planning, implementation, code-review, and handoff requests can route to the Skill automatically; invoke it explicitly with `$groundrail` in Codex or `/groundrail` in Claude Code. **Ground the agent in facts, then use the fewest rails that let a strong model keep exercising its ability.**

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

<a id="installation"></a>

## ⚙️ Installation

GroundRail follows the open Agent Skills structure, so the same `groundrail/SKILL.md` works in Codex and Claude Code. It uses only the standard `name`, `description`, and Markdown instructions. Optional Codex UI metadata lives separately in `groundrail/agents/openai.yaml` and does not affect Claude Code.

### Quick install

```bash
npx skills add PeterLeeXX/GroundRail --skill groundrail -a codex -a claude-code -g --copy -y
```

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

## ⚖️ Why GroundRail fits today's strong models better than Superpowers

[Superpowers](https://github.com/obra/superpowers) and GroundRail answer two different questions about reliable agentic development:

> **Superpowers asks:** How do we make an agent follow a dependable development method?
>
> **GroundRail asks:** Once the model already knows the method, which boundaries still need protection?

### 🧠 The capability shift

**When models needed more procedural support, encode the method.** Superpowers provides a complete methodology: brainstorming, worktrees, highly detailed plans, strict red-green-refactor TDD, task-level execution and review, and branch finishing.

**With today's strongest models, protect the judgment.** They can already inspect unfamiliar repositories, choose implementations, reuse abstractions, test focused behavior, and revise an approach. Re-encoding those abilities as permanent step-by-step instructions consumes context and turns useful judgment into workflow compliance.

### 🔀 What changes in practice

- **Planning:** exhaustive implementation script → smallest coherent statement of intent, evidence, boundaries, and verification.
- **Testing:** one universal method → verification matched to the actual failure risk.
- **Delegation:** task-level subagents or batch checkpoints → delegate only bounded work that earns its context cost.
- **Review:** repeated process checkpoints → clean-context review of stable artifacts where findings can still change the outcome.
- **Context:** keep the method present → externalize task state so the main context remains available for judgment.

### 🎯 The better trade for strong models

> **Remove instructions the model no longer needs. Tighten the boundaries it still cannot safely infer.**

Superpowers controls more of *how development is performed*. GroundRail controls *what may become fact, when work may advance, who owns consequential decisions, and what context survives*. That means less standing process where capability is already high, and harder gates where failure remains expensive.

Superpowers remains a strong choice for teams that want one mandatory, end-to-end methodology. GroundRail is the better fit when the model is already capable and the goal is to constrain drift without constraining intelligence.

## 🤝 Contributing

Open an [Issue](https://github.com/PeterLeeXX/GroundRail/issues) for evidence-coverage gaps, incorrect gates, or review loops that add no value. Useful proposals include a reproducible task or clear before/after behavior. New standing rules should solve an observed failure rather than make the workflow look more complete.

GroundRail follows the [Agent Skills standard](https://agentskills.io) and is available under the [MIT License](./LICENSE).
