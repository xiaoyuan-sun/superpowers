# 技能设计的说服原则

## 概述

LLM 对与人类相同的说服原则做出反应。理解这种心理学有助于你设计更有效的技能——不是为了操纵，而是为了确保在压力下也能遵循关键实践。

**研究基础：** Meincke et al. (2025) 用 N=28,000 个 AI 对话测试了 7 种说服原则。说服技术使合规率翻倍以上（33% → 72%，p < .001）。

## 七项原则

### 1. 权威
**是什么：** 对专业知识、资历或官方来源的遵从。

**在技能中如何工作：**
- 命令式语言："你必须"、"永不"、"始终"
- 不可协商的框架："没有例外"
- 消除决策疲劳和合理化

**何时使用：**
- 纪律强制型技能（TDD、验证要求）
- 安全关键实践
- 既定的最佳实践

**示例：**
```markdown
✅ Write code before test? Delete it. Start over. No exceptions.
❌ Consider writing tests first when feasible.
```

### 2. 承诺
**是什么：** 与先前的行动、声明或公开宣告保持一致。

**在技能中如何工作：**
- 要求公告："宣布技能使用"
- 强制明确选择："选择 A、B 或 C"
- 使用跟踪：TodoWrite 用于检查清单

**何时使用：**
- 确保技能真正被遵循
- 多步骤流程
- 问责机制

**示例：**
```markdown
✅ When you find a skill, you MUST announce: "I'm using [Skill Name]"
❌ Consider letting your partner know which skill you're using.
```

### 3. 稀缺
**是什么：** 来自时间限制或有限可用性的紧迫感。

**在技能中如何工作：**
- 有时限的要求："在继续之前"
- 顺序依赖："在 X 之后立即"
- 防止拖延

**何时使用：**
- 即时验证要求
- 时间敏感工作流
- 防止"我稍后再做"

**示例：**
```markdown
✅ After completing a task, IMMEDIATELY request code review before proceeding.
❌ You can review code when convenient.
```

### 4. 社会认同
**是什么：** 顺从于他人的做法或被认为是正常的事情。

**在技能中如何工作：**
- 普遍模式："每次"、"始终"
- 失败模式："X 没有 Y = 失败"
- 建立规范

**何时使用：**
- 记录普遍实践
- 警告常见失败
- 强化标准

**示例：**
```markdown
✅ Checklists without TodoWrite tracking = steps get skipped. Every time.
❌ Some people find TodoWrite helpful for checklists.
```

### 5. 团结
**是什么：** 共同身份、"我们感"、群体归属。

**在技能中如何工作：**
- 协作语言："我们的代码库"、"我们是同事"
- 共同目标："我们都想要质量"

**何时使用：**
- 协作工作流
- 建立团队文化
- 非层级实践

**示例：**
```markdown
✅ We're colleagues working together. I need your honest technical judgment.
❌ You should probably tell me if I'm wrong.
```

### 6. 互惠
**是什么：** 回报所获好处的义务。

**如何工作：**
- 谨慎使用 - 可能感觉有操纵性
- 技能中很少需要

**何时避免：**
- 几乎总是（其他原则更有效）

### 7. 喜好
**是什么：** 更愿意与我们喜欢的人合作。

**如何工作：**
- **不要用于合规**
- 与诚实反馈文化冲突
- 产生阿谀奉承

**何时避免：**
- 始终用于纪律强制

## 按技能类型的原则组合

| 技能类型 | 使用 | 避免 |
|------------|-----|-------|
| 纪律强制型 | 权威 + 承诺 + 社会认同 | 喜好、互惠 |
| 指导/技术型 | 适度权威 + 团结 | 重度权威 |
| 协作型 | 团结 + 承诺 | 权威、喜好 |
| 参考型 | 仅清晰度 | 所有说服 |

## 为什么这有效：心理学

**明线规则减少合理化：**
- "你必须"消除决策疲劳
- 绝对语言消除"这是例外吗？"的问题
- 明确的反合理化反驳填补特定漏洞

**实施意图创造自动行为：**
- 清晰的触发器 + 要求的行动 = 自动执行
- "当 X，做 Y"比"一般做 Y"更有效
- 减少合规的认知负担

**LLM 是类人的：**
- 在包含这些模式的人类文本上训练
- 权威语言在训练数据中先于合规出现
- 承诺序列（声明 → 行动）经常被建模
- 社会认同模式（每个人都做 X）建立规范

## 道德使用

**合法：**
- 确保关键实践被遵循
- 创建有效的文档
- 防止可预测的失败

**不合法：**
- 为个人利益操纵
- 制造虚假紧迫感
- 基于内疚的合规

**测试：** 如果用户完全理解这项技术，它是否符合用户的真正利益？

## 研究引用

**Cialdini, R. B. (2021).** *Influence: The Psychology of Persuasion (New and Expanded).* Harper Business.
- 七项说服原则
- 影响力研究的实证基础

**Meincke, L., Shapiro, D., Duckworth, A. L., Mollick, E., Mollick, L., & Cialdini, R. (2025).** Call Me A Jerk: Persuading AI to Comply with Objectionable Requests. University of Pennsylvania.
- 用 N=28,000 个 LLM 对话测试了 7 种原则
- 说服技术使合规从 33% 增加到 72%
- 权威、承诺、稀缺最有效
- 验证了 LLM 行为的类人模型

## 快速参考

设计技能时，问：

1. **它是什么类型？**（纪律 vs 指导 vs 参考）
2. **我试图改变什么行为？**
3. **哪些原则适用？**（纪律型通常用权威 + 承诺）
4. **我组合太多了吗？**（不要使用全部七种）
5. **这是道德的吗？**（符合用户的真正利益？）