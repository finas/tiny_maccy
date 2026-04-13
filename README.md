# yaccm Specification

> A lightweight, keyboard-first clipboard manager for macOS.

---

## Credits

This project was inspired by **[Maccy](https://maccy.app/)**, a wonderful open-source clipboard manager for macOS. The core idea of a fast, global-hotkey-driven clipboard history tool comes from Maccy. yaccm is an independent, minimal implementation built for learning and lightweight daily use.

---

## Document Index

| # | Document | Purpose |
|---|----------|---------|
| 1 | [Overview](./01-overview.md) | Product vision, target users, goals, and non-goals |
| 2 | [Functional Specification](./02-functional-spec.md) | Detailed functional requirements by feature area |
| 3 | [Non-Functional Specification](./03-non-functional-spec.md) | Performance, reliability, security, and portability requirements |
| 4 | [UI/UX Specification](./04-ui-ux-spec.md) | Interface behavior, visual design, and interaction patterns |
| 5 | [Architecture Specification](./05-architecture-spec.md) | System components, data flow, and boundaries |
| 6 | [Test Plan](./06-test-plan.md) | Acceptance criteria, testing strategy, and verification matrix |
| 7 | [Roadmap](./07-roadmap.md) | Phased delivery plan and future enhancements |
| 8 | [Glossary](./glossary.md) | Terms, abbreviations, and definitions |

---

## Specification Status

**Version:** 1.0
**Status:** Approved (MVP Baseline)
**Last Updated:** 2026-04-13

---

## How to Use This Specification

1. **Product Managers** — start with [01-overview.md](./01-overview.md) and [02-functional-spec.md](./02-functional-spec.md).
2. **Engineers** — read [05-architecture-spec.md](./05-architecture-spec.md) alongside [02-functional-spec.md](./02-functional-spec.md).
3. **QA** — use [06-test-plan.md](./06-test-plan.md) as the primary source of truth for verification.
4. **Designers** — reference [04-ui-ux-spec.md](./04-ui-ux-spec.md) for all interaction and visual requirements.
5. **AI Agents** — read [skill.md](./skill.md) before making any code changes.

---

## Change Log

| Date | Version | Changes |
|------|---------|---------|
| 2026-04-13 | 1.0 | Initial specification set derived from PRD |

---

## 中文说明

> 轻量级、键盘优先的 macOS 剪贴板管理器。

### 致谢

本项目受到 **[Maccy](https://maccy.app/)** 的启发。Maccy 是一款优秀的 macOS 开源剪贴板管理工具，yaccm 的核心概念——通过全局快捷键快速调取剪贴板历史——正是源自 Maccy。yaccm 是一个独立的极简实现，主要用于学习和日常轻度使用。

### 文档索引

| 编号 | 文档 | 说明 |
|------|------|------|
| 1 | [Overview](./01-overview.md) | 产品愿景、目标用户、目标与非目标 |
| 2 | [Functional Specification](./02-functional-spec.md) | 按功能模块划分的详细功能需求 |
| 3 | [Non-Functional Specification](./03-non-functional-spec.md) | 性能、可靠性、安全性和可移植性需求 |
| 4 | [UI/UX Specification](./04-ui-ux-spec.md) | 界面行为、视觉设计和交互模式 |
| 5 | [Architecture Specification](./05-architecture-spec.md) | 系统组件、数据流和边界划分 |
| 6 | [Test Plan](./06-test-plan.md) | 验收标准、测试策略和验证矩阵 |
| 7 | [Roadmap](./07-roadmap.md) | 分阶段交付计划和未来增强功能 |
| 8 | [Glossary](./glossary.md) | 术语、缩写和定义 |

### 规范状态

**版本：** 1.0
**状态：** 已批准（MVP 基线）
**最后更新：** 2026-04-13

### 使用方式

1. **产品经理** — 从 [01-overview.md](./01-overview.md) 和 [02-functional-spec.md](./02-functional-spec.md) 开始阅读。
2. **工程师** — 结合 [05-architecture-spec.md](./05-architecture-spec.md) 与 [02-functional-spec.md](./02-functional-spec.md) 进行实现。
3. **测试/QA** — 以 [06-test-plan.md](./06-test-plan.md) 为验收依据。
4. **设计师** — 参考 [04-ui-ux-spec.md](./04-ui-ux-spec.md) 了解所有交互和视觉需求。
5. **AI 智能体** — 在进行任何代码修改前，请先阅读 [skill.md](./skill.md)。
# tiny_maccy
