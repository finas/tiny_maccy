# yaccm 规范文档

> 轻量级、键盘优先的 macOS 剪贴板管理器。

[English Version](./README.md)

---

## 致谢

本项目受到 **[Maccy](https://maccy.app/)** 的启发。Maccy 是一款优秀的 macOS 开源剪贴板管理工具，yaccm 的核心概念——通过全局快捷键快速调取剪贴板历史——正是源自 Maccy。yaccm 是一个独立的极简实现，主要用于学习和日常轻度使用。

---

## 文档索引

| 编号 | 文档 | 说明 |
|------|------|------|
| 1 | [Overview](./spec/01-overview.md) | 产品愿景、目标用户、目标与非目标 |
| 2 | [Functional Specification](./spec/02-functional-spec.md) | 按功能模块划分的详细功能需求 |
| 3 | [Non-Functional Specification](./spec/03-non-functional-spec.md) | 性能、可靠性、安全性和可移植性需求 |
| 4 | [UI/UX Specification](./spec/04-ui-ux-spec.md) | 界面行为、视觉设计和交互模式 |
| 5 | [Architecture Specification](./spec/05-architecture-spec.md) | 系统组件、数据流和边界划分 |
| 6 | [Test Plan](./spec/06-test-plan.md) | 验收标准、测试策略和验证矩阵 |
| 7 | [Roadmap](./spec/07-roadmap.md) | 分阶段交付计划和未来增强功能 |
| 8 | [Glossary](./spec/glossary.md) | 术语、缩写和定义 |

---

## 规范状态

**版本：** 1.0
**状态：** 已批准（MVP 基线）
**最后更新：** 2026-04-13

---

## 使用方式

1. **产品经理** — 从 [01-overview.md](./spec/01-overview.md) 和 [02-functional-spec.md](./spec/02-functional-spec.md) 开始阅读。
2. **工程师** — 结合 [05-architecture-spec.md](./spec/05-architecture-spec.md) 与 [02-functional-spec.md](./spec/02-functional-spec.md) 进行实现。
3. **测试/QA** — 以 [06-test-plan.md](./spec/06-test-plan.md) 为验收依据。
4. **设计师** — 参考 [04-ui-ux-spec.md](./spec/04-ui-ux-spec.md) 了解所有交互和视觉需求。
5. **AI 智能体** — 在进行任何代码修改前，请先阅读 [skill.md](./skill.md)。

---

## 变更日志

| 日期 | 版本 | 变更内容 |
|------|------|----------|
| 2026-04-13 | 1.0 | 从 PRD 生成的初始规范文档集 |
