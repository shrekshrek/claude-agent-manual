# Claude Code 智能体协作手册 (Agent Team Manual)

> **版本**: 2026.02 (Ultimate Edition)
> **目标**: 将 Claude Code 打造为全栈、全能、全流程的虚拟研发团队。

---

## 1. 核心架构与技能来源 (Core Architecture & Sources)

本环境集成了目前开源社区最强的组件，共同构成了您的 AI 结对编程伙伴。我们在后文的技能矩阵中，使用以下标签来标识技能的来源：

### 🧬 来源分类 (Sources Legend)

1.  **`Anthropic` (官方增强)**
    *   **来源**: Anthropic 官方插件库 (`claude-code/plugins`)。
    *   **代表**: **Feature Dev** (全流程开发), **Frontend Design** (UI 生成)。
    *   **特点**: 血统纯正，深度集成，质量最高。

2.  **`Antigravity` (硬核工程)**
    *   **来源**: `antigravity-awesome-skills` (GitHub)。
    *   **代表**: **Architect Review** (架构评审), **DDD** (领域驱动), **Docker**。
    *   **特点**: 专注于后端架构、DevOps 和复杂工程化场景。

3.  **`Vendor` (厂商最佳实践)**
    *   **来源**: 技术厂商官方维护 (`skills.sh` Verified)。
    *   **代表**: **Vercel**, **Supabase**, **Expo**。
    *   **特点**: 权威的框架使用规范，确保代码符合官方最佳实践。

4.  **`Community` (社区生态)**
    *   **来源**: `skills.sh` Top 100 Charts。
    *   **代表**: **Mermaid** (绘图), **PDF**, **Tailwind**。
    *   **特点**: 覆盖面广，解决特定领域的具体工具需求。

5.  **`MCP` (感知连接器)**
    *   **来源**: Model Context Protocol (Official/Third-party)。
    *   **代表**: **Browser Use** (浏览器操作), **Context7** (文档查询)。
    *   **特点**: 连接外部世界，赋予 AI 操作浏览器和读取最新文档的能力。

6.  **`Core` (核心大脑)**
    *   **来源**: `everything-claude-code`。
    *   **代表**: `/plan`, `/tdd` 等核心指令。
    *   **特点**: 提供标准的 SOP 工作流和虚拟角色切换能力。


---

## 2. 内置能力 (Built-in Capabilities)

由 `everything-claude-code` 插件直接提供，无需额外调用，根据指令自动触发。

### 🤖 虚拟角色 (Agents)
AI 会根据上下文自动切换身份：

| 角色 | 触发指令 | 职责 |
| :--- | :--- | :--- |
| **Architect** (架构师) | `/plan` | 系统设计、技术选型、模块划分 |
| **Planner** (规划师) | `/plan` | 拆解任务、生成 `PLAN.md` 实施文档 |
| **TDD Guide** (测试导师) | `/tdd` | 指导“红-绿-重构”开发流程 |
| **Code Reviewer** (审查员) | `/code-review` | 代码质量检查、风格规范审查 |
| **Security Reviewer** (安全员) | `/security-review` | 检查 SQL 注入、XSS 等安全漏洞 |
| **Refactor Cleaner** (重构员) | `/refactor` | 识别坏味道、清理死代码 |

### ⚡ 核心指令 (Commands)
在 Claude CLI 输入框中直接使用的斜杠命令：

*   **`/plan "需求"`**: **[轻量级]** 启动规划，生成详细步骤 (基于 everything-claude-code)。
*   **`/feature-dev "需求"`**: **[重量级]** 启动 7 阶段全流程开发 (基于 Anthropic 官方插件)，包含深度代码探索与多方案架构设计。
*   **`/tdd "功能"`**: 进入测试驱动开发模式。
*   **`/fix`**: 自动修复当前的报错或构建失败。
*   **`/review`**: 发起代码审查（别名 `/code-review`）。
*   **`/commit`**: 智能分析变更，自动生成规范的 Commit Message 并提交。
*   **`/learn`**: 提取当前会话的经验教训并持久化保存。

---

## 3. 全局技能矩阵 (Skill Matrix)

以下技能均已全局安装至 `~/.agents/skills/`，Claude 可随时调用。（来源标签说明请见第一章）

### 🔥 官方增强 (Official Power-Ups)
| 技能名称 | 来源 (Source) | 作用与能力 |
| :--- | :--- | :--- |
| **Feature Dev** | `Anthropic` | 全流程功能开发智能体，包含 Discovery, Exploration, Architecture 阶段 |
| **PR Review Toolkit** | `Anthropic` | 包含 6 个维度的深度审查专家 (注释、测试覆盖率、错误处理、类型设计等) |
| **Commit Commands** | `Anthropic` | 智能生成符合项目风格的 Commit Message |

