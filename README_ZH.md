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

## ⚡ 快速安装

```bash
npx skills add PeterLeeXX/GroundRail --skill groundrail -a codex -a claude-code -g --copy -y
```

这条命令会同时安装到 Codex 和 Claude Code。单宿主、项目级及原生目录安装见[完整安装说明](#installation)。

## 🚧 为什么是 GroundRail？

GroundRail 面向 Sol、Claude Opus 4.8、GLM-5.3 等已经内化了大量编码流程的强基模。它不再从头教授如何读代码、拆任务、写测试或实现功能，也不试图用一份巨细无遗的操作手册代替模型判断。强基模真正需要的，通常不是更多“该怎么写代码”的细则，而是在几个容易脱轨的位置保留清晰、可检查的行为边界。

**Ground** 是可定位、可复现的事实地基；**Rail** 是约束阶段顺序、决策归属和审查边界的最少护轨。轨道规定“什么时候可以继续、什么决定必须留给用户”，不规定每一行代码应当怎样写。模型仍然负责理解仓库、选择实现、复用现有设计和处理普通工程细节。

### 🛤️ 五种常见脱轨

GroundRail 不限制模型如何写代码。它只在证据、假设、决策权、审查和上下文开始偏离时，把任务拉回轨道。

> #### `01` 局部证据，过早方案
>
> **失控点** — 只读了局部代码，Agent 就开始解释根因或设计方案。<br>
> **护轨** — 先建立与风险匹配的最小充分 `Fact`，让关键结论能追溯到源码、配置、运行结果或外部契约。

> #### `02` 假设漂移，层层固化
>
> **失控点** — 调查中的猜测进入 Plan，Plan 中的假设又在 Execute 时变成“事实”。<br>
> **护轨** — 分开观察、证据支持的结论与行动；未知始终标记为证据缺口，不能被流程悄悄洗白。

> #### `03` 决策越界，替用户取舍
>
> **失控点** — Agent 自行决定产品行为、范围、成本、安全或不可逆风险。<br>
> **护轨** — 把关键取舍留给用户；模型只依据事实和仓库惯例，自主完成普通实现细节。

> #### `04` 审查失焦，关键遗漏
>
> **失控点** — Review 漏掉真正影响交付的问题，却反复纠缠风格和低价值建议。<br>
> **护轨** — 只审查稳定工件，在干净上下文中沿四个轴检查，并用 P0/P1/P2 控制优先级与噪声。

> #### `05` 上下文淤积，判断受污染
>
> **失控点** — 长任务把探索过程和实现细节持续堆在主对话中，让后续判断被旧上下文牵引。<br>
> **护轨** — 用稳定工件、结果式委派和安全 Handoff 外置状态，把主 Agent 的上下文留给连续判断、冲突消解与最终综合。

项目名和 Skill 名均为 **GroundRail**。涉及仓库调查、计划、实施、代码审查或任务交接的请求可以自动路由到该 Skill；Codex 可用 `$groundrail`、Claude Code 可用 `/groundrail` 显式调用：**用事实让 Agent 落地，用最少护轨让强基模继续发挥。**

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

## 🪶 GroundRail 刻意留给模型的空间

GroundRail 不是一套覆盖整个软件开发生命周期的重型方法论。它不会默认强制 brainstorming、逐任务子代理、2–5 分钟颗粒度拆解、严格 TDD、worktree、频繁审查或固定分支收尾；这些能力可以由模型、仓库规范或专项 Skill 按任务需要调用。

它也不会把普通实现选择不断抛回用户。只有会改变产品行为、范围、验收、成本、安全或不可逆状态的选择才需要用户决定；其余部分应让强基模依据证据和仓库惯例自主完成。GroundRail 约束的是越权与失真，不是能力本身。

<a id="installation"></a>

## ⚙️ 安装

GroundRail 使用开放的 Agent Skills 结构，同一份 `groundrail/SKILL.md` 可同时供 Codex 和 Claude Code 使用。它只采用标准 `name`、`description` 和 Markdown 指令；Codex 专属的可选 UI 元数据单独放在 `groundrail/agents/openai.yaml`，不会影响 Claude Code。

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

## ⚖️ GroundRail 与 Superpowers

[Superpowers](https://github.com/obra/superpowers) 是包含 brainstorming、worktree、计划、TDD、子代理开发、代码审查和分支交付的完整方法论。它适合希望 Agent 遵循一套细致开发方法的场景。GroundRail 的出发点不同：假定强基模已经掌握大部分编码流程，只在证据、阶段、授权、审查和上下文这五类边界上铺设护轨。

### 适合选择 GroundRail 的情况

- 模型已经具备强编码能力，普通工程判断应继续由模型完成；
- 主要风险是证据缺口、决策越界、审查噪声和上下文污染；
- 计划应描述最小连贯改动，而不是脚本化每个实现步骤；
- 测试和委派强度需要随任务真实风险变化。

### 适合选择 Superpowers 的情况

- 希望采用完整、强约束的开发方法论；
- brainstorming、细粒度计划、严格 TDD、worktree 和频繁子代理审查应成为默认；
- 希望流程本身指导大部分实现行为。

两者可以互相借鉴，但优化的是不同控制面：Superpowers 规定更多开发方法，GroundRail 保护强模型自主判断周围的关键边界。Superpowers 的具体行为以其最新上游文档为准。

## 📦 仓库结构

发布时只需以下运行与说明文件；`upstream/` 和设计笔记属于开发参考，不是 Skill 运行内容。

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

## 🧠 设计参考

- [Agent Skills standard](https://agentskills.io)
- [Anthropic Skills](https://github.com/anthropics/skills)
- [Vercel Agent Skills](https://github.com/vercel-labs/agent-skills)
- [Hugging Face Skills](https://github.com/huggingface/skills)
- [Thoughtworks: Agent instruction bloat](https://www.thoughtworks.com/radar/techniques/agent-instruction-bloat)
- [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)

## 🤝 参与项目

发现事实覆盖缺口、错误门禁或无价值的审查循环，请提交 [Issue](https://github.com/PeterLeeXX/GroundRail/issues)。改进建议最好附带可复现任务或前后行为，不需要为了让流程看起来更完整而增加常驻规则。

## 📄 许可证

GroundRail 使用 [MIT License](./LICENSE)。
