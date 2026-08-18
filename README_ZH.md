<p align="center">
  <img src="./groundrail.svg" width="180" alt="GroundRail logo" />
</p>

<h1 align="center">GroundRail</h1>

<p align="center">
  <strong>给强编码基模的证据地基与轻量护轨。</strong>
  <br />
  <sub>约束事实、阶段与决策边界，不接管模型的工程判断。</sub>
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
  <code>Fact → Plan → Execute</code> · 稳定工件 Review · 安全 Handoff
  <br />
  <sub>Codex · Claude Code · Agent Skills compatible hosts</sub>
</p>

## 🚧 为什么是 GroundRail？

> 强编码模型通常不再需要一份“如何写代码”的教程。它们需要的，是一块确认什么为真的可靠地基，以及几根约束“何时继续、谁来决定、哪些旧上下文不该带入下一次判断”的轻量护轨。

GroundRail 面向 Sol、Claude Opus 4.8、GLM-5.3 等已经内化了大量编码流程的强基模。它不重新教授仓库阅读、任务拆解、测试或实现，也不用一份巨细无遗的操作手册代替模型判断。

**Ground** 是支撑决策的可追溯证据。**Rail** 是围绕阶段顺序、决策归属、审查和上下文的最少约束。在这些边界之间，仓库理解、实现设计、代码复用和普通工程细节仍由模型决定。

### ⭕ 五种常见脱轨

GroundRail 不限制模型如何写代码。它只在证据、假设、决策权、审查和上下文开始偏离时，把任务拉回轨道。

> **`01` · 局部证据 → 过早方案**<br>
> Agent 只读了局部代码，就开始解释根因或设计方案。
>
> **GroundRail：**先建立与风险匹配的最小充分 `Fact`，让关键结论能追溯到源码、配置、运行结果或外部契约。

> **`02` · 猜测 → 被继承的“事实”**<br>
> 调查中的猜测进入 Plan，又在 Execute 时逐渐固化。
>
> **GroundRail：**分开观察、证据支持的结论与行动，让未知始终作为可见的证据缺口。

> **`03` · 工程判断 → 产品决策**<br>
> Agent 自行决定产品行为、范围、成本、安全或不可逆取舍。
>
> **GroundRail：**把关键取舍留给用户，普通实现选择继续交给模型。

> **`04` · 更多审查 → 更少信号**<br>
> Review 漏掉真正影响交付的问题，却反复纠缠风格和低价值建议。
>
> **GroundRail：**在干净上下文中审查稳定工件，沿四个轴检查，并用 P0/P1/P2 限制优先级与噪声。

> **`05` · 更多上下文 → 更差判断**<br>
> 长任务把探索过程和实现细节堆在主对话中，直到旧上下文牵引后续判断。
>
> **GroundRail：**用稳定工件、结果式委派和安全 Handoff 外置状态。

GroundRail 只在这些失效边界上介入。仓库调查、计划、实施、代码审查或任务交接可以自动路由到该 Skill；Codex 可用 `$groundrail`、Claude Code 可用 `/groundrail` 显式调用。**用事实让 Agent 落地，用最少护轨让强基模继续发挥。**

## 🧭 工作方式

GroundRail 把观察到的事实、基于证据形成的结论，以及获得授权后才能执行的改动明确分开：

```text
Fact → Review → Plan → Review → user approval → Execute → Review
```

这是一条随风险收缩的路径，不是文档生产线。低风险任务可以内联 Fact 和 Plan；复杂、长时、跨 Agent 或跨会话任务才需要把它们保存为稳定工件。

### 1. Fact — 建立事实地基

搜集与风险匹配的最小充分证据。重要观察保持可追溯，否定结果写明搜索边界，未知继续作为证据缺口，而不是被补成看似合理的答案。

### 2. Plan — 把证据变成最小连贯改动

分开证据支持的结论与待验证假设，识别可复用的现有实现，并定义聚焦验证。产品行为、范围、成本、安全和不可逆取舍留给用户；普通工程选择交给模型。

### 3. Execute — 只在授权边界内行动

实施从已审查且获授权的 Plan 开始。模型可以自主编码、验证和简化触达区域，但不扩大范围。实质新事实会重新打开 Fact；重大方案变化会重新打开 Plan 和授权。

### 4. Review — 对抗性检查稳定工件

干净上下文 Reviewer 沿 Contract / Intent、Correctness / Safety、Repository Shape / Code health 和 Verification 四个轴检查。`P0` 阻塞，`P1` 标记重要缺陷或遗漏，`P2` 最多保留三项低影响改进。Reviewer 提供证据，主 Agent 负责仲裁。

### 5. Handoff — 外置状态，不复制整段对话

主 Agent 保留判断和最终综合，边界清楚的重活可以委派。跨会话 Handoff 引用已有计划、diff、commit 和研究工件，而不重复其内容，让下一段上下文继续用于决策，而不是承载旧实现历史。

<a id="installation"></a>

## ⚙️ 安装

GroundRail 使用开放的 Agent Skills 结构，同一份 `groundrail/SKILL.md` 可同时供 Codex 和 Claude Code 使用。它只采用标准 `name`、`description` 和 Markdown 指令；Codex 专属的可选 UI 元数据单独放在 `groundrail/agents/openai.yaml`，不会影响 Claude Code。

### 快速安装

```bash
npx skills add PeterLeeXX/GroundRail --skill groundrail -a codex -a claude-code -g --copy -y
```

### 推荐：同时安装到 Codex 和 Claude Code

