# Handoff

最近更新时间：2026-07-20 23:22:00 Asia/Shanghai

## Current project state

当前项目是一个轻量 VPN / 代理配置仓库。仓库根目录当前有 4 份 `r_equ_*` 主配置、1 份 Shadowrocket 备份配置和 1 份共享直连规则集，本轮继续维护长期上下文文档。

已确认：

- `r_equ_onlyUS_mac` 是 Shadowrocket / Mac 仅美国优先配置。
- `r_equ_all_countries_mac` 是 Shadowrocket / Mac 全地区赠送节点配置。
- `r_equ_onlyUS_android` 是 Mihomo / Clash Meta for Android 仅美国优先配置。
- `r_equ_all_countries_android` 是 Mihomo / Clash Meta for Android 全地区赠送节点配置。
- 已拆分衍生出了 `r_equ_all_static_mac` / `android`（全静态住宅节点配置）及 `r_equ_all_channel_countries_mac` / `android`。
- `lazy_group_防DNS泄露去广告后的备份.conf` 是 Shadowrocket 备份配置。
- `raymond_direct.list` 是用户自维护直连规则集，被 Shadowrocket 和 Mihomo 共同引用。
- 最新提交锚点为 `da42b24 让国外兜底流量走赠送美国`。
- 2026-06-15 检查时，`main` 与 `origin/main` 同步。
- `2e94fb9` 已将 YouTube App / 媒体域名和 Google Photos 备份 / 媒体传输域名显式前置到 `赠送美国`。
- `45deae4` 已在 `isp_local` 中启用 Mihomo sniffer 相关配置。
- `774a32d` 已将用户自维护直连域名迁入 GitHub-hosted `raymond_direct.list`。
- `6b6436d` 已加入飞书 / Lark 直连域名，并让 `RaymondDirect` 在 Mihomo 中使用系统 DNS。
- `785b164` 已将网易系整体加入直连与系统 DNS 设计。
- `ca06385` 已将 Apple / iCloud 加入自定义直连规则。
- `41ee9d7` 已将 `apple-cloudkit.com` 加入自定义直连规则，并为 `isp_local`、`local_group.conf` 补充 DNS、策略组和规则集注释。
- `5022a34` 已将 `赠送美国` 调整为美国非静态节点主选、非美非静态节点兜底的 fallback 结构。
- `da42b24` 已将 Shadowrocket `Global` / `FINAL` 和 Mihomo `Global` / `MATCH` 统一兜底到 `赠送美国`，并将 Mihomo 中 `机场悠兔` provider 更新出口改为 `PROXY`。
- 2026-06-28 工作树显示旧 `local_group.conf` 和 `isp_local` 已删除，并新增四份 `r_equ_*` 配置；该迁移尚未提交。
- Android 配置包含敏感订阅入口，不得输出真实值。
- 本地 `logs/` 目录存在 Clash 日志和 SQLite 运行态文件；只记录存在性，不读取内容，不直接提交。

待验证：

- 真实客户端导入和连通性。
- DNS leak 和 TUN 行为。
- Mihomo sniffer 对纯 IP 流量的实际识别效果。
- YouTube、Google Photos、Google API 和 AI 工具的真实规则命中优先级。
- Shadowrocket 与 Mihomo 分流语义是否完全一致。
- `RaymondDirect` 在真实客户端中的路由和 DNS policy 命中效果。
- Apple / iCloud / CloudKit 是否需要在 Shadowrocket `[Host]` 中同步系统 DNS。
- `赠送美国` 在美国主选节点不可用时是否能按预期切到非美兜底。
- `Global` / `FINAL` / `MATCH` 兜底到 `赠送美国` 后的真实规则命中。
- `机场悠兔` provider 通过 `PROXY` 更新订阅的客户端表现。
- 四份 `r_equ_*` 配置在对应客户端中的导入、订阅刷新、远程规则刷新和真实规则命中。
- 旧 `local_group.conf` / `isp_local` 删除是否已完成迁移确认。

## What previous agents did

本轮 Codex 做了首次上下文维护：

- 盘点仓库结构。
- 识别配置文件职责。
- 创建项目级 AI agent 规则、README、测试说明、交接文档和 Codex 历史记录。
- 记录敏感订阅入口和验证缺口。

