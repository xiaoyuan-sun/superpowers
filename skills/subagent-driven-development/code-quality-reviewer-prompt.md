# 代码质量审查者提示模板

派遣代码质量审查子代理时使用此模板。

**目的：** 验证实现构建良好（干净、有测试、可维护）

**仅在规格合规审查通过后派遣。**

```
Task tool (superpowers:code-reviewer):
  Use template at requesting-code-review/code-reviewer.md

  WHAT_WAS_IMPLEMENTED: [来自实现者的报告]
  PLAN_OR_REQUIREMENTS: [计划文件]中的任务 N
  BASE_SHA: [任务前的提交]
  HEAD_SHA: [当前提交]
  DESCRIPTION: [任务摘要]
```

**除了标准代码质量关注点，审查者应该检查：**
- 每个文件是否有一个清晰的职责和定义良好的接口？
- 单元是否分解为可以独立理解和测试？
- 实现是否遵循计划中的文件结构？
- 这个实现是否创建了已经很大的新文件，或显著增加了现有文件的大小？（不要标记预先存在的文件大小 — 关注这个更改贡献了什么。）

**代码审查者返回：** 优点、问题（关键/重要/次要）、评估