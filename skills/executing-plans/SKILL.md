---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# 执行计划

## 概述

加载计划，批判性审查，执行所有任务，完成时报告。

**开始时宣布：** "我正在使用 executing-plans 技能实现此计划。"

**注意：** 告诉你的伙伴，Superpowers 在有子代理访问权限时工作得更好。如果在支持子代理的平台（如 Claude Code 或 Codex）上运行，其工作质量会显著更高。如果有子代理可用，改用 superpowers:subagent-driven-development。

## 流程

### 步骤 1：加载和审查计划
1. 读取计划文件
2. 批判性审查 - 识别对计划的任何问题或顾虑
3. 如果有顾虑：在开始前向你的伙伴提出
4. 如果没有顾虑：创建 TodoWrite 并继续

### 步骤 2：执行任务

对于每个任务：
1. 标记为进行中
2. 精确遵循每一步（计划有小口大小的步骤）
3. 按指定运行验证
4. 标记为已完成

### 步骤 3：完成开发

所有任务完成并验证后：
- 宣布："我正在使用 finishing-a-development-branch 技能完成这项工作。"
- **必需子技能：** 使用 superpowers:finishing-a-development-branch
- 遵循该技能验证测试、展示选项、执行选择

## 何时停止并请求帮助

**立即停止执行当：**
- 遇到阻碍（缺失依赖、测试失败、指令不清楚）
- 计划有阻止开始的关键空白
- 你不理解某个指令
- 验证反复失败

**请求澄清而不是猜测。**

## 何时返回早期步骤

**返回审查（步骤 1）当：**
- 伙伴根据你的反馈更新计划
- 根本方法需要重新思考

**不要硬闯阻碍** — 停下来问。

## 记住
- 先批判性审查计划
- 精确遵循计划步骤
- 不要跳过验证
- 计划说引用技能时引用
- 阻塞时停止，不要猜测
- 永远不要在没有明确用户同意的情况下在 main/master 分支上开始实现

## 集成

**必需工作流技能：**
- **superpowers:using-git-worktrees** - 必需：在开始前设置隔离工作空间
- **superpowers:writing-plans** - 创建此技能执行的计划
- **superpowers:finishing-a-development-branch** - 所有任务完成后完成开发
