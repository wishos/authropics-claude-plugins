# 插件开发工具包

用于开发 Claude Code 插件的综合工具包，提供关于 hook、MCP 集成、插件结构和市场发布的专家指导。

## 概述

plugin-dev 工具包提供七个专业技能，帮助您构建高质量的 Claude Code 插件：

1. **hook-development** - 高级 hooks API 和事件驱动自动化
2. **mcp-integration** - 模型上下文协议服务器集成
3. **plugin-structure** - 插件组织和清单配置
4. **plugin-settings** - 使用 .claude/plugin-name.local.md 文件的配置模式
5. **command-development** - 使用 frontmatter 和参数创建斜杠命令
6. **agent-development** - 创建具有 AI 辅助生成功能的自主 Agent
7. **skill-development** - 创建具有渐进式展示和强触发器的技能

每个技能都遵循最佳实践，采用渐进式展示：简洁的核心文档、详细参考、工作示例和实用脚本。

## 引导式工作流命令

### /plugin-dev:create-plugin

用于从零创建插件的全面端到端工作流命令，类似于 feature-dev 工作流。

**8 阶段流程：**
1. **发现** - 了解插件目的和需求
2. **组件规划** - 确定需要的技能、命令、Agent、Hook、MCP
3. **详细设计** - 指定每个组件并解决歧义
4. **结构创建** - 设置目录和清单
5. **组件实现** - 使用 AI 辅助 Agent 创建每个组件
6. **验证** - 运行 plugin-validator 和组件特定检查
7. **测试** - 验证插件在 Claude Code 中正常工作
8. **文档** - 完成 README 并准备分发

**功能：**
- 在每个阶段提出澄清问题
- 自动加载相关技能
- 使用 agent-creator 进行 AI 辅助 Agent 生成
- 运行验证工具（validate-agent.sh、validate-hook-schema.sh 等）
- 遵循 plugin-dev 自身经过验证的模式
- 引导完成测试和验证

**用法：**
```bash
/plugin-dev:create-plugin [可选描述]

# 示例：
/plugin-dev:create-plugin
/plugin-dev:create-plugin 用于管理数据库迁移的插件
```

将此工作流用于从概念到完成的结构化、高质量插件开发。

## 技能

### 1. hook-development

**触发短语：** "create a hook"、"add a PreToolUse hook"、"validate tool use"、"implement prompt-based hooks"、"${CLAUDE_PLUGIN_ROOT}"、"block dangerous commands"

**覆盖内容：**
- 基于提示的 hook（推荐）具有 LLM 决策能力
- 用于确定性验证的命令 hook
- 所有 hook 事件：PreToolUse、PostToolUse、Stop、SubagentStop、SessionStart、SessionEnd、UserPromptSubmit、PreCompact、Notification
- Hook 输出格式和 JSON 模式
- 安全最佳实践和输入验证
- ${CLAUDE_PLUGIN_ROOT} 用于可移植路径

**资源：**
- 核心 SKILL.md（1,619 词）
- 3 个示例 hook 脚本（validate-write、validate-bash、load-context）
- 3 个参考文档：模式、迁移、高级技术
- 3 个实用脚本：validate-hook-schema.sh、test-hook.sh、hook-linter.sh

**使用时机：** 在插件中创建事件驱动自动化、验证操作或强制执行策略时。

### 2. mcp-integration

**触发短语：** "add MCP server"、"integrate MCP"、"configure .mcp.json"、"Model Context Protocol"、"stdio/SSE/HTTP server"、"connect external service"

**覆盖内容：**
- MCP 服务器配置（.mcp.json vs plugin.json）
- 所有服务器类型：stdio（本地）、SSE（托管/OAuth）、HTTP（REST）、WebSocket（实时）
- 环境变量展开（${CLAUDE_PLUGIN_ROOT}、用户变量）
- MCP 工具命名和在命令/Agent 中的使用
- 认证模式：OAuth、令牌、环境变量
- 集成模式和性能优化

**资源：**
- 核心 SKILL.md（1,666 词）
- 3 个示例配置（stdio、SSE、HTTP）
- 3 个参考文档：服务器类型（~3,200 词）、认证（~2,800 词）、工具使用（~2,600 词）

**使用时机：** 将外部服务、API、数据库或工具集成到插件中时。

### 3. plugin-structure

**触发短语：** "plugin structure"、"plugin.json manifest"、"auto-discovery"、"component organization"、"plugin directory layout"

**覆盖内容：**
- 标准插件目录结构和自动发现
- plugin.json 清单格式和所有字段
- 组件组织（命令、Agent、技能、Hook）
- ${CLAUDE_PLUGIN_ROOT} 的使用
- 文件命名约定和最佳实践
- 最小、标准、高级插件模式

