---
name: writing-skills
description: Use when creating new skills, editing existing skills, or verifying skills work before deployment
---

# 编写技能

## 概述

**编写技能就是将测试驱动开发（TDD）应用于流程文档。**

**个人技能存放在代理特定目录中（Claude Code 为 `~/.claude/skills`，Codex 为 `~/.agents/skills/`）**

你编写测试用例（使用子代理的压力场景），观察它们失败（基线行为），编写技能（文档），观察测试通过（代理遵守），然后重构（填补漏洞）。

**核心原则：** 如果你没有观察过代理在没有技能时失败，你就不知道技能是否教会了正确的东西。

**必需背景：** 你必须理解 superpowers:test-driven-development 才能使用此技能。那个技能定义了基本的 RED-GREEN-REFACTOR 循环。此技能将 TDD 适配到文档。

**官方指南：** 关于 Anthropic 官方技能编写最佳实践，参见 anthropic-best-practices.md。该文档提供了补充此技能 TDD 方法的额外模式和指南。

## 什么是技能？

**技能**是已验证技术、模式或工具的参考指南。技能帮助未来的 Claude 实例找到并应用有效的方法。

**技能是：** 可复用的技术、模式、工具、参考指南

**技能不是：** 关于你如何解决某个问题的叙述

## 技能的 TDD 映射

| TDD 概念 | 技能创建 |
|-------------|----------------|
| **测试用例** | 使用子代理的压力场景 |
| **生产代码** | 技能文档 (SKILL.md) |
| **测试失败 (RED)** | 代理在没有技能时违反规则（基线） |
| **测试通过 (GREEN)** | 代理在有技能时遵守规则 |
| **重构** | 在保持合规的同时填补漏洞 |
| **先写测试** | 在编写技能之前运行基线场景 |
| **观察失败** | 记录代理使用的确切合理化借口 |
| **最小代码** | 编写针对特定违规的技能 |
| **观察通过** | 验证代理现在遵守规则 |
| **重构循环** | 发现新的合理化借口 → 填补 → 重新验证 |

整个技能创建过程遵循 RED-GREEN-REFACTOR。

## 何时创建技能

**在以下情况创建：**
- 技术对你来说不是直观明显的
- 你会跨项目再次引用这个技术
- 模式广泛适用（不是项目特定的）
- 其他人会受益

**不要为以下情况创建：**
- 一次性解决方案
- 其他地方已有良好文档的标准实践
- 项目特定约定（放在 CLAUDE.md 中）
- 机械约束（如果可以用正则/验证强制执行，就自动化它——将文档留给需要判断的情况）

## 技能类型

### 技术型
有具体步骤可遵循的方法（基于条件的等待、根因追踪）

### 模式型
思考问题的方式（使用标志扁平化、测试不变量）

### 参考型
API 文档、语法指南、工具文档（Office 文档）

## 目录结构

```
skills/
  skill-name/
    SKILL.md              # 主参考（必需）
    supporting-file.*     # 仅在需要时
```

**扁平命名空间** — 所有技能在一个可搜索的命名空间中

**单独文件用于：**
1. **重型参考**（100+ 行）— API 文档、综合语法
2. **可复用工具** — 脚本、实用程序、模板

**保持内联：**
- 原则和概念
- 代码模式（< 50 行）
- 其他所有内容

## SKILL.md 结构

**Frontmatter (YAML)：**
- 只支持两个字段：`name` 和 `description`
- 最多 1024 字符
- `name`：只使用字母、数字和连字符（不要括号、特殊字符）
- `description`：第三人称，只描述何时使用（不描述做什么）
  - 以 "Use when..." 开头以聚焦触发条件
  - 包含具体的症状、情况和上下文
  - **永远不要总结技能的流程或工作流**（参见 CSO 部分了解原因）
  - 尽可能保持在 500 字符以内

```markdown
---
name: Skill-Name-With-Hyphens
description: Use when [具体触发条件和症状]
---

# 技能名称

## 概述
这是什么？用 1-2 句话说明核心原则。

## 何时使用
[如果不明显，用小型内联流程图]

列出症状和用例的要点
何时不使用

## 核心模式（用于技术/模式型）
前后代码对比

## 快速参考
常用操作的表格或要点

## 实现
简单模式的内联代码
重型参考或可复用工具的文件链接

## 常见错误
什么会出错 + 修复方法

## 实际影响（可选）
具体结果
```


