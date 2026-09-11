## 仓库定位

这是一套文档驱动的个人产品工作流承接平台。当前以开发流为主，并提供独立的业务文档整理能力，支持会话提炼、任务归档和历史逻辑梳理。后续可按最小路径扩展其他生命周期能力；当前范围以 `docs/authority/product/scope.md` 为准。

这里的“平台”指 repo-native、skill-based、Markdown-first 的个人工作流承接层，不指 CLI 后端、守护进程或重型流水线产品。

## 文档治理冲突处理规则

本仓库自身的 Doc Loom Least 文档治理必须遵守 SSOT、ADR 和文档冲突处理规则。维护本仓库文档时，先使用 `docs/authority/constitution.md` 和 `docs/authority/README.md` 判断根本原则、事实归属、权威顺序和冲突处理方式。

默认权威顺序：

1. `docs/authority/constitution.md`：Doc Loom Least 根本性宪法，定义不可违反的顶层设计原则。
2. `docs/authority/README.md` 与 `docs/authority/**`：当前已治理事实入口与分领域权威。
3. `skills/**`：当前 Skill 实现事实。

若 adapter 与 `docs/authority/**`、ADR 或当前 Skill 实现冲突，应修正 adapter。
