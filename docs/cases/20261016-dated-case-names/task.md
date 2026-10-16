# Case 名称保留日期

## 目标与成功条件

明确新 case 名称必须包含日期，让 Skill 和任务模板继续采用仓库既有的
`YYYYMMDD-主题` 格式；开发流说明与实现保持一致。

## 约束与决策

- 来源：2026-10-16 当前会话，用户原话：“更新一下 skills，case命名 还是得加上日期”。这是会话摘录，无独立会话链接。
- 沿用现有 case 的八位日期前缀，日期取用户时区下的 case 创建日，后接简短主题。
- 续办沿用现有目录。本 case 以 `20261016-dated-case-names` 实际使用该格式。
- 本轮为技术工作流命名约定，无业务规则变更。

## 当前状态

done — 已在 [Skill](../../../skills/development/docloom-workflow/SKILL.md) 中
明确日期前缀，同步[任务模板](../../../skills/development/docloom-workflow/templates/task.md)
和[开发流说明](../../authority/workflow/development-flow.md)，结构校验与差异检查通过。

## 下一步

本轮源仓库修改已完成，无后续实施步骤。

## 验证证据

- Skill 结构校验通过，`git diff --check` 通过。
- 已核对新建时采用创建日期、续办时沿用目录的边界；本 case 实际采用日期前缀。
- 检查覆盖指令、模板和说明文本，未运行独立模型行为测试。

## 结果与剩余事项

源仓库的 case 命名要求已明确。安装副本尚未同步。