2026-06-08 Codex 做了增量上下文维护：

- 识别最新提交从 `f8b098e` 更新到 `45deae4`。
- 把 YouTube / Google Photos 分流优化和 Mihomo sniffer 写入长期上下文。
- 记录长期上下文文档仍是未跟踪文件。

2026-06-10 Codex 做了第二次增量上下文维护：

- 识别最新提交从 `45deae4` 更新到 `ca06385`。
- 把 `RaymondDirect`、飞书 / Lark、网易系、Apple / iCloud、系统 DNS policy 和推送敏感边界写入长期上下文。
- 记录长期上下文文档仍是未跟踪文件。

2026-06-10 Codex 做了第三次增量上下文维护：

- 识别最新提交从 `ca06385` 更新到 `41ee9d7`。
- 记录 `apple-cloudkit.com` 已进入 `raymond_direct.list` 直连。
- 记录 `isp_local` 和 `local_group.conf` 新增注释只解释 DNS、策略组、规则集和顺序意图，不改变配置逻辑。
- 当时记录本地分支相对 `origin/main` ahead 1，需要用户手动 push；该状态已在 2026-06-14 确认过期。

2026-06-14 Codex 做了第四次增量上下文维护：

- 识别最新提交从 `41ee9d7` 更新到 `5022a34`。
- 记录 `赠送美国` 现在封装美国主选和非美兜底两个子策略组。
- 记录 `main` 已与 `origin/main` 同步，旧的 ahead 1 状态已过期。
- 记录本地 Clash 日志和 SQLite 运行态文件为未跟踪产物，本轮未读取内容。

2026-06-15 Codex 做了第五次增量上下文维护：

- 识别最新提交从 `5022a34` 更新到 `da42b24`。
- 记录 Shadowrocket `Global` / `FINAL` 和 Mihomo `Global` / `MATCH` 已改走 `赠送美国`。
- 记录 Mihomo 中 `机场悠兔` provider 更新出口已改为 `PROXY`。
- 创建 `ai-history/INDEX.md`，补充历史索引入口。

2026-06-28 Codex 做了第六次增量上下文维护：

- 识别工作树从旧 `local_group.conf` / `isp_local` 迁移到四份 `r_equ_*` 配置。
- 记录 Mac / Android × 仅美国优先 / 全地区赠送节点的配置矩阵。
- 记录全地区配置使用 `赠送节点`，仅美国优先配置使用 `赠送美国`。
- 记录运行态文件已移动到 `logs/` 目录，根目录和 `logs/` 仍有 `.DS_Store` 边界。

## Files created or modified

长期上下文资料：

- `AGENTS.md`
- `README.md`
- `CHANGELOG.md`
- `docs/PROJECT_CONTEXT.md`
- `docs/TESTING.md`
- `docs/HANDOFF.md`
- `ai-history/INDEX.md`
- `ai-history/codex/2026-06-07-context-maintenance.md`
- `ai-history/codex/2026-06-08-context-maintenance.md`
- `ai-history/codex/2026-06-10-context-maintenance.md`
- `ai-history/codex/2026-06-10-apple-cloudkit-comments.md`
- `ai-history/codex/2026-06-14-context-maintenance.md`
- `ai-history/codex/2026-06-15-context-maintenance.md`
- `ai-history/codex/2026-06-28-配置拆分为多客户端多节点池.md`
- `ai-history/antigravity/2026-07-20-232200-配置文件扩展及静态住宅代理更新-跨Agent会话归纳.md`

已知历史配置修改：

- `local_group.conf`
- `isp_local`
- `raymond_direct.list`
- `r_equ_onlyUS_mac`
- `r_equ_all_countries_mac`
- `r_equ_onlyUS_android`
- `r_equ_all_countries_android`
- `r_equ_all_static_mac`
- `r_equ_all_static_android`
- `r_equ_all_channel_countries_mac`
- `r_equ_all_channel_countries_android`

本轮上下文维护未修改 VPN 配置文件：

- `r_equ_onlyUS_mac`
- `r_equ_all_countries_mac`
- `r_equ_onlyUS_android`
- `r_equ_all_countries_android`
- `raymond_direct.list`
- `lazy_group_防DNS泄露去广告后的备份.conf`

## Current working configurations

部分确认：

