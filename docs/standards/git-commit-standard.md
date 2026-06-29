# Git 提交标准

提交标题除 Git 自动生成的 `Merge ...`、`Revert ...` 外，统一使用：

```text
<type>: <summary>
```

- `type` 必填且小写，只允许 `feat`、`fix`、`docs`、`chore`、`refactor`、`style`、`test`、`perf`、`revert`。
- `summary` 必填，简短说明业务对象和动作。
- `type` 与 `summary` 之间必须使用英文半角冒号 `:`。

示例：

```text
feat: 新增xxx功能
```
