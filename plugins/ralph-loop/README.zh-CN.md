# Ralph Loop 插件

在 Claude Code 中实现 Ralph Wiggum 技术的迭代式自引用 AI 开发循环。

## 什么是 Ralph Loop？

Ralph Loop 是一种基于持续 AI Agent 循环的开发方法。正如 Geoffrey Huntley 所描述的：**"Ralph 是一个 Bash 循环"** - 一个简单的 `while true`，它反复将提示文件馈送给 AI Agent，允许它迭代地改进其工作直到完成。

该技术受 Ralph Wiggum 编码技术启发（以《辛普森一家》中的角色命名），体现了尽管遇到挫折仍坚持迭代的理念。

### 核心概念

此插件使用 **Stop hook** 来拦截 Claude 的退出尝试：

```bash
# 您运行一次：
/ralph-loop "您的任务描述" --completion-promise "DONE"

# 然后 Claude Code 自动：
# 1. 处理任务
# 2. 尝试退出
# 3. Stop hook 阻止退出
# 4. Stop hook 将相同的提示重新馈送
# 5. 重复直到完成
```

循环发生在**您当前会话内部** - 您不需要外部 bash 循环。`hooks/stop-hook.sh` 中的 Stop hook 通过阻止正常会话退出来创建自引用反馈循环。

这创建了一个**自引用反馈循环**：
- 提示在迭代之间从不更改
- Claude 的先前工作保存在文件中
- 每次迭代都能看到修改后的文件和 git 历史
- Claude 通过在文件中阅读自己先前的工作来自主改进

## 快速开始

```bash
/ralph-loop "构建一个待办事项 REST API。需求：CRUD 操作、输入验证、测试。完成后输出 <promise>COMPLETE</promise>。" --completion-promise "COMPLETE" --max-iterations 50
```

Claude 将：
- 迭代实现 API
- 运行测试并查看失败
- 根据测试输出修复 bug
- 迭代直到满足所有要求
- 完成后输出完成承诺

## 命令

### /ralph-loop

在当前会话中启动 Ralph 循环。

**用法：**
```bash
/ralph-loop "<prompt>" --max-iterations <n> --completion-promise "<text>"
```

**选项：**
- `--max-iterations <n>` - N 次迭代后停止（默认：无限制）
- `--completion-promise <text>` - 表示完成的短语

### /cancel-ralph

取消活动的 Ralph 循环。

**用法：**
```bash
/cancel-ralph
```

## 提示编写最佳实践

### 1. 明确的完成标准

❌ 不好："构建一个待办 API 并使其良好"

✅ 好：
```markdown
构建一个待办事项 REST API。

完成后：
- 所有 CRUD 端点正常工作
- 输入验证已到位
- 测试通过（覆盖率 > 80%）
- 包含 API 文档的 README
- 输出：<promise>COMPLETE</promise>
```

### 2. 增量目标

❌ 不好："创建一个完整的电子商务平台"

✅ 好：
```phase 1：用户认证（JWT、测试）
阶段 2：产品目录（列表/搜索、测试）
阶段 3：购物车（添加/移除、测试）

所有阶段完成后输出 <promise>COMPLETE</promise>
```

### 3. 自我纠正

❌ 不好："为功能 X 编写代码"

✅ 好：
```markdown
按照 TDD 实现功能 X：
1. 编写失败的测试
2. 实现功能
3. 运行测试
4. 如有任何失败，调试并修复
5. 如需要则重构
6. 重复直到全部绿色
7. 输出：<promise>COMPLETE</promise>
```

### 4. 脱困出口

始终使用 `--max-iterations` 作为安全网以防止在不可能完成的任务上无限循环：

```bash
# 建议：始终设置合理的迭代限制
/ralph-loop "尝试实现功能 X" --max-iterations 20

# 在您的提示中，包含如果卡住该怎么办：
# "15 次迭代后，如未完成：
#  - 记录什么阻止了进展
#  - 列出尝试过的方法
#  - 建议替代方法"
```

**注意**：`--completion-promise` 使用精确字符串匹配，因此您不能将其用于多个完成条件（如 "SUCCESS" vs "BLOCKED"）。始终将 `--max-iterations` 作为您的主要安全机制。

## 理念

Ralph 体现了几个关键原则：

### 1. 迭代 > 完美
不要在第一次尝试时就追求完美。让循环改进工作。

### 2. 失败是数据
"确定性坏"意味着失败是可预测的且有信息的。用它们来调整提示。

### 3. 操作员技能很重要
成功取决于编写好的提示，而不仅仅是有一个好的模型。

### 4. 坚持就是胜利
继续尝试直到成功。循环自动处理重试逻辑。

## 何时使用 Ralph

**适用于：**
- 具有明确成功标准的定义良好的任务
- 需要迭代和改进的任务（如让测试通过）
- 您可以离开的全新项目
- 具有自动验证的任务（测试、linter）

**不适用于：**
- 需要人工判断或设计决策的任务
- 一次性操作
- 成功标准不清楚的任务
- 生产调试（使用有针对性的调试代替）

## 实际结果

- 在 Y Combinator 黑客马拉松测试中成功在一夜之间生成了 6 个仓库
- 一个 5 万美元的合同以 297 美元的 API 成本完成
- 使用这种方法在 3 个月内创建了完整的编程语言（"cursed"）

## 了解更多

- 原始技术：https://ghuntley.com/ralph/
- Ralph Orchestrator：https://github.com/mikeyobrien/ralph-orchestrator

## 获取帮助

在 Claude Code 中运行 `/help` 获取详细的命令参考和示例。
