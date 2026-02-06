# Claude Code CLI 命令参考

> **版本**: 2.1.34
> **基本用法**: `claude [选项] [子命令] [提示词]`
> 直接运行 `claude` 进入交互式会话。

---

## 1. 会话管理

日常开发最常用的一组参数。

| 命令 | 作用 | 示例 |
|:---|:---|:---|
| `claude` | 启动新的交互式会话 | `claude` |
| `claude "提示词"` | 启动会话并发送第一条消息 | `claude "帮我看看这个项目"` |
| `claude -c` | 继续当前目录下最近一次会话 | `claude -c` |
| `claude --resume` | 弹出会话选择器，选择要恢复的会话 | `claude --resume` |
| `claude --resume <id>` | 恢复指定 ID 的会话 | `claude --resume abc123` |
| `claude --fork-session` | 恢复时创建新会话 ID（分叉，不修改原会话） | `claude -c --fork-session` |
| `claude --from-pr` | 恢复关联到某个 PR 的会话 | `claude --from-pr 123` |
| `claude --session-id <uuid>` | 使用指定的 UUID 作为会话 ID | `claude --session-id xxx-xxx` |

---

## 2. 权限模式

控制 Claude 在会话中能做什么。也可在会话中通过 `Shift+Tab` 切换。

| 参数 | 模式 | 说明 |
|:---|:---|:---|
| `--permission-mode default` | Normal | 每次编辑请求批准（默认） |
| `--permission-mode plan` | Plan | 只讨论不编辑文件 |
| `--permission-mode acceptEdits` | Auto-accept | 自动接受所有编辑 |
| `--permission-mode dontAsk` | Don't Ask | 不询问，自主执行 |
| `--permission-mode delegate` | Delegate | 委托模式 |
| `--permission-mode bypassPermissions` | Bypass | 跳过所有权限检查 |

> `--dangerously-skip-permissions` 是 `bypassPermissions` 的快捷方式，仅限无网络的沙盒环境使用。
> `--allow-dangerously-skip-permissions` 启用该选项但不默认激活。

---

## 3. 模型与输出

| 参数 | 作用 | 示例 |
|:---|:---|:---|
| `--model <model>` | 指定本次会话的模型 | `--model opus` 或 `--model sonnet` |
| `-p, --print` | 非交互模式：输出结果后退出（适合管道） | `claude -p "解释这段代码"` |
| `--output-format <格式>` | 输出格式（仅 `--print`） | `text`（默认）、`json`、`stream-json` |
| `--input-format <格式>` | 输入格式（仅 `--print`） | `text`（默认）、`stream-json` |
| `--json-schema <schema>` | 结构化输出的 JSON Schema 验证 | `--json-schema '{"type":"object",...}'` |
| `--max-budget-usd <金额>` | API 调用最大花费（仅 `--print`） | `--max-budget-usd 5` |
| `--fallback-model <model>` | 默认模型过载时自动切换（仅 `--print`） | `--fallback-model sonnet` |
| `--verbose` | 启用详细输出模式 | `--verbose` |

---

## 4. 工具与权限控制

| 参数 | 作用 | 示例 |
|:---|:---|:---|
| `--allowed-tools <工具...>` | 允许使用的工具白名单 | `--allowed-tools "Bash(git:*) Edit"` |
| `--disallowed-tools <工具...>` | 禁止使用的工具黑名单 | `--disallowed-tools "Bash"` |
| `--tools <工具...>` | 指定可用的内置工具集 | `""` 禁用全部，`"default"` 全部启用 |
| `--disable-slash-commands` | 禁用所有 skill（斜杠命令） | `--disable-slash-commands` |

---

## 5. 系统提示与上下文

| 参数 | 作用 | 示例 |
|:---|:---|:---|
| `--system-prompt <提示词>` | 替换系统提示词 | `--system-prompt "你是一个代码审查员"` |
| `--append-system-prompt <提示词>` | 追加到默认系统提示词之后 | `--append-system-prompt "始终使用中文回答"` |
| `--add-dir <目录...>` | 添加额外目录到工具访问范围 | `--add-dir ../shared-lib` |
| `--file <文件...>` | 启动时下载文件资源 | `--file file_abc:doc.txt` |

---

## 6. Agent 与插件

| 参数 | 作用 | 示例 |
|:---|:---|:---|
| `--agent <agent>` | 指定本次会话使用的 Agent | `--agent reviewer` |
| `--agents <json>` | 以 JSON 定义自定义 Agent | `--agents '{"reviewer":{"prompt":"..."}}'` |
| `--plugin-dir <路径...>` | 临时加载指定目录的插件 | `--plugin-dir ./my-plugin` |

---

## 7. MCP 配置