## Claude 搜索优化 (CSO)

**对发现至关重要：** 未来的 Claude 需要找到你的技能

### 1. 丰富的描述字段

**目的：** Claude 读取描述来决定为给定任务加载哪些技能。让它回答："我现在应该读取这个技能吗？"

**格式：** 以 "Use when..." 开头以聚焦触发条件

**关键：描述 = 何时使用，不是技能做什么**

描述应该只描述触发条件。不要在描述中总结技能的流程或工作流。

**为什么这很重要：** 测试表明，当描述总结了技能的工作流时，Claude 可能会遵循描述而不是阅读完整的技能内容。一个说"任务间代码审查"的描述导致 Claude 只做一次审查，尽管技能的流程图清楚地显示有两次审查（规格合规然后代码质量）。

当描述改为只写 "Use when executing implementation plans with independent tasks"（没有工作流总结）时，Claude 正确地阅读了流程图并遵循了两阶段审查流程。

**陷阱：** 总结工作流的描述创造了 Claude 会走的捷径。技能正文变成了 Claude 跳过的文档。

```yaml
# ❌ 不好：总结工作流 - Claude 可能遵循这个而不是阅读技能
description: Use when executing plans - dispatches subagent per task with code review between tasks

# ❌ 不好：太多流程细节
description: Use for TDD - write test first, watch it fail, write minimal code, refactor

# ✅ 好：只有触发条件，没有工作流总结
description: Use when executing implementation plans with independent tasks in the current session

# ✅ 好：只有触发条件
description: Use when implementing any feature or bugfix, before writing implementation code
```

**内容：**
- 使用具体的触发器、症状和情况来表明这个技能适用
- 描述*问题*（竞态条件、不一致行为）而不是*语言特定的症状*（setTimeout, sleep）
- 保持触发器与技术无关，除非技能本身是技术特定的
- 如果技能是技术特定的，在触发器中明确说明
- 用第三人称写（注入到系统提示中）
- **永远不要总结技能的流程或工作流**

```yaml
# ❌ 不好：太抽象、模糊，没有包含何时使用
description: For async testing

# ❌ 不好：第一人称
description: I can help you with async tests when they're flaky

# ❌ 不好：提到技术但技能不是特定于该技术
description: Use when tests use setTimeout/sleep and are flaky

# ✅ 好：以 "Use when" 开头，描述问题，没有工作流
description: Use when tests have race conditions, timing dependencies, or pass/fail inconsistently

# ✅ 好：技术特定的技能有明确的触发器
description: Use when using React Router and handling authentication redirects
```

### 2. 关键词覆盖

使用 Claude 会搜索的词：
- 错误消息："Hook timed out", "ENOTEMPTY", "race condition"
- 症状："flaky", "hanging", "zombie", "pollution"
- 同义词："timeout/hang/freeze", "cleanup/teardown/afterEach"
- 工具：实际命令、库名称、文件类型

### 3. 描述性命名

**使用主动语态，动词优先：**
- ✅ `creating-skills` 不是 `skill-creation`
- ✅ `condition-based-waiting` 不是 `async-test-helpers`

### 4. Token 效率（关键）

**问题：** getting-started 和频繁引用的技能会加载到每个对话中。每个 token 都很重要。

**目标字数：**
- getting-started 工作流：每个 <150 词
- 频繁加载的技能：总共 <200 词
- 其他技能：<500 词（仍要简洁）

**技术：**

**将细节移到工具帮助：**
```bash
# ❌ 不好：在 SKILL.md 中记录所有标志
search-conversations supports --text, --both, --after DATE, --before DATE, --limit N

# ✅ 好：引用 --help
search-conversations supports multiple modes and filters. Run --help for details.
```

**使用交叉引用：**
```markdown
# ❌ 不好：重复工作流细节
When searching, dispatch subagent with template...
[20 行重复的指令]

# ✅ 好：引用其他技能
Always use subagents (50-100x context savings). REQUIRED: Use [other-skill-name] for workflow.
```

**压缩示例：**
```markdown
# ❌ 不好：冗长示例（42 词）
your human partner: "How did we handle authentication errors in React Router before?"
You: I'll search past conversations for React Router authentication patterns.
[Dispatch subagent with search query: "React Router authentication error handling 401"]

# ✅ 好：最小示例（20 词）
Partner: "How did we handle auth errors in React Router?"
You: Searching...
[Dispatch subagent → synthesis]
```

