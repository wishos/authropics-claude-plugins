# Agent SDK 开发插件

用于在 Python 和 TypeScript 中创建和验证 Claude Agent SDK 应用程序的综合插件。

## 概述

Agent SDK 开发插件简化了构建 Agent SDK 应用程序的整个生命周期，从初始脚手架到根据最佳实践进行验证。它帮助您快速启动具有最新 SDK 版本的项目，并确保您的应用程序遵循官方文档模式。

## 功能

### 命令：`/new-sdk-app`

交互式命令，引导您创建新的 Claude Agent SDK 应用程序。

**功能：**
- 询问关于您项目的问题（语言、名称、agent 类型、起点）
- 检查并安装最新 SDK 版本
- 创建所有必要的项目文件和配置
- 设置适当的环境文件（.env.example, .gitignore）
- 提供适合您用例的工作示例
- 运行类型检查（TypeScript）或语法验证（Python）
- 使用适当的验证 agent 自动验证设置

**用法：**
```bash
/new-sdk-app my-project-name
```

或者简单：
```bash
/new-sdk-app
```

该命令将交互式询问您：
1. 语言选择（TypeScript 或 Python）
2. 项目名称（如果未提供）
3. Agent 类型（coding、business、custom）
4. 起点（minimal、basic 或特定示例）
5. 工具偏好（npm/yarn/pnpm 或 pip/poetry）

**示例：**
```bash
/new-sdk-app customer-support-agent
# → 为客户支持 agent 创建新的 Agent SDK 项目
# → 设置 TypeScript 或 Python 环境
# → 安装最新 SDK 版本
# → 自动验证设置
```

### Agent：`agent-sdk-verifier-py`

彻底验证 Python Agent SDK 应用程序的正确设置和最佳实践。

**验证检查：**
- SDK 安装和版本
- Python 环境设置（requirements.txt、pyproject.toml）
- 正确的 SDK 使用和模式
- Agent 初始化和配置
- 环境和安全性（.env、API 密钥）
- 错误处理和功能
- 文档完整性

**使用时机：**
- 创建新的 Python SDK 项目后
- 修改现有的 Python SDK 应用程序后
- 部署 Python SDK 应用程序之前

**用法：**
Agent 在 `/new-sdk-app` 创建 Python 项目后自动运行，或者您可以通过以下方式触发它：
```
"验证我的 Python Agent SDK 应用程序"
"检查我的 SDK 应用程序是否遵循最佳实践"
```

**输出：**
提供综合报告：
- 总体状态（PASS / PASS WITH WARNINGS / FAIL）
- 阻止功能的严重问题
- 关于次优模式的警告
- 通过检查列表
- 带有 SDK 文档引用的具体建议

### Agent：`agent-sdk-verifier-ts`

彻底验证 TypeScript Agent SDK 应用程序的正确设置和最佳实践。

**验证检查：**
- SDK 安装和版本
- TypeScript 配置（tsconfig.json）
- 正确的 SDK 使用和模式
- 类型安全和导入
- Agent 初始化和配置
- 环境和安全性（.env、API 密钥）
- 错误处理和功能
- 文档完整性

**使用时机：**
- 创建新的 TypeScript SDK 项目后
- 修改现有的 TypeScript SDK 应用程序后
- 部署 TypeScript SDK 应用程序之前

**用法：**
Agent 在 `/new-sdk-app` 创建 TypeScript 项目后自动运行，或者您可以通过以下方式触发它：
```
"验证我的 TypeScript Agent SDK 应用程序"
"检查我的 SDK 应用程序是否遵循最佳实践"
```

**输出：**
提供综合报告：
- 总体状态（PASS / PASS WITH WARNINGS / FAIL）
- 阻止功能的严重问题
- 关于次优模式的警告
- 通过检查列表
- 带有 SDK 文档引用的具体建议

## 工作流示例

以下是使用此插件的典型工作流：

1. **创建新项目：**
```bash
/new-sdk-app code-reviewer-agent
```

2. **回答交互式问题：**
```
语言：TypeScript
Agent 类型：Coding agent（代码审查）
起点：具有常见功能的基础 agent
```

3. **自动验证：**
命令自动运行 `agent-sdk-verifier-ts` 以确保一切设置正确。

4. **开始开发：**
```bash
# 设置您的 API 密钥
echo "ANTHROPIC_API_KEY=your_key_here" > .env

# 运行您的 agent
npm start
```

5. **更改后验证：**
```
"验证我的 SDK 应用程序"
```

## 安装

此插件包含在 Claude Code 仓库中。要使用它：

1. 确保已安装 Claude Code
2. 插件命令和 Agent 自动可用

## 最佳实践

- **始终使用最新 SDK 版本**：`/new-sdk-app` 检查并安装最新版本
- **部署前验证**：在部署到生产环境之前运行验证 agent
- **保持 API 密钥安全**：永不提交 `.env` 文件或硬编码 API 密钥
- **遵循 SDK 文档**：验证 agent 根据官方模式进行检查
- **定期检查 TypeScript 项目类型**：运行 `npx tsc --noEmit`
- **测试您的 agent**：为 agent 的功能创建测试用例

## 资源

- [Agent SDK 概述](https://docs.claude.com/zh-CN/api/agent-sdk/overview)
- [TypeScript SDK 参考](https://docs.claude.com/zh-CN/api/agent-sdk/typescript)
- [Python SDK 参考](https://docs.claude.com/zh-CN/api/agent-sdk/python)
- [Agent SDK 示例](https://docs.claude.com/zh-CN/api/agent-sdk/examples)

## 故障排除

### TypeScript 项目中的类型错误

**问题**：TypeScript 项目创建后有类型错误

**解决方案**：
- `/new-sdk-app` 命令自动运行类型检查
- 如果错误仍然存在，检查您是否使用最新 SDK 版本
- 验证您的 `tsconfig.json` 匹配 SDK 要求

### Python 导入错误

**问题**：无法从 `claude_agent_sdk` 导入

**解决方案**：
- 确保已安装依赖：`pip install -r requirements.txt`
- 如果使用虚拟环境则激活它
- 检查 SDK 是否已安装：`pip show claude-agent-sdk`

### 验证失败并有警告

**问题**：验证 agent 报告警告

**解决方案**：
- 审查报告中具体的警告
- 检查提供的 SDK 文档引用
- 警告不会阻止功能，但表示需要改进的领域

## 作者

Ashwin Bhat (ashwin@anthropic.com)

## 版本

1.0.0