- `r_equ_onlyUS_mac` 和 `r_equ_all_countries_mac` 是当前 Mac / Shadowrocket 配置候选，但是否为用户当前设备正在使用版本需要用户确认。
- `r_equ_onlyUS_android` 和 `r_equ_all_countries_android` 是当前 Android / Mihomo 配置候选，但导入状态需要用户确认。
- Android 配置的 sniffer 配置已静态确认存在，但实际流量识别效果需要客户端日志验证。
- `raymond_direct.list` 是两端共享的直连规则源；在 Mihomo 中同时影响 `dns.nameserver-policy`。
- Shadowrocket `[Host]` 已为飞书 / Lark、网易系等域名维护 `server:system`，但 Apple / iCloud / CloudKit 当前只确认进入 `raymond_direct.list`，未确认是否同步系统 DNS。
- `赠送美国` 当前是高流量服务的总策略组，内部先走美国非静态节点主选，再在主选不可用时走非美非静态兜底。
- Shadowrocket `Global` / `FINAL` 与 Mihomo `Global` / `MATCH` 当前都兜底到 `赠送美国`，不再兜底到手工 `PROXY`。
- Mihomo 中 `机场悠兔` provider 当前通过 `PROXY` 更新订阅，`机场equaldcdn` provider 仍通过 `DIRECT` 更新订阅。
- 全地区配置当前使用 `赠送节点` 作为高流量和国外通用兜底策略组。

## Generated outputs

本轮只生成 Markdown 项目资料，没有生成新 VPN 配置。

## Known bugs encountered

未发现可确认的代码 bug。本仓库当前没有业务代码。

风险：

- 代理订阅入口已存在于 Android 配置，后续对外分享或提交说明必须脱敏。
- 没有自动化检查，手工修改规则时容易出现策略组引用不一致。
- YouTube / Google Photos 规则依赖顺序优先级，后续调整 Google 宽规则时必须复查。
- sniffer 行为依赖 Mihomo 客户端版本和运行环境，静态配置存在不等于实测有效。
- `raymond_direct.list` 现在同时影响直连路由和 Mihomo 系统 DNS；新增域名时不要只按路由需求判断，也要评估 DNS 泄露面。
- `赠送美国` fallback 结构依赖客户端对 fallback / url-test 的实现，仍需在真实客户端验证主选失败后的切换行为。
- `Global` / `FINAL` / `MATCH` 改走 `赠送美国` 后，少数需要手工 `PROXY` 兜底的流量可能需要后续观察。
- provider 更新出口变更可能影响订阅刷新稳定性，仍需客户端实测。
- 旧 `local_group.conf` / `isp_local` 在 Git 中显示删除，四份 `r_equ_*` 显示新增；提交前必须确认迁移方向。
- 未跟踪 Clash 日志、SQLite 运行态文件和 `.DS_Store` 可能含运行痕迹或本地环境信息，提交前需要排除或脱敏。
- Codex 代推 GitHub 曾因 VPN 配置外发风险被安全层拒绝；用户可在本机终端自行推送，agent 不应规避安全层。

## Important warnings for next agent

- 不要把 Android 配置中的订阅 URL 复制到聊天、文档、commit message 或 issue。
- 不要未经确认修改 DNS、TUN、MITM、订阅、规则顺序或策略组名称。
- 不要把 `raymond_direct.list` 当作普通直连列表盲目扩展；它在 Mihomo 中也控制系统 DNS。
- 不要把 `PROXY` 误写成当前默认国外兜底；当前国外通用兜底是 `赠送美国`。
- 不要把 `logs/clash-*.log.txt`、`logs/proxy-*.db*` 或 `.DS_Store` 当成项目配置读取；除非用户明确要求诊断，否则只记录存在性和大小。
- 不要访问个人 Chrome profile、浏览器 cookie、Keychain、SSH 私钥或 `/Users/raymond/`。
- 如果用户只问分析或方案，不要直接改配置。

## Suggested next steps

