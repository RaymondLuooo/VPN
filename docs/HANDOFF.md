# Handoff

最近更新时间：2026-07-31 14:30:00 Asia/Shanghai

## Current project state

当前项目是一个轻量 VPN / 代理配置仓库。仓库根目录当前有 8 份 `r_equ_*` 矩阵配置文件、1 份 Shadowrocket 备份配置和 1 份共享直连规则集，本轮继续维护长期上下文文档。

已确认：

- `r_equ_onlyUS_mac` 是 Shadowrocket / Mac 仅美国优先配置。
- `r_equ_all_countries_mac` 是 Shadowrocket / Mac 全地区赠送节点配置。
- `r_equ_onlyUS_android` 是 Mihomo / Clash Meta for Android 仅美国优先配置。
- `r_equ_all_countries_android` 是 Mihomo / Clash Meta for Android 全地区赠送节点配置。
- `r_equ_all_static_mac` / `android` 为全静态住宅节点配置。
- `r_equ_all_channel_countries_mac` / `android` 为全渠道全地区衍生配置。
- `lazy_group_防DNS泄露去广告后的备份.conf` 是 Shadowrocket 备份配置。
- `raymond_direct.list` 是用户自维护直连规则集，被 Shadowrocket 和 Mihomo 共同引用。
- 最新提交锚点为 `d26e6cd 用关键字加入claude gemini openai 等ai (补充剩余5份Mac及Android配置文件)`。
- 2026-07-31 检查时，`main` 与 `origin/main` 完全同步。
- 2026-07-30 已在 Android 配置中修补了 Mihomo 底层对于单独使用 `exclude-filter` 导致的解构节点列表为空 bug（补充 `filter: ".*"`）。
- 2026-07-30 已在 Android 配置中将“机场悠兔”订阅 URL 升格为 `sub.xeton.dev` 云端转换，解决了 `type: anytls` 私有协议类型被 Clash Meta 核心丢弃的问题。
- 2026-07-30 已将所有 Android 配置文件中的 `url-test` 策略组参数更新为 `tolerance: 50`（防延迟波动频繁切节点）及 `lazy: true`（降低后台耗电）。
- 2026-07-30 ~ 2026-07-31 已将 `DOMAIN-KEYWORD`（`anthropic`、`claude`、`openai`、`gemini`、`chatgpt`）全量部署至 8 份 `r_equ_*` 配置文件中，定向锁死至 `静态住宅` 策略组，并推送至 GitHub 远端仓库。
- 本地 `logs/` 目录存在 Clash 日志和 SQLite 运行态文件；只记录存在性，不读取内容，不直接提交。

待验证：

- 持续关注公益转换服务 `sub.xeton.dev` 的可用性。
- Shadowrocket 与 Mihomo 分流语义是否完全一致。

## What previous agents did

2026-06-07 至 2026-07-21 历次上下文维护由 Codex 及 Antigravity 整理完成，建成了包括 `AGENTS.md`、`README.md`、`docs/` 及 `ai-history/` 在内的完整交接文档链条。

2026-07-31 Antigravity 进行了第八次增量上下文维护：

- 梳理并提交了全量 8 份 `r_equ_*` 配置文件中新增的 AI 域名关键词（`anthropic` / `claude` / `openai` / `gemini` / `chatgpt`）路由。
- 梳理并记录了 Mihomo 底层 `exclude-filter` 导致的空节点列表 bug 修复（补充 `filter: ".*"`）。
- 梳理并记录了“机场悠兔”在 Clash Meta 模式下通过 `sub.xeton.dev` 解决 `anytls` 协议兼容性问题的实测结论。
- 梳理并记录了 `url-test` 策略组中 `tolerance: 50` 及 `lazy: true` 的参数调优。
- 生成了符合规范的凭证文件及 YAML 读取日志，并在 `ai-history/` 追加了新的跨 Agent 会话归纳。

## Files created or modified

长期上下文资料：

