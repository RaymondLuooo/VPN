# 项目知识索引

项目名称：VPN

本文件只用于定位现行有效资料，不复制架构、Decision、Lesson 或历史正文。未来 Agent 应先根据稳定 ID、Scope / 匹配线索和读取触发筛选，再读取直接相关的文档或章节。

## 当前文档

| ID | 类型 | 状态 | Scope / 匹配线索 | 读取触发 | 文档 |
|---|---|---|---|---|---|
| ARCH-ROOT | Architecture | Active | 全项目；`r_equ_*`、`raymond_direct.list`；分流矩阵、策略组、DNS、外部依赖 | 涉及项目目的、Scope、架构、分流规则、策略组职责或公共接口时 | `architecture.md` |
| TEST-ROOT | Testing | Active | 全项目；`r_equ_*`；静态检查、真机测试、客户端兼容性、回归边界 | 涉及规则语法校验、DNS leak 排查、真机验证与测试边界时 | `TESTING.md` |
| HANDOFF-ROOT | Handoff | Active | 全项目；交接记录、已验证边界、断点恢复入口 | 接手未完成任务、恢复中断工作或核对上一 Agent 工作范围时 | `HANDOFF.md` |

## 历史证据入口

`docs/ai-history/INDEX.md` 保存历史演进与事故证据链。当前知识正文可通过 `history_refs` 指向稳定历史 ID；本文件不复制历史条目或正文。

## 检索规则

1. 代码或注释直接引用文档 ID 时，优先读取对应条目。
2. 其次匹配“Scope / 匹配线索”中的路径、模块、符号、字段、术语和主题，再核对“读取触发”。
3. 只读取会影响当前判断的文档或章节；取得足够证据后停止扩大范围。
4. 当前条目存在 `history_refs`，或任务的路径、符号、主题命中历史索引时，按 `AGENTS.md` 选择性读取相关 AI History。
5. 当前资料与相关 AI History 仍缺失或冲突，且缺口会改变结论时，才定向读取原始会话。

## 状态说明

- `Active`：当前有效、可作为现行依据。
- `Superseded`：已被新文档替代，必须同时给出替代文档 ID。
- `Deprecated`：仍可能描述遗留状态，但不应作为新实现依据。