- 用户确认后，可创建脱敏示例配置。
- 用户确认后，可写一个只读静态检查脚本，检查规则引用和敏感字段。
- 用户完成客户端导入后，把验证结果追加到本文件的 Handoff log。
- 用户决定是否将当前未跟踪的上下文文档纳入 Git 提交。
- 用户确认后，可新增 `.gitignore` 规则排除 Clash 日志和 SQLite 运行态文件。
- 真机验证 `Global` / `FINAL` / `MATCH` 兜底到 `赠送美国` 后的规则命中。
- 真机验证 `机场悠兔` provider 通过 `PROXY` 刷新订阅的稳定性。
- 真机验证四份 `r_equ_*` 配置的导入、订阅刷新、远程规则刷新和规则命中。
- 提交前确认 `.DS_Store`、`logs/` 运行态文件不会进入 Git。
- 提交前确认旧 `local_group.conf` / `isp_local` 删除和四份新配置新增是预期迁移。
- 如果继续扩展 `raymond_direct.list`，优先使用明确域名后缀，避免 `DOMAIN-KEYWORD` 误匹配。
- 如需公开发布或模板化仓库，先拆分或脱敏 Android 配置中的订阅入口。

## Handoff log

### 2026-06-07 - initial context maintenance

- Agent: Codex
- Goal: 初始化项目长期上下文资料。
- Files touched: `AGENTS.md`、`README.md`、`CHANGELOG.md`、`docs/PROJECT_CONTEXT.md`、`docs/TESTING.md`、`docs/HANDOFF.md`、`ai-history/codex/2026-06-07-context-maintenance.md`
- Verified: 仓库结构、Git 状态、配置文件类型、关键节标题、敏感订阅入口存在性。
- Not verified: 客户端导入、节点连通性、远程规则刷新、DNS leak、TUN 行为。
- Notes: 未修改任何 VPN 配置文件。

### 2026-06-08 - incremental context maintenance

- Agent: Codex
- Goal: 根据最新提交维护项目长期上下文资料。
- Files touched: `README.md`、`CHANGELOG.md`、`docs/PROJECT_CONTEXT.md`、`docs/TESTING.md`、`docs/HANDOFF.md`、`ai-history/codex/2026-06-08-context-maintenance.md`
- Verified: Git 最新提交、配置文件结构、sniffer 顶层区块、YouTube / Google Photos 前置规则存在性。
- Not verified: 客户端导入、节点连通性、远程规则刷新、DNS leak、TUN 行为、sniffer 实际效果、规则命中日志。
- Notes: 未修改任何 VPN 配置文件；上下文文档仍未被 Git 跟踪提交。

### 2026-06-10 - incremental context maintenance

- Agent: Codex
- Goal: 根据 `RaymondDirect`、飞书 / Lark、网易系、Apple / iCloud 和系统 DNS 变更维护项目长期上下文。
- Files touched: `AGENTS.md`、`README.md`、`CHANGELOG.md`、`docs/PROJECT_CONTEXT.md`、`docs/TESTING.md`、`docs/HANDOFF.md`、`ai-history/codex/2026-06-10-context-maintenance.md`
- Verified: Git 最新提交、配置文件行数、`RaymondDirect` provider 引用、`dns.nameserver-policy` 引用、Shadowrocket `[Host]` 中的系统 DNS 条目。
- Not verified: 客户端导入、节点连通性、远程规则刷新、DNS leak、真实 DNS policy 命中日志。
- Notes: 未修改 VPN 配置逻辑；上下文文档仍未被 Git 跟踪提交。

### 2026-06-10 - apple cloudkit direct and comments

- Agent: Codex
- Goal: 记录 Apple CloudKit API 直连和两份配置注释维护。
- Files touched: `raymond_direct.list`、`isp_local`、`local_group.conf`、`README.md`、`CHANGELOG.md`、`docs/PROJECT_CONTEXT.md`、`docs/TESTING.md`、`docs/HANDOFF.md`、`ai-history/codex/2026-06-10-apple-cloudkit-comments.md`
- Verified: 本地提交 `41ee9d7` 已创建；`raymond_direct.list` 包含 `DOMAIN-SUFFIX,apple-cloudkit.com`；`isp_local` YAML 解析通过；`isp_local` 和 `local_group.conf` 的本轮配置文件变更除注释外无非注释变更。
- Not verified: 客户端导入、节点连通性、远程规则刷新、DNS leak、Apple CloudKit 实际规则命中和 DNS policy 日志。
- Notes: 当时未代推 GitHub，`main` 相对 `origin/main` ahead 1；该 Git 同步状态已在 2026-06-14 确认过期。上下文文档仍未被 Git 跟踪提交。

