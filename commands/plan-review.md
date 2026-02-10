---
description: "方案质量审查（只读）：需求覆盖、架构偏移、接口一致性、依赖完整性、方案可行性"
argument-hint: "审查范围：'project'、模块名、或留空自动检测"
---

# Plan Review

You are performing a read-only quality review of the project's planning artifacts and their alignment with actual code. This command NEVER modifies any files — it produces a structured review report directly in the conversation.

## Core Principles

- **Read-only**: Do not create or modify any files. Output the report directly in the conversation.
- **Evidence-based**: Every finding must reference a specific file, section, or line.
- **Actionable**: Every finding must include a suggested remediation path (which command to run).
- **Proportional**: Scale agent usage to the amount of implemented code. No agents if no code exists yet.

---

## Prerequisites

This command requires at minimum:
- `CLAUDE.md` — project overview, module list
- `PROGRESS.md` — module status table
- `docs/architecture.md` — system architecture, module boundaries

If these files don't exist, tell the user to run `/project-plan` first.

---

## Entry: Scope Detection

### If $ARGUMENTS is "project" or "全局" → Project-Level Review

Run all 5 review dimensions across the entire project.

### If $ARGUMENTS is a module name → Module-Level Review

Run review dimensions scoped to that module and its direct neighbors (upstream dependencies + downstream dependents).

### If $ARGUMENTS is empty → Auto-Detect

1. Read `PROGRESS.md`
2. If a module is in "方案已确认" status → suggest module-level review for it
3. If multiple modules are "已完成" and none is "方案已确认" → suggest project-level review
4. If all modules are "未开始" → suggest project-level review of initial architecture
5. Present suggestion and **wait for user to confirm scope**

---

## Phase 1: Document Analysis

**Goal**: Review plan quality using only the planning documents (no code scanning)

### Dimension 1: Requirements Coverage

**Actions**:
1. Extract feature/requirement list from `CLAUDE.md` "功能范围" section
2. Extract module list and responsibilities from `CLAUDE.md`
3. Read `docs/architecture.md` module boundaries
4. Cross-reference: for each declared requirement, verify at least one module is responsible for it
5. For module scope: cross-reference module's responsibilities (from `docs/architecture.md`) against `docs/plan.md` implementation steps

Classify each requirement:
- ✓ 已覆盖 — at least one module's responsibility clearly maps to this requirement
- ✗ 未覆盖 — no module claims responsibility
- ⚠ 部分覆盖 — module exists but scope description doesn't fully address the requirement

### Dimension 2: Dependency Integrity

**Actions**:
1. Extract the module dependency graph from `docs/architecture.md` (the "Dependencies" field under each module boundary)
2. Check for circular dependencies — attempt topological sort; if it fails, report the cycle
3. Read `PROGRESS.md` for module statuses
4. For each module about to be developed ("方案已确认"): verify all upstream dependencies are "已完成" or at least "方案已确认"
5. For module scope: focus on the target module's upstream and downstream neighbors

Classify findings:
- ✗ 循环依赖 — module A depends on B and B depends on A (directly or transitively)
- ⚠ 上游未就绪 — module depends on an upstream that is still "未开始"
- ⚠ 顺序风险 — module planned before its dependency is complete

**Gate**: Present Phase 1 findings. Wait for user acknowledgment before proceeding.

---

## Phase 2: Code-Aware Analysis

**Goal**: Compare planning artifacts against actual implemented code

**Skip this phase if**: No modules have status "已完成" (no code to scan). Tell the user: "目前没有已实现的模块，跳过代码对照分析。"

### Dimension 3: Architecture Drift

**Actions**:
1. Launch 1-2 **codebase-explorer** agents (scale by number of implemented modules):
   - Agent 1: "Scan the project's actual directory structure and compare against the module list and boundaries declared in `docs/architecture.md`. Report: (a) directories/modules in code but not in architecture docs, (b) modules in architecture docs but not found in code, (c) data models in code that differ from the ER diagram or schema descriptions."
   - Agent 2 (if 3+ modules implemented): "Trace the actual import/call dependencies between implemented modules. Compare against the dependency graph declared in `docs/architecture.md`. Report any undeclared dependencies or missing declared dependencies."
2. Synthesize agent findings

Classify each drift:
- 代码中新增 — exists in code, not documented in architecture
- 文档中存在 — documented in architecture, not found in code
- 形态变更 — both exist but shape differs (e.g., different fields, different interface)

### Dimension 4: Cross-Module Interface Consistency

**Actions**:
1. For each pair of (provider module, consumer module) where both have implementation:
   - Launch **codebase-explorer** agent: "Find the public interface exported by [provider] (exported functions, API routes, type definitions) and find where [consumer] actually calls it. Report any mismatches in: function signatures, parameter types, response shapes, error handling contracts."
2. Cross-reference against `docs/architecture.md` interface definitions
3. Also check against module-level CLAUDE.md files (`src/[module]/CLAUDE.md`) if they exist
4. For module scope: only check the target module's direct neighbors

Classify:
- ✓ 一致 — documented, exported, and consumed interfaces all match
- ⚠ 不一致 — mismatch between any two of (documented / exported / consumed)
- ✗ 缺失 — interface documented but not exported, or consumed but not exported

**Gate**: Present Phase 2 findings. Wait for user acknowledgment.

