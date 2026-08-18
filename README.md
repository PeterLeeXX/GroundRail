<p align="center">
  <img src="./groundrail.svg" width="180" alt="GroundRail logo" />
</p>

<h1 align="center">GroundRail</h1>

<p align="center">给强编码基模的证据地基与轻量护轨。</p>

<p align="center">
  <a href="./README.md">简体中文</a> · <a href="./README_EN.md">English</a>
</p>

[![][github-forks-shield]][github-forks-link]
[![][github-stars-shield]][github-stars-link]
[![][github-issues-shield]][github-issues-link]
[![][agent-skills-shield]][agent-skills-link]

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

核心路径是 `Fact → Plan → Execute`，Review 在稳定检查点横切其中。它是逻辑门槛，不是文档仪式：低风险小任务可以只用几行内联事实和计划；只有复杂、长时、跨 Agent 或跨会话任务才需要持久化工件。

项目名和 Skill 名均为 **GroundRail**。涉及仓库调查、计划、实施、代码审查或任务交接的请求可以自动路由到该 Skill；Codex 可用 `$groundrail`、Claude Code 可用 `/groundrail` 显式调用：**用事实让 Agent 落地，用最少护轨让强基模继续发挥。**

## 它刻意不做什么

GroundRail 不是一套覆盖整个软件开发生命周期的重型方法论。它不会默认强制 brainstorming、逐任务子代理、2–5 分钟颗粒度拆解、严格 TDD、worktree、频繁审查或固定分支收尾；这些能力可以由模型、仓库规范或专项 Skill 按任务需要调用。

它也不会把普通实现选择不断抛回用户。只有会改变产品行为、范围、验收、成本、安全或不可逆状态的选择才需要用户决定；其余部分应让强基模依据证据和仓库惯例自主完成。GroundRail 约束的是越权与失真，不是能力本身。

## 安装

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

## 使用

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

## 五根护轨

复杂实施任务的完整路径如下：

```text
Fact → Review → Plan → Review → user approval → Execute → Review
```

这条路径按风险收缩，而不是机械执行。低风险 Fact 可以内联并合并进 Plan Review；简单修改不需要创建一套任务文档；用户也可以只要 Fact、Plan 或只读 Review。GroundRail 要的是阶段边界真实存在，不是每个阶段都留下文件。

### 1. 事实不能由直觉代替

Fact 只记录与当前目标相关、可观察且可定位的内容。证据强度与风险匹配：Bug 要有能命中症状的反馈信号，外部行为要核对实际版本和一手契约，UI、安全、异步任务分别检查其真实运行时、信任边界和进展性。没有找到的内容注明搜索边界，未知保留为缺口，不能靠合理猜测补齐。

### 2. 计划不能替假设“洗白”

Plan 在 Fact 完成后形成，把观察事实、证据支持的结论和待验证假设分开。它只描述满足目标所需的最小连贯改动、现有复用点和最小充分验证，不预写大段实现代码。产品行为、范围、验收、成本、安全和不可逆取舍由用户确认；函数拆分、命名和符合仓库惯例的普通实现选择交给模型。

### 3. 实施不能越过授权边界

Execute 只在计划已审且用户授权后开始。模型在边界内自主实施、验证和简化触达区域，但不顺手扩张范围。新事实实质改变需求或安全边界时回到 Fact；方案需要重大调整时回到 Plan 并重新审查。这样约束的是改什么，而不是把如何写的每一步都脚本化。

### 4. 审查必须对准稳定工件

Review 不占据一个固定阶段，也不在每次保存后触发。它用干净上下文检查稳定的 Fact、Plan 或代表性 diff，避免继承实现者的结论；审查集中在 Contract / Intent、Correctness / Safety、Repository Shape / Code health 和 Verification 四个轴上：

| 级别 | 含义 | 处理方式 |
| --- | --- | --- |
| P0 | 阻塞、重大缺陷、关键逻辑错误、安全或数据完整性问题 | 实施任务中修复并复审；只读审查仅报告 |
| P1 | 重要缺陷或风险、明显遗漏、重复实现、偏离需求或计划 | 实施任务中修复并复审；只读审查仅报告 |
| P2 | 非关键校验、有限鲁棒性、风格或低影响改进 | 最多报告三项；实施任务按收益选择处理，只读审查不修改 |

Reviewer 只提供证据，主 Agent 负责逐项仲裁；P2 最多三项，避免风格建议淹没真实风险。宿主无法创建独立 Reviewer 时必须明确标记降级并停在检查点，不能把同上下文自审包装成独立通过。

### 5. 长任务必须外置状态

主 Agent 保留连续判断、冲突消解和最终综合，只把耗时研究、宽仓库扫描、独立假设、机械修改或独立验证等边界清楚的重活委派出去；子代理默认返回结果，不把完整探索过程灌回主对话。

跨会话时，Handoff 只保留续接所需状态，并引用已有 specs、plans、ADRs、issues、commits、diffs 和研究工件，而不重复它们。交接文档放在操作系统临时目录并隐去敏感信息，避免把上下文快照变成仓库中的长期噪声。