**消除冗余：**
- 不要重复交叉引用技能中的内容
- 不要解释从命令中显而易见的内容
- 不要包含相同模式的多个示例

**验证：**
```bash
wc -w skills/path/SKILL.md
# getting-started workflows: 目标每个 <150
# 其他频繁加载的: 目标总共 <200
```

**按你做的事或核心洞见命名：**
- ✅ `condition-based-waiting` > `async-test-helpers`
- ✅ `using-skills` 不是 `skill-usage`
- ✅ `flatten-with-flags` > `data-structure-refactoring`
- ✅ `root-cause-tracing` > `debugging-techniques`

**动名词 (-ing) 适合流程：**
- `creating-skills`, `testing-skills`, `debugging-with-logs`
- 主动，描述你正在采取的行动

### 4. 交叉引用其他技能

**当编写引用其他技能的文档时：**

只使用技能名称，带有明确的要求标记：
- ✅ 好：`**必需子技能：** 使用 superpowers:test-driven-development`
- ✅ 好：`**必需背景：** 你必须理解 superpowers:systematic-debugging`
- ❌ 不好：`See skills/testing/test-driven-development`（不清楚是否必需）
- ❌ 不好：`@skills/testing/test-driven-development/SKILL.md`（强制加载，消耗上下文）

**为什么不用 @ 链接：** `@` 语法会立即强制加载文件，在你需要之前就消耗 200k+ 上下文。

## 流程图使用

```dot
digraph when_flowchart {
    "需要展示信息？" [shape=diamond];
    "我可能出错的决策？" [shape=diamond];
    "使用 markdown" [shape=box];
    "小型内联流程图" [shape=box];

    "需要展示信息？" -> "我可能出错的决策？" [label="是"];
    "我可能出错的决策？" -> "小型内联流程图" [label="是"];
    "我可能出错的决策？" -> "使用 markdown" [label="否"];
}
```

**只在以下情况使用流程图：**
- 不明显的决策点
- 你可能过早停止的流程循环
- "何时使用 A vs B" 的决策

**永远不要用于：**
- 参考材料 → 表格、列表
- 代码示例 → Markdown 块
- 线性指令 → 编号列表
- 没有语义意义的标签（step1, helper2）

参见 @graphviz-conventions.dot 了解 graphviz 风格规则。

**为你的伙伴可视化：** 使用此目录中的 `render-graphs.js` 将技能的流程图渲染为 SVG：
```bash
./render-graphs.js ../some-skill           # 每个图单独
./render-graphs.js ../some-skill --combine # 所有图在一个 SVG 中
```

## 代码示例

**一个优秀的示例胜过许多平庸的示例**

选择最相关的语言：
- 测试技术 → TypeScript/JavaScript
- 系统调试 → Shell/Python
- 数据处理 → Python

**好的示例：**
- 完整且可运行
- 良好注释，解释为什么
- 来自真实场景
- 清晰展示模式
- 准备好适配（不是通用模板）

**不要：**
- 用 5+ 种语言实现
- 创建填空模板
- 编造的示例

你擅长移植——一个出色的示例就足够了。

## 文件组织

### 自包含技能
```
defense-in-depth/
  SKILL.md    # 所有内容内联
```
当：所有内容都适合，不需要重型参考

### 带可复用工具的技能
```
condition-based-waiting/
  SKILL.md    # 概述 + 模式
  example.ts  # 可适配的工作助手
```
当：工具是可复用代码，不只是叙述

### 带重型参考的技能
```
pptx/
  SKILL.md       # 概述 + 工作流
  pptxgenjs.md   # 600 行 API 参考
  ooxml.md       # 500 行 XML 结构
  scripts/       # 可执行工具
```
当：参考材料太大不适合内联

## 铁律（与 TDD 相同）

```
没有先失败的测试就没有技能
```

这适用于新技能和对现有技能的编辑。

在测试前编写技能？删除它。重新开始。
没有测试就编辑技能？同样的违规。

**没有例外：**
- 不是"简单添加"
- 不是"只是添加一节"
- 不是"文档更新"
- 不要保留未测试的更改作为"参考"
- 不要在运行测试时"适配"
- 删除就是删除

**必需背景：** superpowers:test-driven-development 技能解释了为什么这很重要。同样的原则适用于文档。