### 🎨 前端与交互 (Frontend & Design)
| 技能名称 | 来源 (Source) | 作用与能力 |
| :--- | :--- | :--- |
| **Frontend Design** | `Anthropic` | 生成高质量、符合现代审美的前端 UI 代码 |
| **Vercel React/Next.js** | `Vercel` | Vercel 官方工程规范：Server Actions, RSC, 性能优化 (Rank 1) |
| **Tailwind Design System** | `Community` | 构建可扩展的 Tailwind 原子化设计系统 |
| **3D Web Experience** | `Community` | 打造 Three.js / React Three Fiber 3D 交互体验 |
| **Nuxt 4 Expert** | `Community` | 掌握 Nuxt 4 服务端路由、中间件与配置最佳实践 |
| **Nuxt UI** | `Community` | 熟练使用 Nuxt UI v4 组件库构建界面 |
| **Vue Debug Guides** | `Community` | 解决 Vue 3 / Nuxt 复杂响应式丢失、Hydration Mismatch 问题 |
| **Frontend Dev Guidelines**| `Community` | 前端开发规范与最佳实践 (原 Frontend Architect) |
| **Expo/React Native UI** | `Expo` | 构建原生级移动端 UI (Expo Router, Animations, Native Tabs) |
| **Canvas Design** | `Community` | 编程式绘图与 HTML5 Canvas 复杂操作 |
| **Web Design Guidelines** | `Community` | 审查 UI 是否符合 Web 界面设计规范 |

### 🧱 后端、架构与运维 (Backend, Architecture & DevOps)
| 技能名称 | 来源 (Source) | 作用与能力 |
| :--- | :--- | :--- |
| **FastAPI Expert** | `Community` | 异步 API 开发、Pydantic V2 验证、依赖注入模式 |
| **Supabase Best Practices**| `Supabase` | Postgres 数据库性能优化、RLS 安全策略设计 |
| **Backend Architect** | `Antigravity`| 后端微服务拆分、高可用系统设计、DDD 落地 |
| **Architect Review (DDD)** | `Antigravity`| 真正的软件架构师专家，精通 Clean Architecture、微服务与 DDD 评审 |
| **Docker Expert** | `Antigravity`| 编写最佳实践 Dockerfile、多阶段构建、镜像瘦身 |
| **GitHub Actions Templates**| `Antigravity`| 生成生产级 CI/CD 工作流，自动化构建、测试与部署 |
| **Testing Patterns** | `Antigravity`| Jest 测试模式、TDD 工作流、Mock 策略专家 |
| **API Security** | `Community` | OWASP Top 10 防护、JWT 鉴权、速率限制实现 |

### 🤖 AI 工程与大模型 (AI Engineering)
| 技能名称 | 来源 (Source) | 作用与能力 |
| :--- | :--- | :--- |
| **AI Engineer** | `Community` | 构建 RAG 系统、Agent 编排、Prompt 优化、向量库设计 |
| **MCP Builder** | `Community` | 指导构建自定义 Model Context Protocol (MCP) 服务器 |
| **Skill Creator** | `Community` | 指导编写新的 Claude Skill |

### 🛠️ 办公与通用工具 (Utilities & Office)
| 技能名称 | 来源 (Source) | 作用与能力 |
| :--- | :--- | :--- |
| **Brainstorming** | `Community` | 头脑风暴模式，辅助 `/plan` 前的发散性思维与创意构思 |
| **PDF / Docx / PPTX** | `Community` | 读取、分析、生成 PDF/Word/PPT 文档 |
| **Mermaid Diagrams** | `Community` | 自动生成架构图、流程图、时序图、ER 图 |
| **Copywriting** | `Community` | 撰写营销文案、润色技术文档、优化 UX 文案 |
| **GitHub** | `GitHub CLI` | (需配置 `gh` CLI) 管理 Issue、PR、查看 Actions 状态 |
| **Code Review** | `Community` | 基于最佳实践清单的自动化代码审查 |
| **Find Skills** | `Community` | 查找和发现更多适合当前任务的技能 |

### 🌐 审计与网络 (Audit & Web)
| 技能名称 | 来源 (Source) | 作用与能力 |
| :--- | :--- | :--- |
| **Agent Browser** | `Community` | 自动化网页浏览、截图、交互 |
| **Webapp Testing** | `Community` | 使用 Playwright 进行本地 Web 应用的自动化测试与调试 |
| **Browser Use (MCP)** | `MCP` | **[强力推荐]** 全自动网页智能体，支持复杂交互与视觉识别 |
| **SEO Audit** | `Community` | 网站 SEO 健康度诊断与优化建议 |
| **Audit Website** | `Community` | 全面网站技术指标与性能体检 |

---

## 4. 标准工作流 (SOP)

我们推荐根据任务的复杂度，在以下两种模式中选择：

### 模式 A：轻量级快速迭代 (Standard P-C-R-L)
**适用**: Bug 修复、小功能调整、已有模块的维护。

1. **Plan**: `/plan "修改登录页样式"` (生成步骤)
2. **Code**: `/implement` 或 `/tdd` (执行开发)
3. **Review**: `/review` (快速检查)
4. **Learn**: `/learn` (记录经验)

### 模式 B：深度功能开发 (Agentic Feature-Dev)
**适用**: 全新功能开发、复杂模块重构、涉及多个文件修改的大任务。

