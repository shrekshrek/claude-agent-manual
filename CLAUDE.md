# Claude Code 智能体协作手册 (claude-agent-manual)

## 项目简介
中文文档项目，将 Claude Code 的最佳实践整理为可执行的工作流手册和技能参考。

## 文件结构与职责

| 文件 | 性质 | 内容 |
|:---|:---|:---|
| `README.md` | 技能参考 | 技能矩阵、安装指南、工作流速查（回答"有什么工具"） |
| `WORKFLOW.md` | 实操指南 | 项目生命周期、模块开发流程、会话管理（回答"怎么跑完一个项目"） |
| `REFERENCE-BORIS-WORKFLOW.md` | 参考文档 | Boris Cherny 的个人工作流（事实记录） |
| `REFERENCE-ECC-WORKFLOW.md` | 参考文档 | everything-claude-code 推荐工作流（事实记录） |
| `REFERENCE-LONG-RUNNING-AGENTS.md` | 参考文档 | Anthropic 官方长周期 Agent 方案（事实记录） |
| `REFERENCE-CLI.md` | 参考文档 | Claude Code CLI 命令速查 |

## 编辑约定

### REFERENCE 文档 vs WORKFLOW 文档
- **REFERENCE-*.md 是事实记录**：忠实描述原始来源的做法，不掺入我们自己的观点或策略
- **WORKFLOW.md 是我们的总结**：综合各来源，形成自有的工作流建议
- 两者有分歧时以 WORKFLOW.md 为准，但不回溯修改 REFERENCE 文档的事实描述

### 写作规范
- 语言：中文为主，技术术语保留英文（如 CLAUDE.md、Plan Mode、/commit）
- 交叉引用使用"见 X.Y 节"格式，确保 section 编号准确
- Mermaid 流程图不在 subgraph 内使用 `direction TB`（GitHub 渲染兼容）
- 表格使用左对齐 `:---`

### CLAUDE.md 策略（本手册核心观点）
- CLAUDE.md = 静态配置（技术选型、编码约定、常见错误），< 300 行
- PROGRESS.md = 动态进度（模块状态、下次入口），每次会话更新
- docs/architecture.md = 全局设计详情（架构图、数据模型、模块边界、设计决策）
- docs/plan.md = 当前模块的实施方案（临时，每模块覆盖更新）
- 子目录 CLAUDE.md 利用 Claude Code 内置自动加载机制（确定性），优于手动路由指针（~56% 执行率）

## 注意事项
- 修改 WORKFLOW.md 后须检查内部一致性（flowchart、表格、正文、交叉引用）
- 修改 REFERENCE 文档时只更新事实错误或补充遗漏，不改变原始来源的观点
- 所有 flowchart 修改后应在 GitHub 上验证 Mermaid 渲染