## 测试所有类型的技能

不同技能类型需要不同的测试方法：

### 纪律强制型技能（规则/要求）

**示例：** TDD, verification-before-completion, designing-before-coding

**测试方式：**
- 学术问题：他们理解规则吗？
- 压力场景：他们在压力下遵守吗？
- 组合多种压力：时间 + 沉没成本 + 疲劳
- 识别合理化借口并添加明确的反驳

**成功标准：** 代理在最大压力下遵守规则

### 技术型技能（操作指南）

**示例：** condition-based-waiting, root-cause-tracing, defensive-programming

**测试方式：**
- 应用场景：他们能正确应用技术吗？
- 变体场景：他们处理边缘情况吗？
- 缺失信息测试：指令有空白吗？

**成功标准：** 代理成功将技术应用于新场景

### 模式型技能（心智模型）

**示例：** reducing-complexity, information-hiding concepts

**测试方式：**
- 识别场景：他们识别何时模式适用吗？
- 应用场景：他们能使用心智模型吗？
- 反例：他们知道何时不应用吗？

**成功标准：** 代理正确识别何时/如何应用模式

### 参考型技能（文档/API）

**示例：** API 文档、命令参考、库指南

**测试方式：**
- 检索场景：他们能找到正确信息吗？
- 应用场景：他们能正确使用找到的信息吗？
- 空白测试：常见用例都覆盖了吗？

**成功标准：** 代理找到并正确应用参考信息

## 跳过测试的常见合理化借口

| 借口 | 现实 |
|--------|---------|
| "技能明显很清楚" | 对你清楚 ≠ 对其他代理清楚。测试它。 |
| "只是参考文档" | 参考文档可能有空白、不清楚的部分。测试检索。 |
| "测试是小题大做" | 未测试的技能有问题。总是如此。15 分钟测试节省数小时。 |
| "有问题再测试" | 问题 = 代理无法使用技能。在部署前测试。 |
| "测试太繁琐" | 测试比在生产中调试糟糕的技能要少繁琐。 |
| "我确信它是好的" | 过度自信保证有问题。还是要测试。 |
| "学术审查够了" | 阅读 ≠ 使用。测试应用场景。 |
| "没时间测试" | 部署未测试的技能以后修复更浪费时间。 |

**所有这些都意味着：部署前测试。没有例外。**

## 让技能抵抗合理化

强制纪律的技能（如 TDD）需要抵抗合理化。代理很聪明，在压力下会找到漏洞。

**心理学注释：** 理解说服技术为什么有效有助于你系统地应用它们。参见 persuasion-principles.md 了解研究基础（Cialdini, 2021; Meincke et al., 2025）关于权威、承诺、稀缺、社会认同和团结原则。

### 明确关闭每个漏洞

不要只陈述规则——禁止特定的变通方法：

<Bad>
```markdown
Write code before test? Delete it.
```
</Bad>

<Good>
```markdown
Write code before test? Delete it. Start over.

**No exceptions:**
- Don't keep it as "reference"
- Don't "adapt" it while writing tests
- Don't look at it
- Delete means delete
```
</Good>

### 处理"精神 vs 字面"的论点

早期添加基础原则：

```markdown
**Violating the letter of the rules is violating the spirit of the rules.**
**违反规则的字面意思就是违反规则的精神。**
```

这切断了整类"我在遵循精神"的合理化借口。

### 构建合理化表格

从基线测试中捕获合理化借口（参见下面的测试部分）。代理使用的每个借口都进入表格：

```markdown
| Excuse | Reality |
|--------|---------|
| "Too simple to test" | Simple code breaks. Test takes 30 seconds. |
| "I'll test after" | Tests passing immediately prove nothing. |
| "Tests after achieve same goals" | Tests-after = "what does this do?" Tests-first = "what should this do?" |
```

### 创建危险信号列表

让代理在合理化时容易自我检查：

```markdown
## Red Flags - STOP and Start Over

- Code before test
- "I already manually tested it"
- "Tests after achieve the same purpose"
- "It's about spirit not ritual"
- "This is different because..."

**All of these mean: Delete code. Start over with TDD.**
```

### 为违规症状更新 CSO

添加到描述：你即将违反规则时的症状：

```yaml
description: use when implementing any feature or bugfix, before writing implementation code
```

## 技能的 RED-GREEN-REFACTOR

遵循 TDD 循环：

