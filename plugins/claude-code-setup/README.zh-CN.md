# Claude Code 设置插件

分析代码库并推荐定制的 Claude Code 自动化——hook、技能、MCP 服务器等。

## 功能

Claude 使用此技能扫描您的代码库并推荐每个类别的前 1-2 个自动化：

- **MCP 服务器** - 外部集成（用于文档的 context7、用于前端的 Playwright）
- **技能** - 打包的专业知识（Plan agent、frontend-design）
- **Hook** - 自动操作（自动格式化、自动 lint、阻止敏感文件）
- **Subagent** - 专业审查者（安全、性能、可访问性）
- **Slash 命令** - 快速工作流（/test、/pr-review、/explain）

此技能是**只读的** - 它分析但不修改文件。

## 使用方法

```
"为此项目推荐自动化"
"帮助我设置 Claude Code"
"我应该使用什么 hook？"
```

<img src="automation-recommender-example.png" alt="自动化推荐器分析代码库并提供定制推荐" width="600">

## 作者

Isabella He (isabella@anthropic.com)
