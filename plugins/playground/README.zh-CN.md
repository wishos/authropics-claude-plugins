# Playground 插件

创建交互式 HTML playground——独立的单文件探索器，让用户通过控件直观地配置某些内容，查看实时预览，然后复制生成的提示。

## 什么是 Playground？

Playground 是一个独立的 HTML 文件，包含：
- 一侧的交互式控件
- 另一侧的实时预览
- 底部带有复制按钮的提示输出

用户调整控件，直观地探索，然后复制生成的提示回传给 Claude。

## 使用时机

当用户请求交互式 playground、探索器或某主题的可视化工具时使用此插件——特别是当输入空间很大、是可视化的或结构化的、难以用纯文本表达时。

## 模板

该技能包含常见 playground 类型的模板：
- **design-playground** — 可视化设计决策（组件、布局、间距、颜色、字体排版）
- **data-explorer** — 数据和查询构建（SQL、API、管道、正则表达式）
- **concept-map** — 学习和探索（概念图、知识缺口、范围映射）
- **document-critique** — 文档审查（带批准/拒绝/评论工作流的建议）

## 安装

将此插件添加到您的 Claude Code 配置以启用 playground 技能。
