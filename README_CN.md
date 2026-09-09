# Doc Loom Least

Doc Loom Least 是一个面向个人开发者和 AI Agent 的 Markdown-first 工作流。
它保留当前事实、人类决策和验证证据，不把仓库变成复杂流水线。

## 默认循环

从当前权威和实际工作区理解目标，在授权范围内执行，逐项验证成功条件，
同步受影响文档并报告结果。

大多数可逆的单轮工作不建 Case。需要跨会话接续、保留重要决定、处理中断，
或用户明确要求时，才创建一份 task.md，记录目标、成功条件、约束与决定、
当前状态、下一步、验证证据和结果。旧的 plan、execution、closure 文件仍可
作为历史证据读取。

只在关键事实无法确认、目标或约束改变、行动超出授权或涉及重大后果时询问用户。

## Skills

| Skill | 用途 |
|---|---|
| docloom-workflow | 日常开发入口、可选任务记录、状态、接续和发现。 |
| review | 用户要求或风险与证据需要时的只读复核。 |
| grill | 用户明确要求时，对主张或假设进行对话式追问。 |
| setup-doc-governance | 结构性文档和权威治理。 |

context-authority、plan-confirm、tdd-execute、doc-sync-close 四个旧入口由
ADR-0004 退役；必要行为已归入 docloom-workflow，旧 Case 记录继续作为证据。

## 边界

没有 CLI 后端、守护进程、运行时工作流引擎或自动发布。review 和 grill 不修改
文件或任务状态。authority 文档记录确认后的可复用事实，归档内容是历史证据。

## 安装

使用 skillshare 安装四个 Skill：

    skillshare install github.com/yomij/doc-loom-least --track --json
    skillshare sync

私有仓库使用 SSH 地址。已有安装不会自动清理；请按现有 skillshare 配置同步
源并移除退役副本。更新与审计命令见 INSTALL.md。
