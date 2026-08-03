# AI History Index

最近更新时间：2026-07-21 17:06:03 Asia/Shanghai

## 用途

本目录保存各 AI agent 的历史会话摘要、任务历史摘要、上下文维护记录、关键决策和未验证边界。不要把完整原始聊天直接塞进项目入口文档。

## 阅读规则

- 只做普通配置检查或文档入口阅读时，不必先读完整 `ai-history/`。
- 做项目资料维护、交接、复盘、风险判断、规则迁移、历史决策确认时，先读本索引，再按主题打开相关文件。
- 判断增量维护范围时，优先看 `CHANGELOG.md` 和 `docs/HANDOFF.md` 的最近更新时间，不用 git commit 作为项目资料维护锚点。
- 不读取 Clash 日志、SQLite 运行态数据库、订阅 URL、token、cookie、密码或节点凭据。

## 主题索引

- `2026-06-07-context-maintenance.md`：首次初始化项目长期上下文文档，确认 Shadowrocket 与 Mihomo 配置职责和敏感订阅入口边界。
- `2026-06-08-context-maintenance.md`：记录 YouTube / Google Photos 分流优化和 Mihomo sniffer 配置进入长期上下文。
- `2026-06-10-context-maintenance.md`：记录 `RaymondDirect`、飞书 / Lark、网易系、Apple / iCloud 和系统 DNS policy 的共享直连规则设计。
- `2026-06-10-apple-cloudkit-comments.md`：记录 `apple-cloudkit.com` 直连、配置注释维护、Git 推送边界和客户端验证缺口。
- `2026-06-14-context-maintenance.md`：记录 `赠送美国` 拆分为美国主选和非美兜底，以及本地运行态文件的安全边界。
- `2026-06-15-context-maintenance.md`：记录国外通用兜底从手工 `PROXY` 改为 `赠送美国`，以及 `机场悠兔` provider 更新出口改为 `PROXY`。
- `2026-06-28-配置拆分为多客户端多节点池.md`：记录旧主配置拆分为 Mac / Android × 仅美国优先 / 全地区赠送节点四份 `r_equ_*` 配置，以及 `logs/` 运行态目录边界。
- `2026-07-20-232200-配置文件扩展及静态住宅代理更新-跨Agent会话归纳.md`：记录配置文件的四分化演进及 rule-providers 代理策略变更为静态住宅节点，同时更新直连规则和 Mac 配置项下的策略组说明。
- `2026-07-21-170603-完善DNS配置注释-跨Agent会话归纳.md`：记录在 8 份配置文件的开头统一补充了 `# DNS 配置说明：`，并把之前大量 untracked 的项目文件和文档提交到 Git。
- `2026-07-31-143000-修复Mihomo语法瑕疵与接入转换器及AI关键字路由-跨Agent会话归纳.md`：记录 Mihomo exclude-filter 空节点列表 bug 修复、sub.xeton.dev 转换悠兔 anytls 协议、url-test 参数优化及全量 8 份配置添加 AI 关键字分流规则。
