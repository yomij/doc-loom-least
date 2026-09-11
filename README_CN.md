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

只在缺失事实会改变目标或约束，或行动需要尚未提供的授权时询问用户。
恢复任务沿用已有明确意图和授权。

## Skills

| Skill | 用途 |
|---|---|
| docloom-workflow | 日常开发入口、可选任务记录、状态、接续和发现。 |
| business-docs | 独立整理会话业务结论、任务业务档案和历史业务逻辑。 |
| review | 用户要求或命中明确开发触发条件时的只读复核，无须另建 Agent。 |
| grill | 用户明确要求时，对主张或假设进行对话式追问。 |
| setup-doc-governance | 结构性文档和权威治理。 |

context-authority、plan-confirm、tdd-execute、doc-sync-close 四个旧入口由
ADR-0004 退役；必要行为已归入 docloom-workflow，旧 Case 记录继续作为证据。

## 业务文档

可以直接调用 business-docs，不要求此前使用过 Docloom，也不要求已有 task.md：

- “把这次讨论整理成业务文档，保留最终规则、关键决策和未决问题。”
- “归档本次任务的业务内容，说明需求修正和实际交付范围。”
- “梳理现有订单取消逻辑，写给产品和测试看，包含限制条件和异常场景。”
- “根据历史任务整理优惠券退回规则的演变，区分当前和废弃规则。”

正文保持纯业务表达，来源集中列出；从代码观察到的行为不能代替业务意图，
找不到的决策原因保留为未知。Docloom 开发过程中记录业务结论和修正，收尾时
完成归档；纯技术任务无需空档案。优先遵循已有目录约定，否则写入任务目录的
business.md，或 docs/business/archives/ 下的日期加主题文档，并更新业务索引。
归档保留历史快照，不自动确立现行业务权威。

规则见[业务文档契约](docs/authority/workflow/business-docs.md)，本仓库的样例见
[业务文档索引](docs/business/README.md)。

## 边界

没有 CLI 后端、守护进程、运行时工作流引擎或自动发布。review 和 grill 不修改
文件或任务状态。authority 文档记录确认后的可复用事实，归档内容是历史证据。

## 安装

使用 skillshare 安装五个 Skill：

    skillshare install github.com/yomij/doc-loom-least --track --json
    skillshare sync

私有仓库使用 SSH 地址。已有安装不会自动清理；请按现有 skillshare 配置同步
源并移除退役副本。更新与审计命令见 INSTALL.md。