## 适用范围

GroundRail 最适合“能力已经足够，边界仍会漂移”的场景：

- 涉及多个模块、配置或数据面的存量仓库改动；
- 容易因版本、运行状态或外部契约产生错误判断的任务；
- 需要先交付调查结果或实施计划的任务；
- 需要跨会话或跨 Agent 交接，同时控制上下文污染的任务；
- 用户希望保留产品、范围与风险决策权的任务。

它不替代专项的安全、性能、测试、UI 或发布 Skill，也不压制模型调用这些能力。简单、低风险且事实明确的任务无需为了“遵守流程”而生成文档；风险很高或仓库另有严格规范时，则应叠加相应专项约束。

## 与 Superpowers 比较

[Superpowers](https://github.com/obra/superpowers) 是包含 brainstorming、worktree、计划、TDD、子代理开发、代码审查和分支交付的完整方法论。它适合希望 Agent 遵循一套细致开发方法的场景。GroundRail 的出发点不同：假定强基模已经掌握大部分编码流程，只在证据、阶段、授权、审查和上下文这五类边界上铺设护轨。

**🧠 基本假设不同。** GroundRail 假设模型已经具备强编码能力，主要风险是证据失真和边界漂移；Superpowers 用完整方法论把开发过程展开为明确、细粒度的操作步骤。

**🚦 启用方式与起点不同。** GroundRail 按调查、计划、实施、代码审查或交接意图自动路由，也可在 Codex 中用 `$groundrail`、在 Claude Code 中用 `/groundrail` 显式调用，并从事实基线开始。Superpowers 从对话开始检查和路由相关 Skills，先分类任务，再通过 brainstorming 明确设计。

**📝 计划粒度不同。** GroundRail 简洁记录修改位置、现有复用点、关键逻辑和验证，不预写大段代码。Superpowers 的 architectural plan 可拆成 2–5 分钟任务，并包含更完整的实现说明。

**⚙️ 实施编排不同。** GroundRail 默认由主 Agent 执行已审计划，只委派边界清楚的独立重活。Superpowers 可以为每项任务派发新子代理，再分别进行规格和代码质量审查。

**🧪 测试约束不同。** GroundRail 根据具体风险选择最小充分证据；Superpowers 对功能、修复、重构和行为变化默认采用严格 TDD。

**🔍 审查节奏不同。** GroundRail 在稳定的 Fact、Plan、代表性 diff 和终态等检查点触发 Review，避免频繁审查制造噪声。Superpowers 更强调早审查和频繁审查，并覆盖任务、重大功能和合并前等边界。

**🙋 用户门禁不同。** GroundRail 要求修改代码前批准已审计划，或由用户预先明确授权；Superpowers 的计划型实施在设计确认后开始。

如果需要完整的开发方法论、强制 TDD、worktree 和分支收尾，Superpowers 覆盖得更全面。如果使用 Sol、Claude Opus 4.8、GLM-5.3 等强基模，希望保留其自主判断和工程发挥，同时减少事实遗漏、越权决策、上下文混杂和无效审查，GroundRail 更贴近这个取舍。

对比依据为 `obra/superpowers` 的 [`b36e082`](https://github.com/obra/superpowers/tree/b36e0829c6d0140e93cfef2ca599b1b07d4a7797) 版本（2026-08-18）。具体行为以其最新文档为准。

## 发布结构

发布时只需以下运行与说明文件；`upstream/` 和设计笔记属于开发参考，不是 Skill 运行内容。

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

## 设计参考

- [Agent Skills standard](https://agentskills.io)
- [Anthropic Skills](https://github.com/anthropics/skills)
- [Vercel Agent Skills](https://github.com/vercel-labs/agent-skills)
- [Hugging Face Skills](https://github.com/huggingface/skills)
- [Thoughtworks: Agent instruction bloat](https://www.thoughtworks.com/radar/techniques/agent-instruction-bloat)
- [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)

## 参与项目

发现事实覆盖缺口、错误门禁或无价值的审查循环，请提交 [Issue][github-issues-link]。改进建议最好附带可复现任务或前后行为，不需要为了让流程看起来更完整而增加常驻规则。

[github-forks-shield]: https://img.shields.io/github/forks/PeterLeeXX/GroundRail?style=flat-square&logo=github&color=2D565E
[github-forks-link]: https://github.com/PeterLeeXX/GroundRail/network/members
[github-stars-shield]: https://img.shields.io/github/stars/PeterLeeXX/GroundRail?style=flat-square&logo=github&color=2D565E
[github-stars-link]: https://github.com/PeterLeeXX/GroundRail/stargazers
[github-issues-shield]: https://img.shields.io/github/issues/PeterLeeXX/GroundRail?style=flat-square&logo=github&color=2D565E
[github-issues-link]: https://github.com/PeterLeeXX/GroundRail/issues
[agent-skills-shield]: https://img.shields.io/badge/Agent%20Skills-compatible-2D565E?style=flat-square
[agent-skills-link]: https://agentskills.io