### 2026-06-14 - gifted us fallback context update

- Agent: Codex
- Goal: 根据 `5022a34` 更新项目长期上下文资料。
- Files touched: `AGENTS.md`、`README.md`、`CHANGELOG.md`、`docs/PROJECT_CONTEXT.md`、`docs/TESTING.md`、`docs/HANDOFF.md`、`ai-history/codex/2026-06-14-context-maintenance.md`
- Verified: Git 最新提交、`main` 与 `origin/main` 同步状态、配置文件行数、`赠送美国主选` / `赠送非美兜底` / `赠送美国` 结构、未跟踪运行态文件存在性。
- Not verified: 客户端导入、节点连通性、远程规则刷新、DNS leak、`赠送美国` fallback 实际切换行为。
- Notes: 未修改 VPN 配置文件；未读取 Clash 日志和 SQLite 运行态数据库内容。

### 2026-06-15 - foreign fallback to gifted us context update

- Agent: Codex
- 更新时间：2026-06-15 15:10:14 Asia/Shanghai
- Goal: 根据 `da42b24` 更新项目长期上下文资料。
- Files touched: `AGENTS.md`、`README.md`、`CHANGELOG.md`、`docs/PROJECT_CONTEXT.md`、`docs/TESTING.md`、`docs/HANDOFF.md`、`ai-history/INDEX.md`、`ai-history/codex/2026-06-15-context-maintenance.md`
- Verified: Git 最新提交、`main` 与 `origin/main` 同步状态、`Global` / `FINAL` / `MATCH` 兜底目标、`机场悠兔` provider 更新出口、未跟踪运行态文件存在性。
- Not verified: 客户端导入、节点连通性、远程规则刷新、DNS leak、国外通用兜底真实规则命中、provider 刷新实测。
- Notes: 未修改 VPN 配置文件；未读取 Clash 日志和 SQLite 运行态数据库内容；未自动 commit/push。

### 2026-06-28 - config matrix split context update

- Agent: Codex
- 更新时间：2026-06-28 01:45:54 Asia/Shanghai
- Goal: 根据当前工作树配置拆分维护项目长期上下文资料。
- Files touched: `AGENTS.md`、`README.md`、`CHANGELOG.md`、`docs/PROJECT_CONTEXT.md`、`docs/TESTING.md`、`docs/HANDOFF.md`、`ai-history/INDEX.md`、`ai-history/codex/2026-06-28-配置拆分为多客户端多节点池.md`
- Verified: Git 状态、四份 `r_equ_*` 配置文件存在性和行数、Mac / Android 顶层结构、`FINAL` / `MATCH` 兜底目标、`RaymondDirect` 引用、运行态文件存在性。
- Not verified: 客户端导入、节点连通性、远程规则刷新、DNS leak、四份配置真实规则命中、provider 刷新实测。
- Notes: 未修改 VPN 配置文件；未读取 `logs/` 下 Clash 日志和 SQLite 运行态数据库内容；未自动 commit/push。

### 2026-07-20 - configuration matrix expansion and static proxy update

- Agent: antigravity
- 更新时间：2026-07-20 23:22:00 Asia/Shanghai
- Goal: 根据最新未提交的多份配置文件变化和直连域名变化维护项目长期上下文资料。
- Files touched: `AGENTS.md`、`README.md`、`CHANGELOG.md`、`docs/PROJECT_CONTEXT.md`、`docs/TESTING.md`、`docs/HANDOFF.md`、`ai-history/INDEX.md`、`ai-history/antigravity/2026-07-20-232200-配置文件扩展及静态住宅代理更新-跨Agent会话归纳.md`
- Verified: 配置拆分及新增的 `r_equ_all_static`、`r_equ_all_channel_countries` 文件，Mac 配置文件中的正则与注释，rule-providers 下载规则变更为静态住宅节点，`raymond_direct.list` 及其关联配置放宽 `bytedance.com` 和 `zijieapi.com` 直连。
- Not verified: 客户端真实导入情况、节点连通性、通过静态住宅下载外部规则集的稳定性，以及新节点正则匹配效果。
- Notes: 发现多份重要配置仍在本地属于 untracked 或 modified 状态未被提交，需要在后续操作中提醒用户确认及提交。未运行自动化 commit / push。