---

## Phase 3: Plan Feasibility

**Goal**: Assess whether the current module plan (`docs/plan.md`) is ready for implementation

**Skip this phase if**: `docs/plan.md` does not exist. Tell the user: "当前无 `docs/plan.md`，跳过方案可行性分析。如需审查模块方案，请先运行 `/module-plan`。"

### Dimension 5: Plan Feasibility

**Actions**:

1. Read `docs/plan.md`

2. Internal consistency checks:
   - Do implementation steps reference data models defined in the plan's data model section?
   - Do implementation steps reference APIs/interfaces defined in the plan's interface design section?
   - Is step ordering valid? (no step depends on a later step's output)
   - Does the test strategy cover the edge cases listed in the plan's edge case section?

3. External consistency checks:
   - Do consumed interfaces (from upstream modules) match what those modules actually provide?
   - Check against module-level CLAUDE.md files and actual code if available

4. Scope assessment:
   - Count implementation steps. If > 15, suggest the module may benefit from splitting.
   - Flag any step whose description is vague or lacks specificity (e.g., "implement the logic" without specifying what logic)

Classify:
- ✓ 可行 — no issues found
- ⚠ 关注 — concern that could cause problems (with specifics)
- ✗ 阻塞 — issue that will likely block implementation

---

## Summary Report

After all applicable phases, compile the complete report.

### Report Structure

```markdown
# 方案审查报告

## 审查范围
- 级别: 项目级 / 模块级 ([模块名])
- 模块状态概览: N 个已完成 / M 个方案已确认 / K 个未开始

## 1. 需求覆盖
| 需求/功能 | 对应模块 | 状态 |
|:---|:---|:---|
| 功能 A | auth | ✓ 已覆盖 |
| 功能 B | — | ✗ 未覆盖 |

## 2. 依赖完整性
[Mermaid dependency graph with status indicators]
- [Findings list]

## 3. 架构偏移 (仅当 Phase 2 执行时)
| 偏移项 | 类型 | 详情 |
|:---|:---|:---|
| ... | 代码中新增 / 文档中存在 / 形态变更 | ... |

## 4. 接口一致性 (仅当 Phase 2 执行时)
| 提供方 | 消费方 | 状态 | 详情 |
|:---|:---|:---|:---|
| ... | ... | ✓/⚠/✗ | ... |

## 5. 方案可行性 (仅当 Phase 3 执行时)
- [Findings list]

## 总评
- **CRITICAL**: [N] 项 — 阻塞性问题，建议立即修复
- **WARNING**: [N] 项 — 潜在风险，建议关注
- **INFO**: [N] 项 — 优化建议

## 建议的下一步
- [Actionable next steps with specific commands]
```

### Severity Classification

| 级别 | 标准 | 示例 |
|:---|:---|:---|
| **CRITICAL** | 阻塞开发或导致运行时错误 | 循环依赖、核心需求无模块承接、已实现模块间接口不匹配 |
| **WARNING** | 有未来风险但不立即阻塞 | 架构偏移、上游模块未完成、方案引用不存在的接口 |
| **INFO** | 改进建议 | 模块步骤过多建议拆分、测试策略未覆盖某边界、文档可更清晰 |

### Next-Step Mapping

Remediation comes in two forms: **direct fix** (ask Claude to modify the specific file) and **command-driven** (re-enter a formal workflow). Choose based on scope of change.

| 发现类型 | 改动对象 | 小范围调整 | 大范围重构 |
|:---|:---|:---|:---|
| 需求未覆盖 | `CLAUDE.md`, `docs/architecture.md` | 直接让 Claude 补充模块职责或需求映射 | `/project-plan` revision mode |
| 循环依赖 / 依赖顺序 | `docs/architecture.md` | 直接让 Claude 调整模块边界和依赖关系 | `/project-plan` revision mode |
| 架构偏移（文档落后于代码） | `docs/architecture.md` | 直接让 Claude 将实际架构同步到文档 | — |
| 架构偏移（代码偏离设计） | 实现代码 | 直接修复代码 | `/module-dev` refinement mode |
| 接口不一致（文档与代码） | 模块 CLAUDE.md 或代码 | 直接让 Claude 对齐接口定义 | — |
| 接口不一致（模块间代码） | 实现代码 | 直接修复调用方或提供方 | `/module-dev` refinement mode |
| 方案内部不一致 | `docs/plan.md` | 直接让 Claude 修正 plan 中的矛盾 | `/module-plan` revision mode |
| 方案步骤过多 | `docs/plan.md`, `docs/architecture.md` | — | `/module-plan` 拆分模块 |

After fixing, run `/plan-review` again to confirm resolution.

---

## Important Notes

- **This command is READ-ONLY.** It does not create, modify, or delete any files. The report is output directly in the conversation.
- **Scale agent usage proportionally.** 0 agents if no code exists, 1-2 agents for few modules, 2-3 agents for many modules. Never launch agents unnecessarily.
- **Do not block the user.** If certain dimensions cannot be checked (no code for drift, no plan for feasibility), skip them gracefully and explain why.
- **Suggest re-running after fixes.** After the user addresses findings using other commands, suggest running `/plan-review` again to confirm resolution.
- **This is an assessment, not a verdict.** Findings are potential issues for the user to evaluate. The user decides which to act on.