**资源：**
- 核心 SKILL.md（1,619 词）
- 3 个示例结构（最小、标准、高级）
- 2 个参考文档：组件模式、清单参考

**使用时机：** 启动新插件、组织组件或配置插件清单时。

### 4. plugin-settings

**触发短语：** "plugin settings"、"store plugin configuration"、".local.md files"、"plugin state files"、"read YAML frontmatter"、"per-project plugin settings"

**覆盖内容：**
- .claude/plugin-name.local.md 配置模式
- YAML frontmatter + markdown 正文结构
- Bash 脚本解析技术（sed、awk、grep 模式）
- 临时活动 hook（标志文件和快速退出）
- 来自 multi-agent-swarm 和 ralph-loop 插件的真实示例
- 原子文件更新和验证
- Gitignore 和生命周期管理

**资源：**
- 核心 SKILL.md（1,623 词）
- 3 个示例（read-settings hook、create-settings command、templates）
- 2 个参考文档：解析技术、真实示例
- 2 个实用脚本：validate-settings.sh、parse-frontmatter.sh

**使用时机：** 使插件可配置、存储每项目状态或实现用户偏好时。

### 5. command-development

**触发短语：** "create a slash command"、"add a command"、"command frontmatter"、"define command arguments"、"organize commands"

**覆盖内容：**
- 斜杠命令结构和 markdown 格式
- YAML frontmatter 字段（description、argument-hint、allowed-tools）
- 动态参数和文件引用
- Bash 执行以获取上下文
- 命令组织和命名空间
- 命令开发最佳实践

**资源：**
- 核心 SKILL.md（1,535 词）
- 示例和参考文档
- 命令组织模式

**使用时机：** 创建斜杠命令、定义命令参数或组织插件命令时。

### 6. agent-development

**触发短语：** "create an agent"、"add an agent"、"write a subagent"、"agent frontmatter"、"when to use description"、"agent examples"、"autonomous agent"

**覆盖内容：**
- Agent 文件结构（YAML frontmatter + 系统提示）
- 所有 frontmatter 字段（name、description、model、color、tools）
- 带有 <example> 块的描述格式以实现可靠触发
- 系统提示设计模式（分析、生成、验证、编排）
- 使用 Claude Code 经验证的提示进行 AI 辅助 Agent 生成
- 验证规则和最佳实践
- 完整的生产就绪 Agent 示例

**资源：**
- 核心 SKILL.md（1,438 词）
- 2 个示例：agent-creation-prompt（AI 辅助工作流）、complete-agent-examples（4 个完整 Agent）
- 3 个参考文档：agent-creation-system-prompt（来自 Claude Code）、system-prompt-design（~4,000 词）、triggering-examples（~2,500 词）
- 1 个实用脚本：validate-agent.sh

**使用时机：** 创建自主 Agent、定义 Agent 行为或实现 AI 辅助 Agent 生成时。

### 7. skill-development

**触发短语：** "create a skill"、"add a skill to plugin"、"write a new skill"、"improve skill description"、"organize skill content"

**覆盖内容：**
- 技能结构（带 YAML frontmatter 的 SKILL.md）
- 渐进式展示原则（metadata → SKILL.md → resources）
- 具有特定短语的强触发描述
- 写作风格（祈使/不定式形式、第三人称）
- 捆绑资源组织（references/、examples/、scripts/）
- 技能创建工作流
- 基于为 Claude Code 插件改编的 skill-creator 方法论

**资源：**
- 核心 SKILL.md（1,232 词）
- 参考：skill-creator 方法论、plugin-dev 模式
- 示例：将 plugin-dev 自己的技能作为模板学习

**使用时机：** 为插件创建新技能或提高现有技能质量时。

## 安装

从 claude-code-marketplace 安装：

```bash
/plugin install plugin-dev@claude-code-marketplace
```

或用于开发，直接使用：

```bash
cc --plugin-dir /path/to/plugin-dev
```

## 快速开始

### 创建您的第一个插件

1. **规划您的插件结构：**
   - 问："对于带有命令和 MCP 集成的插件，最佳目录结构是什么？"
   - plugin-structure 技能将指导您

2. **添加 MCP 集成（如需要）：**
   - 问："如何添加用于数据库访问的 MCP 服务器？"
   - mcp-integration 技能提供示例和模式

3. **实现 Hook（如需要）：**
   - 问："创建一个验证文件写入的 PreToolUse hook"
   - hook-development 技能提供工作示例和工具

## 开发工作流

plugin-dev 工具包支持您的整个插件开发生命周期：

