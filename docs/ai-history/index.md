# AI History Index

最近更新时间：2026-10-01 14:32:00 Asia/Shanghai

## 用途与边界

本目录保存各 AI Agent 的历史会话摘要、任务演进记录、复杂事故排查与关键决策转折。
- 历史正文用于解释“为什么走到今天”以及保留踩坑与失败证据，**不是当前事实或当前规则的权威来源**。
- 日常配置修改与文档查阅无需通读本目录；仅在遇到复杂 Bug、追溯历史方案转折或当前文档无法解释时按需调阅。

## 历史路由索引

| ID | 状态 | 摘要 | Scope / 匹配线索 | 读取触发 | 文档 |
|---|---|---|---|---|---|
| HIST-20260607-01 | Active | 首次初始化项目长期上下文文档，确认 Shadowrocket 与 Mihomo 配置职责和敏感订阅入口边界 | `r_equ_*`, 初始化, 敏感订阅, context-maintenance | 查阅项目上下文初始化决策或初版配置分工时 | `2026-06-07-context-maintenance.md` |
| HIST-20260608-01 | Active | YouTube / Google Photos 分流优化与 Mihomo sniffer 配置进入长期上下文 | YouTube, Google Photos, sniffer, Mihomo | 排查流媒体规则顺序或 sniffer 引入原因时 | `2026-06-08-context-maintenance.md` |
| HIST-20260610-01 | Active | `RaymondDirect`、飞书 / Lark、网易系、Apple / iCloud 和系统 DNS policy 的共享直连规则设计 | `RaymondDirect`, `raymond_direct.list`, 飞书, 网易, Apple, `nameserver-policy` | 修改自维护直连域名或系统 DNS 策略时 | `2026-06-10-context-maintenance.md` |
| HIST-20260610-02 | Active | `apple-cloudkit.com` 直连、配置注释维护、Git 推送边界和客户端验证缺口 | `apple-cloudkit.com`, 注释规范, Git 推送 | 排查 Apple CloudKit 直连或验证缺口时 | `2026-06-10-apple-cloudkit-comments.md` |
| HIST-20260614-01 | Active | `赠送美国` 拆分为美国主选和非美兜底，以及本地运行态文件安全边界 | `赠送美国`, `赠送美国主选`, `赠送非美兜底`, `logs/` | 调整美国节点 fallback 架构或 logs 安全策略时 | `2026-06-14-context-maintenance.md` |
| HIST-20260615-01 | Active | 国外通用兜底从手工 `PROXY` 改为 `赠送美国`，以及 `机场悠兔` provider 更新出口改为 `PROXY` | PROXY, 赠送美国, MATCH, FINAL, 悠兔 | 调整未匹配国外流量兜底策略或订阅更新出口时 | `2026-06-15-context-maintenance.md` |
| HIST-20260628-01 | Active | 主配置拆分为 Mac / Android × 仅美国优先 / 全地区赠送节点 4 份 `r_equ_*` 配置及 `logs/` 运行态边界 | `r_equ_*_mac`, `r_equ_*_android`, 配置四分化 | 查阅多客户端矩阵配置文件拆分决策时 | `2026-06-28-配置拆分为多客户端多节点池.md` |
| HIST-20260720-01 | Active | 配置文件扩展至 8 份矩阵及 rule-providers 代理策略变更为静态住宅节点 | 8 份矩阵, 全渠道, 全静态, rule-providers, 静态住宅 | 查阅全渠道与全静态衍生配置演进时 | `2026-07-20-232200-配置文件扩展及静态住宅代理更新-跨Agent会话归纳.md` |
| HIST-20260721-01 | Active | 8 份配置文件开头统一补充 DNS 配置说明，提交 untracked 项目文件 | DNS 注释, Git 提交, 配置注释 | 查阅 DNS 开头说明注释背景或 Git 历史时 | `2026-07-21-170603-完善DNS配置注释-跨Agent会话归纳.md` |
| HIST-20260731-01 | Active | 修复 Mihomo exclude-filter 空列表 bug、sub.xeton.dev 转换 anytls、url-test 参数优化及全量增加 AI 关键字分流 | `exclude-filter`, `filter: ".*"`, `sub.xeton.dev`, `anytls`, `tolerance`, `DOMAIN-KEYWORD`, AI | 排查订阅转换、Mihomo 内核 filter bug 或 AI 路由时 | `2026-07-31-143000-修复Mihomo语法瑕疵与接入转换器及AI关键字路由-跨Agent会话归纳.md` |

## 状态说明

- `Active`：仍适合作为历史演进与背景证据读取。
- `Superseded`：存在更新的更正记录，必须标注替代文档。
- `Partial`：证据不完整，未确认内容不可作为结论依据。
