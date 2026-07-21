# AI History Index

最近更新时间：2026-06-28 01:45:54 Asia/Shanghai

## 用途

本目录保存各 AI agent 的历史会话摘要、任务历史摘要、上下文维护记录、关键决策和未验证边界。不要把完整原始聊天直接塞进项目入口文档。

## 阅读规则

- 只做普通配置检查或文档入口阅读时，不必先读完整 `ai-history/`。
- 做项目资料维护、交接、复盘、风险判断、规则迁移、历史决策确认时，先读本索引，再按主题打开相关文件。
- 判断增量维护范围时，优先看 `CHANGELOG.md` 和 `docs/HANDOFF.md` 的最近更新时间，不用 git commit 作为项目资料维护锚点。
- 不读取 Clash 日志、SQLite 运行态数据库、订阅 URL、token、cookie、密码或节点凭据。

## 目录

- `codex/`：Codex 相关任务历史。

## 主题索引

- `codex/2026-06-07-context-maintenance.md`：首次初始化项目长期上下文文档，确认 Shadowrocket 与 Mihomo 配置职责和敏感订阅入口边界。
- `codex/2026-06-08-context-maintenance.md`：记录 YouTube / Google Photos 分流优化和 Mihomo sniffer 配置进入长期上下文。
- `codex/2026-06-10-context-maintenance.md`：记录 `RaymondDirect`、飞书 / Lark、网易系、Apple / iCloud 和系统 DNS policy 的共享直连规则设计。
- `codex/2026-06-10-apple-cloudkit-comments.md`：记录 `apple-cloudkit.com` 直连、配置注释维护、Git 推送边界和客户端验证缺口。
- `codex/2026-06-14-context-maintenance.md`：记录 `赠送美国` 拆分为美国主选和非美兜底，以及本地运行态文件的安全边界。
- `codex/2026-06-15-context-maintenance.md`：记录国外通用兜底从手工 `PROXY` 改为 `赠送美国`，以及 `机场悠兔` provider 更新出口改为 `PROXY`。
- `codex/2026-06-28-配置拆分为多客户端多节点池.md`：记录旧主配置拆分为 Mac / Android × 仅美国优先 / 全地区赠送节点四份 `r_equ_*` 配置，以及 `logs/` 运行态目录边界。
- `antigravity/2026-07-20-232200-配置文件扩展及静态住宅代理更新-跨Agent会话归纳.md`：记录配置文件的四分化演进及 rule-providers 代理策略变更为静态住宅节点，同时更新直连规则和 Mac 配置项下的策略组说明。