先确保两个宿主的用户级 Skill 目录存在，再使用 [Skills CLI](https://github.com/vercel-labs/skills) 安装：

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

只安装一个宿主时：

```bash
npx skills add PeterLeeXX/GroundRail --skill groundrail --agent codex --global --copy --yes
npx skills add PeterLeeXX/GroundRail --skill groundrail --agent claude-code --global --copy --yes
```

安装后检查两个宿主的识别状态：

```bash
npx skills list --global --agent codex --agent claude-code
```

如果 CLI 显示 `Agents: not linked`，或宿主中看不到 GroundRail，请使用下面的原生目录安装。这样也能避开不同 CLI 版本的链接差异。

### 原生目录安装

先下载仓库：

```bash
git clone --depth 1 https://github.com/PeterLeeXX/GroundRail.git
```

Codex 用户级安装目录为 `~/.agents/skills/`：

```bash
mkdir -p "$HOME/.agents/skills/groundrail"
cp -R GroundRail/groundrail/. "$HOME/.agents/skills/groundrail/"
```

Claude Code 用户级安装目录为 `~/.claude/skills/`：

```bash
mkdir -p "$HOME/.claude/skills/groundrail"
cp -R GroundRail/groundrail/. "$HOME/.claude/skills/groundrail/"
```

项目级安装使用相同结构，把目标目录分别换成仓库根目录下的 `.agents/skills/groundrail/` 和 `.claude/skills/groundrail/`。

Codex 通常会自动发现 Skill 变化；未出现时重新启动 Codex，并用 `/skills` 检查或输入 `$groundrail` 调用。Claude Code 会监视已有的 Skill 目录；如果本次安装新建了顶层 `.claude/skills/`，重新启动 Claude Code，然后输入 `/groundrail` 检查。两者都可以根据自然语言需求自动触发，GroundRail 不会接管无关对话。

## 💬 使用

以下请求无需显式写出 Codex 的 `$groundrail` 或 Claude Code 的 `/groundrail`：

完成调查，不修改代码：

```text
调查这个仓库问题，只完成 Fact，不修改代码。
```

基于已有事实制定计划：

```text
审查 ./facts.md；通过后生成 plan.md，不实施。
```

执行已有计划：

```text
审查 ./plan.md，确认事实和计划有效后向我申请实施授权。
```

只审查当前改动：

```text
审查当前 diff，按 P0 / P1 / P2 报告，只读，不修复。
```

交接当前任务：

```text
把当前任务交接给下一会话，重点继续处理验证失败；生成安全的 handoff 文档。
```

## 🎯 适用范围

GroundRail 最适合“能力已经足够，边界仍会漂移”的场景：

- 涉及多个模块、配置或数据面的存量仓库改动；
- 容易因版本、运行状态或外部契约产生错误判断的任务；
- 需要先交付调查结果或实施计划的任务；
- 需要跨会话或跨 Agent 交接，同时控制上下文污染的任务；
- 用户希望保留产品、范围与风险决策权的任务。

它不替代专项的安全、性能、测试、UI 或发布 Skill，也不压制模型调用这些能力。简单、低风险且事实明确的任务无需为了“遵守流程”而生成文档；风险很高或仓库另有严格规范时，则应叠加相应专项约束。

## ⚖️ 为什么 GroundRail 比 Superpowers 更适合现在？

[Superpowers](https://github.com/obra/superpowers) 把自己定义为一套完整的软件开发方法论。它当前的基础流程默认包含 brainstorming、worktree、2–5 分钟颗粒度的实施任务、严格的 red-green-refactor TDD、逐任务子代理、重复审查和分支收尾。当流程需要弥补 Agent 尚不能自主建立的工程纪律时，这很有价值。

GroundRail 面向的是另一个能力阶段。强模型已经能够调查陌生仓库、追踪行为、选择实现、复用现有抽象、编写聚焦测试并修正方案。把这些能力全部重新写成常驻的逐步指令，会消耗上下文，也可能把有价值的判断变成对流程的机械遵循。

### 瓶颈已经变了

对强模型而言，反复失败的地方已经不再主要是“不知道某种编码技巧”，而是基于局部证据行动、让假设在阶段间固化、跨过用户的决策边界、审查错误的对象，或把过多旧上下文带入下一次判断。GroundRail 把约束集中在这些位置。

### 计划应保留意图，而不是脚本化每个动作

Superpowers 刻意把计划写到低上下文的初级工程师也能照做。GroundRail 只写下能保留目标、证据、复用点、边界和验证的最小连贯 Plan，然后让模型依据实时仓库自主决定普通实现细节。

### 约束应跟随风险缩放

小而明确的改动可以内联 Fact 和 Plan；高风险或长时任务才需要稳定工件、独立 Review、明确授权和 Handoff。TDD、worktree、专项审查或子代理仍然可以在任务需要时调用；它们是能力，不是普遍仪式。

### 更少流程，更强控制

Superpowers 控制更多“开发应如何进行”；GroundRail 控制“什么可以成为事实、何时可以进入下一阶段、谁拥有关键决策、哪些上下文应被保留”。对当下最强的编码模型，这通常是更好的交换：更少的常驻指令，更多的能力空间，同时对那些“仅靠能力仍无法消除”的失败保留更严格的保护。

当团队需要一套强制的端到端开发方法论时，Superpowers 仍然是很强的选择。当模型已经具备能力，而目标是约束漂移而不是约束智能时，GroundRail 更适合。

## 🤝 参与项目

发现事实覆盖缺口、错误门禁或无价值的审查循环，请提交 [Issue](https://github.com/PeterLeeXX/GroundRail/issues)。改进建议最好附带可复现任务或前后行为，不需要为了让流程看起来更完整而增加常驻规则。

GroundRail 遵循 [Agent Skills standard](https://agentskills.io)，并使用 [MIT License](./LICENSE)。