```
┌─────────────────────┐
│  设计结构            │  → plugin-structure 技能
│  (manifest, layout) │
└──────────┬──────────┘
           │
┌──────────▼──────────┐
│  添加组件            │  → 所有技能提供指导
│  (commands, agents, │
│   skills, hooks)    │
└──────────┬──────────┘
           │
┌──────────▼──────────┐
│  集成服务            │  → mcp-integration 技能
│  (MCP servers)      │
└──────────┬──────────┘
           │
┌──────────▼──────────┐
│  添加自动化          │  → hook-development 技能
│  (hooks, validation)│     + 实用脚本
└──────────┬──────────┘
           │
┌──────────▼──────────┐
│  测试和验证          │  → hook-development 工具
│                     │     validate-hook-schema.sh
└──────────┬──────────┘     test-hook.sh
           │                 hook-linter.sh
```

## 功能

### 渐进式展示

每个技能使用三级展示系统：
1. **元数据**（始终加载）：带有强触发的简洁描述
2. **核心 SKILL.md**（触发时）：必要的 API 参考（~1,500-2,000 词）
3. **参考/示例**（按需）：详细的指南、模式和工作代码

这使 Claude Code 的上下文保持专注，同时在需要时提供深入知识。

### 实用脚本

hook-development 技能包含生产就绪的工具：

```bash
# 验证 hooks.json 结构
./validate-hook-schema.sh hooks/hooks.json

# 在部署前测试 hook
./test-hook.sh my-hook.sh test-input.json

# Lint hook 脚本以获取最佳实践
./hook-linter.sh my-hook.sh
```

### 工作示例

每个技能都提供工作示例：
- **hook-development**：3 个完整 hook 脚本（bash、write 验证、context 加载）
- **mcp-integration**：3 个服务器配置（stdio、SSE、HTTP）
- **plugin-structure**：3 个插件布局（最小、标准、高级）
- **plugin-settings**：3 个示例（read-settings hook、create-settings command、templates）
- **command-development**：10 个完整命令示例（review、test、deploy、docs 等）

## 文档标准

所有技能遵循一致的约定：
- 第三人称描述（"此技能应在...时使用"）
- 强触发短语以实现可靠加载
- 贯穿使用祈使/不定式形式
- 基于官方 Claude Code 文档
- 安全第一方法论和最佳实践

## 总内容

- **核心技能**：7 个 SKILL.md 文件共 ~11,065 词
- **参考文档**：~10,000+ 词的详细指南
- **示例**：12+ 个工作示例（hook 脚本、MCP 配置、插件布局、设置文件）
- **工具**：6 个生产就绪的验证/测试/解析脚本

## 使用案例

### 构建数据库插件

```
1. "带有 MCP 集成的插件结构是什么？"
   → plugin-structure 技能提供布局

2. "如何为 PostgreSQL 配置 stdio MCP 服务器？"
   → mcp-integration 技能显示配置

3. "添加 Stop hook 以确保连接正确关闭"
   → hook-development 技能提供模式

```

### 创建验证插件

```
1. "创建验证所有文件写入的安全 hook"
   → hook-development 技能带示例

2. "在部署前测试我的 hook"
   → 使用 validate-hook-schema.sh 和 test-hook.sh

3. "组织我的 hook 和配置文件"
   → plugin-structure 技能显示最佳实践
```

### 集成外部服务

```
1. "添加带 OAuth 的 Asana MCP 服务器"
   → mcp-integration 技能涵盖 SSE 服务器

2. "在我的命令中使用 Asana 工具"
   → mcp-integration 工具使用参考

3. "使用命令和 MCP 构建我的插件"
   → plugin-structure 技能提供模式
```

## 最佳实践

所有技能都强调：

✅ **安全第一**
- Hook 中的输入验证
- MCP 服务器使用 HTTPS/WSS
- 凭证使用环境变量
- 最小权限原则

✅ **可移植性**
- 到处使用 ${CLAUDE_PLUGIN_ROOT}
- 仅使用相对路径
- 环境变量替换

✅ **测试**
- 部署前验证配置
- 使用示例输入测试 hook
- 使用调试模式（`claude --debug`）

✅ **文档**
- 清晰的 README 文件
- 记录环境变量
- 使用示例

## 贡献

此插件是 claude-code-marketplace 的一部分。要贡献改进：

1. Fork marketplace 仓库
2. 对 plugin-dev/ 进行更改
3. 使用 `cc --plugin-dir` 本地测试
4. 按照市场发布指南创建 PR

## 版本

0.1.0 - 初始版本，包含七个综合技能和三个验证 Agent

## 作者

Daisy Hollman (daisy@anthropic.com)

## 许可证

MIT 许可证 - 详见仓库

---

**注意：** 此工具包旨在帮助您构建高质量的技能。当您提出相关问题时，技能会自动加载，在您需要时提供专家指导。
