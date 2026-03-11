# Greptile

[Greptile](https://greptile.com) 是一个用于 GitHub 和 GitLab 的 AI 代码审查代理，可自动审查 pull request。此插件将 Claude Code 连接到您的 Greptile 账户，让您可以直接从终端查看和解决 Greptile 的审查评论。

## 设置

### 1. 创建 Greptile 账户

在 [greptile.com](https://greptile.com) 注册并连接您的 GitHub 或 GitLab 仓库。

### 2. 获取您的 API 密钥

1. 前往 [API 设置](https://app.greptile.com/settings/api)
2. 生成新的 API 密钥
3. 复制密钥

### 3. 设置环境变量

添加到您的 shell 配置文件（`.bashrc`、`.zshrc` 等）：

```bash
export GREPTILE_API_KEY="your-api-key-here"
```

然后重新加载您的 shell 或运行 `source ~/.zshrc`。

## 可用工具

### Pull Request 工具
- `list_pull_requests` - 列出 PR，可按仓库、分支、作者或状态过滤
- `get_merge_request` - 获取详细的 PR 信息，包括审查分析
- `list_merge_request_comments` - 获取 PR 上的所有评论，可过滤

### 代码审查工具
- `list_code_reviews` - 列出代码审查，可选过滤
- `get_code_review` - 获取详细的代码审查信息
- `trigger_code_review` - 在 PR 上启动新的 Greptile 审查

### 评论搜索
- `search_greptile_comments` - 搜索所有 Greptile 审查评论

### 自定义上下文工具
- `list_custom_context` - 列出您组织的编码模式和规则
- `get_custom_context` - 获取特定模式的详细信息
- `search_custom_context` - 按内容搜索模式
- `create_custom_context` - 创建新的编码模式

## 使用示例

让 Claude Code：
- "显示我当前 PR 上的 Greptile 评论并帮助我解决它们"
- "Greptile 在 PR #123 上发现了什么问题？"
- "在此分支上触发 Greptile 审查"

## 文档

更多信息，请访问 [greptile.com/docs](https://greptile.com/docs)。