1. **Start**: `/feature-dev "实现 OAuth 第三方登录"`
2. **Auto-Process**:
   - **Discovery**: AI 会追问你需求细节。
   - **Exploration**: 自动派遣 Agent 扫码全库。
   - **Architecture**: 提供 2-3 种技术方案供你选择。
   - **Implementation**: 确认后自动编写代码。
3. **Review**: 自动触发 Code Review。

---

## 5. 环境配置 (Configuration)

### 全局路径
*   **Skills**: `~/.agents/skills/` (所有技能源码都在这里，可手动修改 `SKILL.md` 定制)
*   **Rules**: `~/.claude/rules/` (ECC 的核心规则文件，定义 AI 角色与行为)
*   **Config**: `~/.claude/settings.json` (Claude Code 的主配置文件，含插件开关与环境变量)

### 推荐 MCP 配置
为了获得最强体验，建议在 `~/.claude/settings.json` 中添加以下 MCP：

#### 1. Context7 (文档增强)
让 AI 实时查阅最新技术文档。
```json
{
  "enabledPlugins": {
    "context7@claude-plugins-official": true
  },
  "env": {
    "CONTEXT7_API_KEY": "sk-xxxxxxxxx"
  }
}
```

#### 2. Browser Use (全自动浏览器)
让 AI 拥有一个真实的浏览器（需安装 `uv`）。
```json
{
  "mcpServers": {
    "browser-use": {
      "command": "uvx",
      "args": ["browser-use", "--mcp"]
    }
  }
}
```

### 常见问题
*   **权限错误**: 如果安装 Skill 报错 `EPERM`，请手动创建目录: `mkdir -p ~/.agents/skills`
*   **插件加载**: 在 Claude Code 中使用 `/plugin list` 查看已加载的插件和技能。

---

## 6. 一键安装指南 (Quick Start for New Machines)

如果您需要在另一台电脑上复刻这套环境，请按以下步骤操作。

### 第一步：基础环境准备
确保安装了 Node.js (v18+), Git, 和 Python (用于 Browser Use)。
```bash
# 1. 安装 uv (Python 包管理器，用于运行 browser-use)
curl -LsSf https://astral.sh/uv/install.sh | sh

# 2. 安装 GitHub CLI (用于 GitHub 技能)
brew install gh

# 3. 创建全局技能目录 (解决权限问题)
mkdir -p ~/.agents/skills
mkdir -p ~/.claude/rules
```

### 第二步：安装核心组件
```bash
# 1. 安装 everything-claude-code (工作流引擎)
# 注意：首次运行会自动安装到 ~/.claude/plugins
npx everything-claude-code@latest

# 2. 安装 antigravity-awesome-skills (硬技能库)
# 这里使用 sparse-checkout 模式只安装核心技能，避免下载整个仓库
mkdir -p /tmp/install_skills && cd /tmp/install_skills
git init
git remote add origin https://github.com/sickn33/antigravity-awesome-skills.git
git config core.sparseCheckout true
# 添加核心技能列表
echo "skills/architect-review/" >> .git/info/sparse-checkout
echo "skills/testing-patterns/" >> .git/info/sparse-checkout
echo "skills/github-actions-templates/" >> .git/info/sparse-checkout
echo "skills/docker-expert/" >> .git/info/sparse-checkout
# 拉取并安装
git fetch --depth 1 origin main
git checkout main
cp -r skills/* ~/.agents/skills/
# 清理临时文件
cd ~ && rm -rf /tmp/install_skills
```

### 第三步：安装 Anthropic 官方增强包 (Official Plugins)
```bash
# 安装 Feature-Dev, PR-Review, Commit-Commands
mkdir -p /tmp/install_plugins && cd /tmp/install_plugins
git init
git remote add origin https://github.com/anthropics/claude-code.git
git config core.sparseCheckout true
# 添加插件列表
echo "plugins/feature-dev/" >> .git/info/sparse-checkout
echo "plugins/pr-review-toolkit/" >> .git/info/sparse-checkout
echo "plugins/commit-commands/" >> .git/info/sparse-checkout
# 拉取并安装
git fetch --depth 1 origin main
git checkout main
cp -r plugins/* ~/.agents/skills/
# 清理
cd ~ && rm -rf /tmp/install_plugins
```

### 第四步：安装最佳实践 (Skills.sh)
推荐使用 `npx skills add` 逐个安装（更稳定）：
```bash
# 核心开发
npx skills add vercel-labs/agent-skills/vercel-react-best-practices
npx skills add supabase/agent-skills/supabase-postgres-best-practices
npx skills add anthropics/skills/frontend-design

# 移动端
npx skills add expo/skills/building-native-ui

# 办公与协作
npx skills add coreyhaines31/marketingskills/copywriting
npx skills add softaworks/agent-toolkit/mermaid-diagrams
```

### 第五步：配置 MCP
复制以下内容到 `~/.claude/settings.json`：
```json
{
  "mcpServers": {
    "browser-use": {
      "command": "uvx",
      "args": ["browser-use", "--mcp"]
    }
  },
  "enabledPlugins": {
    "everything-claude-code@everything-claude-code": true,
    "context7@claude-plugins-official": true
  }
}
```

---

> **祝您编码愉快！**
> This guide is maintained by your AI Pair Programmer.
