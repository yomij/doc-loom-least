# 技能措辞与描述打磨

## 目标

在不改变 Doc Loom Least 现有 5 个 Skill 的职责边界、授权门禁、完成条件与独立安装特性的前提下，打磨各 Skill 的 frontmatter `description`、正文指令措辞与本地资源表达，提升路由区分度与指令可读性。

## 成功条件

- 5 个 Skill 的 `description` 准确覆盖各自触发场景并划清易混边界（涵盖开发规划/续办、通用读者业务文档、只读评审三态结论、显式假设追问、结构化文档治理）。
- 消除 `SKILL.md` 正文中的电报式省略句、错位拼接句与同文件内跨段落重复，保持核心约束完整且单文件 Token 成本不膨胀。
- 清理 `skills/development/docloom-workflow/references/` 历史遗留空目录，理顺本地辅助资源（如 `complexity-only.md`）中的含糊表述。
- 实施后全部 5 个 Skill 通过结构校验、本地链接检查、独立只读评审与 `git diff --check`。

## 约束与决策

- 基线提交：`22b574037aaab91efbae690f08ed7400934e6fc4`。
- 用户已确认改造计划（“处理吧 / 同步用skillshare”），授权修改源仓库 `skills/**` 并通过 `skillshare` 同步本机安装目标。
- 保持宪法、5 个独立 Skill 结构、按需加载条件、输出语言跟随用户、`<YYYYMMDD>-<short-slug>` 命名规则及既有权威契约不变。
- 本任务为技能指令措辞与结构打磨，不涉及业务规则变更，无需生成业务文档。

## 当前状态

done — 已完成全部 5 个 Skill 及 `complexity-only.md` 的描述与措辞打磨，清理 `docloom-workflow/references/` 空目录，并通过 `skillshare` 同步至本机全部 5 个目标环境。

## 下一步

无后续待办步骤。

## 验证证据

- 使用 `uv run --with pyyaml` 执行 `quick_validate.py` 校验全部 5 个 Skill 目录：全部通过（`Skill is valid!`）。
- 运行 `skillshare audit ./skills --format json`：`riskScore: 0`、`riskLabel: "clean"`，无安全或格式告警。
- 运行本地 Markdown 资源链接校验：所有按需加载指针（`templates/task.md`、`templates/business.md`、`references/complexity-only.md`、`references/governance-rules.md`、`templates/governance-plan.md`）均解析有效且保留 `**conditionally**` 触发条件。
- 独立只读 `review` 复核（基线 `22b5740`）：核对 6 个变更文件与空目录清理，结论为 `pass`（无 Critical/Important/Minor 缺陷，未改变任何权威契约或授权门禁）。
- 执行 `skillshare install ... --force -g` 与 `skillshare sync -g`，并通过 `diff -ru` 校验 `/Users/yomi/Documents/project/doc-loom-least/skills` 与 `/Users/yomi/.codex/skills` 下的直连与跟踪副本：差异为 0。
- `git diff --check` 通过。

## 结果与剩余事项

5 个 Skill 的 `description` 与正文措辞已打磨完成并通过 `skillshare` 同步至本机目标环境（claude、codex、droid、grok、pi）。未包含远端 `git push`；若后续推送远端仓库，可再通过远端更新跟踪源。