### RED：编写失败的测试（基线）

在没有技能的情况下用子代理运行压力场景。记录确切行为：
- 他们做了什么选择？
- 他们使用了什么合理化借口（逐字记录）？
- 哪些压力触发了违规？

这就是"观察测试失败"——你必须看到代理自然地做什么，然后才能编写技能。

### GREEN：编写最小技能

编写针对那些特定合理化借口的技能。不要为假设情况添加额外内容。

在有技能的情况下运行相同场景。代理现在应该遵守规则。

### REFACTOR：填补漏洞

代理找到了新的合理化借口？添加明确的反驳。重新测试直到无懈可击。

**测试方法论：** 参见 @testing-skills-with-subagents.md 了解完整的测试方法论：
- 如何编写压力场景
- 压力类型（时间、沉没成本、权威、疲劳）
- 系统地填补漏洞
- 元测试技术

## 反模式

### ❌ 叙述性示例
"In session 2025-10-03, we found empty projectDir caused..."
**为什么不好：** 太具体，不可复用

### ❌ 多语言稀释
example-js.js, example-py.py, example-go.go
**为什么不好：** 质量平庸，维护负担

### ❌ 流程图中的代码
```dot
step1 [label="import fs"];
step2 [label="read file"];
```
**为什么不好：** 不能复制粘贴，难以阅读

### ❌ 通用标签
helper1, helper2, step3, pattern4
**为什么不好：** 标签应该有语义意义

## 停止：在移动到下一个技能之前

**编写任何技能后，你必须停止并完成部署流程。**

**不要：**
- 不测试就批量创建多个技能
- 在当前技能验证之前移动到下一个
- 因为"批处理更高效"而跳过测试

**下面的部署检查清单对每个技能都是强制性的。**

部署未测试的技能 = 部署未测试的代码。这是质量标准的违规。

## 技能创建检查清单（TDD 适配版）

**重要：使用 TodoWrite 为下面的每个检查项创建任务。**

**RED 阶段 - 编写失败的测试：**
- [ ] 创建压力场景（纪律型技能需要 3+ 组合压力）
- [ ] 在没有技能的情况下运行场景 - 逐字记录基线行为
- [ ] 识别合理化借口/失败中的模式

**GREEN 阶段 - 编写最小技能：**
- [ ] 名称只使用字母、数字、连字符（不要括号/特殊字符）
- [ ] YAML frontmatter 只有 name 和 description（最多 1024 字符）
- [ ] Description 以 "Use when..." 开头并包含具体触发器/症状
- [ ] Description 用第三人称写
- [ ] 全文关键词便于搜索（错误、症状、工具）
- [ ] 清晰的概述和核心原则
- [ ] 解决 RED 阶段识别的特定基线失败
- [ ] 代码内联或链接到单独文件
- [ ] 一个优秀示例（不要多语言）
- [ ] 在有技能的情况下运行场景 - 验证代理现在遵守规则

**REFACTOR 阶段 - 填补漏洞：**
- [ ] 从测试中识别新的合理化借口
- [ ] 添加明确的反驳（如果是纪律型技能）
- [ ] 从所有测试迭代中构建合理化表格
- [ ] 创建危险信号列表
- [ ] 重新测试直到无懈可击

**质量检查：**
- [ ] 只在决策不明显时使用小型流程图
- [ ] 快速参考表格
- [ ] 常见错误部分
- [ ] 没有叙述性讲故事
- [ ] 支持文件只用于工具或重型参考

**部署：**
- [ ] 将技能提交到 git 并推送到你的 fork（如果配置了）
- [ ] 考虑通过 PR 回馈（如果广泛有用）

## 发现工作流

未来的 Claude 如何找到你的技能：

1. **遇到问题**（"测试不稳定"）
3. **找到技能**（描述匹配）
4. **扫描概述**（这相关吗？）
5. **阅读模式**（快速参考表格）
6. **加载示例**（只在实现时）

**为这个流程优化** — 早期和经常放置可搜索的术语。

## 总结

**创建技能就是流程文档的 TDD。**

同样的铁律：没有先失败的测试就没有技能。
同样的循环：RED（基线）→ GREEN（编写技能）→ REFACTOR（填补漏洞）。
同样的好处：更好的质量、更少的意外、无懈可击的结果。

如果你对代码遵循 TDD，就对技能遵循 TDD。这是应用于文档的同样纪律。