| 参数 | 作用 | 示例 |
|:---|:---|:---|
| `--mcp-config <配置...>` | 从 JSON 文件或字符串加载 MCP 服务器 | `--mcp-config mcp.json` |
| `--strict-mcp-config` | 仅使用 `--mcp-config` 指定的 MCP，忽略其他 | `--strict-mcp-config` |

---

## 8. 调试

| 参数 | 作用 | 示例 |
|:---|:---|:---|
| `-d, --debug [过滤器]` | 启用调试模式，可选类别过滤 | `--debug "api,hooks"` 或 `--debug "!1p"` |
| `--debug-file <路径>` | 将调试日志写入文件 | `--debug-file ./debug.log` |

---

## 9. 其他

| 参数 | 作用 |
|:---|:---|
| `--chrome` | 启用 Claude in Chrome 集成 |
| `--no-chrome` | 禁用 Chrome 集成 |
| `--ide` | 自动连接 IDE（如果恰好有一个可用 IDE） |
| `--no-session-persistence` | 禁用会话持久化（仅 `--print`） |
| `--settings <文件或JSON>` | 加载额外的配置文件 |
| `--setting-sources <来源>` | 指定加载的配置来源（user, project, local） |
| `-v, --version` | 显示版本号 |
| `-h, --help` | 显示帮助信息 |

---

## 10. 子命令

### claude doctor

检查 Claude Code 自动更新器的健康状态。

```bash
claude doctor
```

### claude install

安装 Claude Code 原生构建版本。

```bash
claude install                # 安装稳定版
claude install latest         # 安装最新版
claude install 2.1.30         # 安装指定版本
claude install --force        # 强制重装
```

### claude update

检查并安装更新。

```bash
claude update
```

### claude setup-token

设置长期认证 token（需要 Claude 订阅）。

```bash
claude setup-token
```

### claude mcp — MCP 服务器管理

```bash
claude mcp list                              # 列出所有已配置的 MCP 服务器
claude mcp get <名称>                         # 查看某个 MCP 的详情
claude mcp add <名称> <命令或URL> [参数...]    # 添加 MCP 服务器
claude mcp add-json <名称> <json>             # 用 JSON 字符串添加 MCP
claude mcp add-from-claude-desktop            # 从 Claude Desktop 导入 MCP
claude mcp remove <名称>                      # 移除 MCP 服务器
claude mcp reset-project-choices              # 重置项目级 MCP 审批状态
claude mcp serve                              # 启动 Claude Code 自身作为 MCP 服务器
```

**添加 MCP 示例**：

```bash
# 添加 HTTP 类型的 MCP
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp

# 添加带认证头的 HTTP MCP
claude mcp add --transport http corridor https://app.corridor.dev/api/mcp \
  --header "Authorization: Bearer xxx"

# 添加 stdio 类型的 MCP，带环境变量
claude mcp add -e API_KEY=xxx my-server -- npx my-mcp-server
```

### claude plugin — 插件管理

```bash
claude plugin list                            # 列出已安装的插件
claude plugin install <插件>                   # 安装插件
claude plugin uninstall <插件>                 # 卸载插件
claude plugin enable <插件>                    # 启用插件
claude plugin disable <插件>                   # 禁用插件
claude plugin update <插件>                    # 更新插件
claude plugin validate <路径>                  # 验证插件配置
```

**插件市场管理**：

```bash
claude plugin marketplace list                # 列出所有市场
claude plugin marketplace add <来源>           # 添加市场（URL、路径或 GitHub 仓库）
claude plugin marketplace remove <名称>        # 移除市场
claude plugin marketplace update [名称]        # 更新市场（不指定名称则更新全部）
```

---

## 11. 交互式会话内快捷键

在 Claude Code 交互式界面中可用：

| 快捷键 | 作用 |
|:---|:---|
| `Shift+Tab` | 循环切换权限模式：Normal → Plan → Auto-accept |
| `/` + 命令名 | 执行斜杠命令（如 `/plan`, `/commit`, `/help`） |
| `Ctrl+C` | 中断当前生成 |
| `Ctrl+D` | 退出会话 |

---

## 12. 常用组合示例

```bash
# 继续上次的工作
claude -c

# 用 Plan 模式启动，讨论方案不动代码
claude --permission-mode plan

# 用 Opus 模型做架构设计
claude --model opus

# 非交互模式：让 Claude 分析代码并输出 JSON
claude -p "分析这个项目的依赖关系" --output-format json

# 限制预算的批量任务
claude -p "重构 src/ 下所有工具函数" --max-budget-usd 10

# 带额外目录访问权限
claude --add-dir ../common-lib --add-dir ../proto

# 用自定义 Agent 启动
claude --agent reviewer

# 从 PR 恢复相关会话
claude --from-pr 456
```
