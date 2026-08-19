# Handoff

当用户要求把任务交给下一会话或 Agent，或当前会话必须在未完成状态停止且确有续接需求时，读取本文件并生成一个安全的 Markdown 交接文档。Handoff 只保存继续工作的最小状态，不是对话摘要，也不替代 Fact、Plan、Review、issue、commit 或最终交付物。

## 何时生成

满足以下任一条件时生成：

- 用户明确要求交接、续接说明或 handoff 文档。
- 后续工作必须在新会话、新上下文或另一执行者中继续。
- 当前任务因权限、外部状态、验证失败或上下文切换而停止，但已有工作需要安全保留。

任务已经完整交付且没有续接动作时不生成。仅为记录聊天历史、展示工作量或替代正常最终回复时也不生成。

## 生成前取证

在既有授权范围内读取与续接直接相关的稳定状态：

- 用户原始目标、已确认的验收、非目标和后续重点。
- 最新 Fact、Plan、Research、Review、issue、commit、diff 或其他权威工件。
- 当前仓库、分支、基准提交、相关未提交改动和实际验证结果。
- 已作用户决定、现有修改授权、待决选择和阻塞项。

只补足会让下一执行者走错方向的状态缺口；不要为了让 Handoff 看起来完整而重新调查整个任务。用户为交接提供的参数是下一会话的裁剪重点，而不是附加在全文末尾的备注。

## 保存位置与文件安全

- 只生成一个 Markdown 文件，保存到用户操作系统解析出的临时目录，不能放在当前工作区。
- 使用宿主或语言运行时提供的临时目录解析方式；不要硬编码 `/tmp`、`$HOME`、用户名或平台专属路径。
- 文件名优先采用 `groundrail-handoff-<task-slug>-<timestamp>.md`，其中 slug 不含敏感信息。
- 写入前确认目标不存在；若冲突则生成新名称，不覆盖未知文件。
- 完成后向用户返回绝对路径和一句当前状态，不在最终回复中粘贴整份交接内容。

## 内容原则

Handoff 应让没有当前对话历史的下一执行者回答五个问题：

1. 最终要完成什么，当前停在哪个阶段？
2. 哪些事实、决定和授权仍然有效？
3. 已完成什么，哪些内容尚未完成或已经失败？
4. 权威证据和工作区状态在哪里？
5. 下一步先做什么，遇到什么情况必须停下？

引用已有工件的路径、commit、diff、issue 或 URL，不复制其正文。只保留理解失败状态所需的短错误摘要；长日志应引用原文件或命令。不得复制聊天记录、重复计划步骤、重新解释完整设计，或把未经验证的摘要写成事实。

## 必填结构

按任务实际情况裁剪可选项，但不得省略目标、状态、决定与授权、权威工件、剩余工作、验证状态、下一步和 Suggested skills。

```markdown
# Handoff — <task>

- Created: <带时区的时间>
- Continuation goal: <下一执行者最终要达成的结果>
- Current stage/state: <Fact / Plan / awaiting authorization / Execute / Review / blocked 等>
- Stop reason: <为什么现在交接；主动换会话时写 planned handoff>

## Contract

- Confirmed acceptance: <已确认验收>
- Non-goals: <明确不做什么>
- Continuation focus: <用户要求下一会话重点处理什么>

## User decisions and authorization

- Confirmed decisions: <仍有效的用户决定>
- Modification authorization: <范围、是否仍有效；没有则写 None>
- Pending user decisions: <会影响下一步的选择；没有则写 None>

## Current state

- Completed: <已完成且有证据的工作>
- In progress: <未形成稳定结果的工作；没有则写 None>
- Remaining: <尚未完成的目标>
- Blockers / unknowns: <阻塞与证据缺口；没有则写 None>

## Authoritative artifacts

| Type | Path / commit / URL | Status and purpose |
| --- | --- | --- |
| Fact / Plan / Research / Review / Diff / Issue | <引用> | <为什么下一执行者应读取它> |

## Repository state

- Repository/worktree: <可安全披露的定位>
- Branch and base commit: <branch / commit；不适用则写 None>
- Relevant changes: <相关已修改、未跟踪或已提交文件>
- Unrelated user changes: <必须保留且不得覆盖的改动；没有则写 None>

## Verification state

| Command or check | Result | Evidence / failure focus |
| --- | --- | --- |
| <实际运行内容> | PASS / FAIL / NOT RUN | <关键结果或日志引用> |

## Next actions

1. <下一执行者的第一个可验证动作>
2. <后续最小步骤>

- Stop and ask when: <权限、产品决定、新事实或方案变化等停止条件>

## Suggested skills

- <精确 skill 名称及用途；没有则写 None>

## Safety and context boundaries

- <敏感信息、外部写入、破坏性操作、范围或并发所有权限制>
```

## 状态记录规则

- **完成状态**只记录已有文件、diff、commit、命令结果或用户确认支持的内容。
- **进行中状态**不得伪装成完成；说明中间产物是否安全复用。
- **验证状态**逐项写真实命令和结果。未运行写 `NOT RUN`，失败写 `FAIL` 并指出首个可行动失败，不写“基本通过”。
- **授权状态**必须说明修改授权的范围和是否仍有效。Plan 实质变化后旧授权失效时要明确写出。
- **仓库状态**区分本任务改动与用户原有或无关改动，提醒下一执行者不得覆盖后者。
- **外部状态**若会过期，记录观测时间和来源；不要把可能已变化的状态写成长期事实。
- **下一步**以一个可验证动作开始，并包含会触发返回 Fact、重开 Plan 或请求用户决定的停止条件。

## 敏感信息处理

- 不写 API key、密码、令牌、cookie、私钥、完整连接串、个人身份信息或其他敏感值。
- 需要说明凭据来源时，只写安全的环境变量名、秘密管理器引用或“需重新取得授权”，不复制值。
- 命令、错误、URL 和路径中包含敏感值时先脱敏；无法安全脱敏则只描述位置和影响。
- 本地绝对路径若含敏感身份信息，改用仓库相对路径、泛化前缀或仓库 URL；不要为了可点击性泄露身份。
- 不把 Handoff 当作绕过权限边界的载体；下一执行者仍需遵守原任务授权和宿主权限。

## 交付前自检

- 文件位于操作系统临时目录且不在工作区。
- 只有一个 Handoff 文件，目标不存在时才写入。
- 目标、当前阶段、停止原因、剩余工作和第一个下一动作清楚。
- 用户决定、修改授权及待决选择没有遗漏。
- 权威工件使用引用而非复制，路径或提交可供下一执行者定位。
- 验证的 PASS、FAIL、NOT RUN 与真实结果一致。
- 相关改动与无关用户改动已区分。
- Suggested skills 使用精确名称；没有则为 `None`。
- 文档及路径不含秘密、令牌或敏感身份信息。

除非用户明确要求或交接本身涉及高风险权限、数据或生产状态，不为 Handoff 文档额外启动独立 Review；以上自检是默认完成门槛。
