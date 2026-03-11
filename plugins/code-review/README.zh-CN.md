# 代码审查插件 (Code Review Plugin)

使用多个专业 Agent 进行基于置信度评分的自动代码审查，用于过滤误报。

## 概述

代码审查插件通过并行启动多个 Agent 从不同角度独立审查代码变更，自动化 pull request 审查流程。它使用置信度评分来过滤误报，确保只发布高质量、可操作的反馈。

## 命令

### `/code-review`

使用多个专业 Agent 执行自动代码审查。

**功能：**
1. 检查是否需要审查（跳过已关闭、草稿、 trivial 或已审查的 PR）
2. 收集仓库中相关的 CLAUDE.md 指南文件
3. 总结 pull request 变更
4. 并行启动 4 个 Agent 进行独立审查：
   - 和 #2**：审查 CLAUDE.md 合规性
   - **Agent # **Agent #13**：扫描变更中的明显 bug
   - **Agent #4**：通过 git blame/history 分析基于上下文的issue
5. 为每个问题打分 0-100 的置信度
6. 过滤掉低于 80 置信度阈值的问题
7. 仅发布高置信度问题的审查评论

**用法：**
```bash
/code-review
```

**工作流程示例：**
```bash
# 在 PR 分支上运行：
/code-review

# Claude 将：
# - 并行启动 4 个审查 Agent
# - 为每个问题评分置信度
# - 发布置信度 ≥80 的问题的评论
# - 如果没有高置信度问题则跳过发布
```

**功能特点：**
- 多个独立 Agent 进行全面审查
- 基于置信度的评分减少误报（阈值：80）
- CLAUDE.md 合规性检查与明确的指南验证
- 专注于变更中的 bug 检测（非预先存在的问题）
- 通过 git blame 进行历史上下文分析
- 自动跳过已关闭、草稿或已审查的 PR
- 直接链接到带有完整 SHA 和行范围的代码

**审查评论格式：**
```markdown
## 代码审查

发现 3 个问题：

1. OAuth 回调缺少错误处理（CLAUDE.md 规定"始终处理 OAuth 错误"）

https://github.com/owner/repo/blob/abc123.../src/auth.ts#L67-L72

2. 内存泄漏：OAuth 状态未清理（由于 finally 块中缺少清理导致的 bug）

https://github.com/owner/repo/blob/abc123.../src/auth.ts#L88-L95

3. 不一致的命名模式（src/conventions/CLAUDE.md 规定"函数使用 camelCase"）

https://github.com/owner/repo/blob/abc123.../src/utils.ts#L23-L28
```

**置信度评分：**
- **0**：不确定，误报
- **25**：不太确定，可能是真的
- **50**：中等置信度，真的但轻微
- **75**：高度置信度，真的且重要
- **100**：绝对确定，绝对是真的

**被过滤的误报：**
- PR 中未引入的预先存在的问题
- 看起来像 bug 但实际不是的代码
- 吹毛求疵的细节
- linter 会捕获的问题
- 一般质量问题（除非在 CLAUDE.md 中）
- 带 lint 忽略注释的问题

## 安装

此插件包含在 Claude Code 仓库中。使用 Claude Code 时命令自动可用。

## 最佳实践

### 使用 `/code-review`
- 维护清晰的 CLAUDE.md 文件以便更好地进行合规性检查
- 信任 80+ 置信度阈值 - 误报已被过滤
- 对所有非平凡的 pull request 运行
- 将 Agent 发现作为人工审查的起点
- 根据 recurring 审查模式更新 CLAUDE.md

### 使用场景
- 所有有实质性变更的 pull request
- 涉及关键代码路径的 PR
- 来自多个贡献者的 PR
- 注重指南合规性的 PR

### 不使用场景
- 已关闭或草稿的 PR（自动跳过）
- 自动化的 trivial PR（自动跳过）
- 需要立即合并的紧急 hotfix
- 已审查的 PR（自动跳过）

## 工作流集成

### 标准 PR 审查工作流：
```bash
# 创建包含变更的 PR
/code-review

# 审查自动反馈
# 进行必要的修复
# 准备合并时
```

### 作为 CI/CD 的一部分：
```bash
# 在 PR 创建或更新时触发
# 自动发布审查评论
# 如果已存在审查则跳过
```

## 要求

- 带有 GitHub 集成的 Git 仓库
- 已安装并认证的 GitHub CLI (`gh`)
- CLAUDE.md 文件（可选但建议使用以进行指南检查）

## 故障排除

### 审查时间过长

**问题**：大型 PR 的 Agent 运行缓慢

**解决方案**：
- 大型变更这是正常的 - Agent 并行运行
- 4 个独立 Agent 确保彻底性
- 考虑将大型 PR 拆分为较小的 PR

### 误报过多

**问题**：审查标记的问题不是真的

**解决方案**：
- 默认阈值是 80（已经过滤了大部分误报）
- 使 CLAUDE.md 更具体地说明重要内容
- 考虑标记的问题是否实际有效

### 没有发布审查评论

**问题**：`/code-review` 运行了但没有出现评论

**解决方案**：
检查以下情况：
- PR 已关闭（跳过审查）
- PR 是草稿（跳过审查）
- PR 是 trivial/自动化（跳过审查）
- PR 已有审查（跳过审查）
- 没有问题评分 ≥80（不需要评论）

### 链接格式损坏

**问题**：代码链接在 GitHub 中正确渲染

**解决方案**：
链接必须遵循此精确格式：
```
https://github.com/owner/repo/blob/[full-sha]/path/file.ext#L[start]-L[end]
```
- 必须使用完整 SHA（不是缩写）
- 必须使用 `#L` 表示法
- 必须包含至少 1 行上下文的行范围

### GitHub CLI 不工作

**问题**：`gh` 命令失败

**解决方案**：
- 安装 GitHub CLI：`brew install gh`（macOS）或参阅 [GitHub CLI 安装](https://cli.github.com/)
- 认证：`gh auth login`
- 验证仓库有 GitHub remote

## 提示

- **编写具体的 CLAUDE.md 文件**：清晰的指南 = 更好的审查
- **在 PR 中包含上下文**：帮助 Agent 理解意图
- **使用置信度评分**：≥80 的问题通常是正确的
- **迭代指南**：根据模式更新 CLAUDE.md
- **自动审查**：作为 PR 工作流的一部分设置
- **信任过滤**：阈值防止噪音

## 配置

### 调整置信度阈值

默认阈值是 80。要调整，请修改 `commands/code-review.md` 中的命令文件：
```markdown
过滤掉任何分数低于 80 的问题。
```

将 `80` 更改为您首选的阈值 (0-100)。

### 自定义审查焦点

编辑 `commands/code-review.md` 以添加或修改 Agent 任务：
- 添加专注于安全的 Agent
- 添加性能分析 Agent
- 添加可访问性检查 Agent
- 添加文档质量检查

## 技术细节

### Agent 架构
- **2x CLAUDE.md 合规性 Agent**：指南检查的冗余
- **1x bug 检测器**：仅专注于变更中的明显 bug
- **1x 历史分析器**：来自 git blame 和历史的上下文
- **Nx 置信度评分器**：每个问题一个进行独立评分

### 评分系统
- 每个问题独立评分 0-100
- 评分考虑证据强度和验证
- 阈值（默认 80）过滤低置信度问题
- 对于 CLAUDE.md 问题：验证指南明确提及它

### GitHub 集成
使用 `gh` CLI：
- 查看 PR 详情和差异
- 获取仓库数据
- 读取 git blame 和历史
- 发布审查评论

## 作者

Boris Cherny (boris@anthropic.com)

## 版本

1.0.0