- `AGENTS.md`
- `README.md`
- `CHANGELOG.md`
- `docs/PROJECT_CONTEXT.md`
- `docs/TESTING.md`
- `docs/HANDOFF.md`
- `ai-history/INDEX.md`
- `.context-maintenance/receipts/2026-07-31-143000-conversation-discovery.json`
- `.context-maintenance/receipts/2026-07-31-143000-session-reading-log.yaml`
- `ai-history/2026-07-31-143000-修复Mihomo语法瑕疵与接入转换器及AI关键字路由-跨Agent会话归纳.md`

已提交的 VPN 配置文件修改：

- `r_equ_onlyUS_mac`
- `r_equ_all_countries_mac`
- `r_equ_onlyUS_android`
- `r_equ_all_countries_android`
- `r_equ_all_static_mac`
- `r_equ_all_static_android`
- `r_equ_all_channel_countries_mac`
- `r_equ_all_channel_countries_android`

## Current working configurations

已确认：

- 全量 8 份 `r_equ_*` 配置现已托管于 GitHub 仓库根目录，可通过 `raw.githubusercontent.com` 直接拉取更新。
- 配置文件对 AI 相关的流量（Claude, Gemini, OpenAI, Anthropic, ChatGPT）已建立由 `DOMAIN-KEYWORD` 与 `RULE-SET` 组成的双重防线，优先导向 `静态住宅`。
- Android 配置下“机场悠兔”订阅已恢复连通性与测速。
- 针对 Android 端测速频率与抖动进行了 `tolerance: 50` 和 `lazy: true` 优化。

## Generated outputs

本轮完成了全量配置文件的升级与推送，并合成了 Markdown 项目上下文交接文档。

## Known bugs encountered

已修复：

- **Clash Meta exclude-filter 空列表 Bug**：解构 proxy-provider 时如果未写 `filter` 仅写 `exclude-filter`，部分 Mihomo 内核版本会导致筛选后节点数变为 0。在 `r_equ_all_channel_countries_android` 中通过补全 `filter: ".*"` 修复。
- **机场悠兔 anytls 私有协议不兼容 Bug**：悠兔下发 `clash.meta` 格式时使用私有协议 `type: anytls`，直接导致 Clash Meta 核心丢弃全部节点。通过接入 `sub.xeton.dev` 将其在云端转换为标准的 `type: trojan` 节点解决。

## Important warnings for next agent

- 不要把 Android 配置中的订阅 URL 复制到聊天、文档、commit message 或 issue。
- 不要未经确认修改 DNS、TUN、MITM、订阅、规则顺序或策略组名称。
- 不要把 `logs/clash-*.log.txt`、`logs/proxy-*.db*` 或 `.DS_Store` 当成项目配置读取；除非用户明确要求诊断，否则只记录存在性和大小。

## Suggested next steps

- 持续关注用户使用过程中的反流及测速反馈。
- 若 `sub.xeton.dev` 后续不稳定，可协助用户通过自建 Subconverter 的方式进行替代。

## Handoff log

### 2026-07-31 - fix Mihomo exclude-filter bug, subconverter integration, and AI keyword routing

- Agent: Antigravity
- 更新时间：2026-07-31 14:30:00 Asia/Shanghai
- Goal: 记录 Mihomo 语法瑕疵修复、悠兔 anytls 转 trojan、url-test 参数调优及全量 8 份配置增加 AI 关键字路由的变更。
- Files touched: `AGENTS.md`、`README.md`、`CHANGELOG.md`、`docs/PROJECT_CONTEXT.md`、`docs/TESTING.md`、`docs/HANDOFF.md`、8 份 `r_equ_*` 配置文件、`ai-history/INDEX.md`、`.context-maintenance/receipts/2026-07-31-143000-*`、`ai-history/2026-07-31-143000-修复Mihomo语法瑕疵与接入转换器及AI关键字路由-跨Agent会话归纳.md`
- Verified: 所有 8 份 `r_equ_*` 配置均已更新 `DOMAIN-KEYWORD`；`r_equ_*_android` 完成了 exclude-filter 补全及 url-test 优化；悠兔订阅通过 `sub.xeton.dev` 转换且已在安卓端实测成功；Git commit/push 已全部同步到远端 main 分支。
- Not verified: `sub.xeton.dev` 服务的长期网络连通波动。
- Notes: 项目资料维护包含对此前已提交推送的配置变动的完整追溯与交接归纳。
