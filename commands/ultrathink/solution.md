---
name: ultrathink-solution
description: ultrathink 全面分析，给出方案
argument-hint: "[问题描述]"
disable-model-invocation: true
user-invocable: true
---

## 用户输入

```text
$ARGUMENTS
```

你**必须**先考虑用户输入(如果不为空)。

## 任务目标

ultrathink 模式，全面深度分析，给出解决方案。有需要澄清先询问我，不要立刻修改实现。

## 执行规则
- 如果提供的文档是业务技术分析，那么需要根据根据当前项目代码，检查文档是否有优化、新增、更新、废弃、冗余、错误等等的地方
