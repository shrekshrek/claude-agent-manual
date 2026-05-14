# `<NNN>` `<slug>` — Spec

> 创建于 YYYY-MM-DD · 状态:**草稿** / 评审中 / 已确认 / 已实现 / 已上线
>
> **本文件回答 WHAT —— 做什么、为什么。评审通过后冻结,需变更则起新功能 spec。**
> 详细写法见 [project-workflow / spec-driven.md](https://github.com/shrekshrek/project-workflow/blob/main/docs/spec-driven.md)。

---

## 1. Outcomes

> 场景化散文 **或 API 行为描述**,不要写 user story 句式("As a X I want Y...")。

(填:谁,在什么场景下,能做什么)

## 2. Scope boundaries

**做**:
-

**不做**(显式列出避免 scope creep):
-

## 3. Constraints

> 性能/安全/兼容性/法规等**硬数字**约束。

-

## 4. Verification

> 上线前怎么验证。具体、可执行,不要写"覆盖率 80%"这种空话。

- 单测:(测什么场景)
- 集成:(测什么端到端流程)
- 手测:(运行什么命令验证什么)
- 上线指标:(如适用)

---

## 修订记录

> 默认 spec **冻结**。实施中真发现 spec 错时(走 [workflow.md §3.5 修订 SOP](https://github.com/shrekshrek/project-workflow/blob/main/docs/workflow.md#35-开发中发现-specplan-错怎么办)),在此追加一行,**每条引用对应 ADR**。

<!-- 例:
- 2026-05-15: 改 §2 Scope 加"不发短信验证";原因见 ADR-0003
- 2026-05-20: 改 §4 Verification 把"用户验证"具体化为可断言 test;ADR-0005
-->

- (实施中按需追加)
