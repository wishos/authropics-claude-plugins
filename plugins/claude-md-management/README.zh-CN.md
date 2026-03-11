# CLAUDE.md 管理插件

用于维护和改进 CLAUDE.md 文件的工具——审计质量、捕获会话学习并保持项目记忆最新。

## 功能

两个互补工具用于不同目的：

| | claude-md-improver（技能）| /revise-claude-md（命令）|
|---|---|---|
| **目的** | 保持 CLAUDE.md 与代码库同步 | 捕获会话学习 |
| **触发方式** | 代码库更改 | 会话结束 |
| **使用时机** | 定期维护 | 会话显示缺少上下文 |

## 使用方法

### 技能：claude-md-improver

根据当前代码库状态审计 CLAUDE.md 文件：

```
"审计我的 CLAUDE.md 文件"
"检查我的 CLAUDE.md 是否是最新的"
```

<img src="claude-md-improver-example.png" alt="CLAUDE.md 改进器显示质量分数和推荐更新" width="600">

### 命令：/revise-claude-md

捕获当前会话的学习成果：

```
/revise-claude-md
```

<img src="revise-claude-md-example.png" alt="修订命令将会话学习捕获到 CLAUDE.md" width="600">

## 作者

Isabella He (isabella@anthropic.com)
