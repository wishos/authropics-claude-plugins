# Claude Code 插件目录

高质量 Claude Code 插件精选目录。

> **⚠️ 重要提示：** 在安装、更新或使用插件之前，请确保您信任该插件。Anthropic 无法控制插件中包含的 MCP 服务器、文件或其他软件，也无法验证它们是否按预期工作或是否会发生变化。更多信息请参阅各插件的主页。

## 目录结构

- **`/plugins`** - Anthropic 开发和维护的内部插件
- **`/external_plugins`** - 合作伙伴和社区提供的第三方插件

## 安装

插件可通过 Claude Code 的插件系统直接安装。

安装方法：
运行 `/plugin install {plugin-name}@claude-plugin-directory`

或在 `/plugin > Discover` 中浏览插件

## 贡献指南

### 内部插件

内部插件由 Anthropic 团队成员开发。请参阅 `/plugins/example-plugin` 作为参考实现。

### 外部插件

第三方合作伙伴可提交插件以加入插件市场。外部插件需要满足质量和安全标准才能获批。如需提交新插件，请使用 [插件目录提交表单](https://clau.de/plugin-directory-submission)。

## 插件结构

每个插件遵循标准结构：

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json      # 插件元数据（必需）
├── .mcp.json            # MCP 服务器配置（可选）
├── commands/            # 斜杠命令（可选）
├── agents/              # Agent 定义（可选）
├── skills/              # Skill 定义（可选）
└── README.md            # 文档
```

## 许可证

请参阅各插件链接中的 LICENSE 文件。

## 文档

有关开发 Claude Code 插件的更多信息，请参阅[官方文档](https://code.claude.com/docs/zh-CN/plugins)。
