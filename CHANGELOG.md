# Changelog

最近更新时间：2026-07-31 14:30:00 Asia/Shanghai

## [0.1.9] - 2026-07-31

更新时间：2026-07-31 14:30:00 Asia/Shanghai
维护 ID：2026-07-31-143000
Git 锚点：d26e6cd
锚点工作树状态：dirty
已覆盖未提交文件：AGENTS.md, CHANGELOG.md, README.md, ai-history/INDEX.md, docs/HANDOFF.md, docs/PROJECT_CONTEXT.md, docs/TESTING.md
增量可信度：精确

### 上下文文档

- 增量更新项目资料，资料维护锚点推进到 2026-07-31 14:30:00。
- 记录 Mihomo 语法瑕疵修复（`exclude-filter` 补全 `filter: ".*"`）。
- 记录接入 `sub.xeton.dev` 云端订阅转换成功解决“机场悠兔” `anytls` 私有协议在 Clash Meta 报错解析为 0 节点的 bug。
- 记录 Android 配置文件 `url-test` 参数调优（`tolerance: 50`, `lazy: true`）。
- 记录全量 8 份 `r_equ_*` 配置文件新增 AI 服务域名关键字路由规则（`anthropic` / `claude` / `openai` / `gemini` / `chatgpt`）并已 commit/push 至 GitHub。
- 追加 `ai-history/2026-07-31-143000-修复Mihomo语法瑕疵与接入转换器及AI关键字路由-跨Agent会话归纳.md`，并更新 `ai-history/INDEX.md`。

### 代码 / 已观察行为

- 在 `r_equ_all_channel_countries_android` 中补充 `filter: ".*"`，解决多订阅下单独使用 `exclude-filter` 导致内核解析出的底层节点列表为空的问题。
- 将 Android 配置中 `机场悠兔` 的订阅 URL 改为通过 `sub.xeton.dev` 转换拉取，成功将 `type: anytls` 转为标准的 `type: trojan`。
- 将所有 Android 配置中的 `url-test` 策略组参数修改为 `tolerance: 50` 和 `lazy: true`。
- 在全部 8 份 `r_equ_*` 配置文件中加入了 `DOMAIN-KEYWORD,anthropic`、`claude`、`openai`、`gemini`、`chatgpt` 导向 `静态住宅`。

### 已记录的问题 / 风险

- 依赖外部公共订阅转换服务 `sub.xeton.dev`，长效稳定性需保持观察。

### 验证确认

- 用户实测确认：通过 `sub.xeton.dev` 转换后，“机场悠兔”在安卓端 Clash Meta / Mihomo 中节点已可正常连接与测速。
- 8 份配置文件已全部 commit 并成功 push 至 GitHub 远端 `main` 分支。

*(⚠️ 历史改动的详细背景、曾被否决的方案和踩坑记录，统一归档在 `ai-history/INDEX.md` 中。除非当前文档无法解释现有逻辑，否则不要前往该索引进行检索。)*

## [0.1.8] - 2026-07-21

更新时间：2026-07-21 17:06:03 Asia/Shanghai
维护 ID：2026-07-21-170603
Git 锚点：e01a8ee
锚点工作树状态：clean
已覆盖未提交文件：无
增量可信度：精确

### 上下文文档

- 增量更新项目资料，资料维护锚点推进到 2026-07-21 17:06:03。
- 记录 8 份主配置增加了开头 DNS 配置说明。
- 记录项目之前大量的未提交文件与配置已完成 git commit 提交。
- 追加 `ai-history/2026-07-21-170603-完善DNS配置注释-跨Agent会话归纳.md`，并更新 `ai-history/INDEX.md`。

### 代码 / 已观察行为

- `r_equ_all_countries_android` 等 8 份主配置文件在开头“配置文件节点组说明”后增加了统一的 `# DNS 配置说明：` 块。对 Android 说明了 TUN 劫持和 fake-ip，对 Mac 说明了 hijack-dns 和默认 DoH 解析。
- 项目配置和长效文档已通过 `e01a8ee` commit 被纳入 Git，解决之前大量 untracked 和 modified 的累积状态。

## [0.1.7] - 2026-07-20

更新时间：2026-07-20 23:22:00 Asia/Shanghai
Git 锚点：6ec9d3b
锚点工作树状态：dirty
已覆盖未提交文件：r_equ_all_countries_android, r_equ_all_countries_mac, r_equ_all_static_android, r_equ_all_static_mac, r_equ_onlyUS_android, r_equ_onlyUS_mac, raymond_direct.list
增量可信度：根据文档与当前工作树重建

### 上下文文档

- 增量更新项目资料，资料维护锚点推进到 2026-07-20 23:22:00。
- 记录旧单一配置拆分为四份，并新衍生了“静态住宅代理”版本及“所有国家渠道”版本，细化了节点池（仅美国、全地区赠送等）。
- 记录 Mac 配置增加策略组说明注释。
- 追加 `ai-history/2026-07-20-232200-配置文件扩展及静态住宅代理更新-跨Agent会话归纳.md`，并更新 `ai-history/INDEX.md`。

### 代码 / 已观察行为

- 四份主力配置在不同端间演变为独立文件：`r_equ_onlyUS_*`、`r_equ_all_countries_*`，同时新增了 `r_equ_all_static_*` 配置文件。
- Android `r_equ_*` 配置文件中将大量基于 rule-providers 的外部规则集（Apple, Google, Microsoft, Telegram, Steam, Game 等）更新时使用的 `proxy` 从 `PROXY` 更改为 `静态住宅`。
- Mac 配置中增加了关于 `静态住宅`、`赠送美国主选`、`赠送非美兜底` 和 `赠送美国` 等不同级别 Proxy Group 的正则来源与职责分流注释。
- `raymond_direct.list` 及相应 Mac 配置的 `[Host]` 将语音直连 `openspeech.bytedance.com` 放宽为 `bytedance.com` 及 `zijieapi.com` 体系下的所有域名，并通过系统 DNS 直连。

### 已记录的问题 / 风险

- 大量未提交的修改（`raymond_direct.list`、`r_equ_*` 等文件）仍在工作树中，等待最终提交。
- `静态住宅` 代理节点用于拉取 GitHub 上的外部分流规则集，如果所用节点供应商到 GitHub 连接异常，可能导致更新失败。

### 未验证

- 未验证 Shadowrocket / Mihomo Android 最新拆分后的节点正则 (`policy-regex-filter`) 及规则命中是否稳定。
- 未验证静态住宅策略在访问 ChatGPT 等敏感服务时的稳定性。

*(⚠️ 历史改动的详细背景、曾被否决的方案和踩坑记录，统一归档在 `ai-history/INDEX.md` 中。除非当前文档无法解释现有逻辑，否则不要前往该索引进行检索。)*

## [0.1.6] - 2026-06-28

更新时间：2026-06-28 01:45:54 Asia/Shanghai

### Context documents

- 增量更新项目资料，资料维护锚点从 2026-06-15 15:10:14 的 0.1.5 文档推进到 2026-06-28 01:45:54。
- 记录工作树中旧 `local_group.conf` 和 `isp_local` 已删除，新增 `r_equ_onlyUS_mac`、`r_equ_all_countries_mac`、`r_equ_onlyUS_android`、`r_equ_all_countries_android` 四份配置。
- 记录配置矩阵从“单 Mac / 单 Android 主配置”迁移为 Mac / Android × 仅美国优先 / 全地区赠送节点。
- 记录 `logs/` 目录承载 Clash 日志和 SQLite 运行态文件，根目录和 `logs/` 均存在 `.DS_Store`；本轮只确认文件名和大小，未读取内容。
- 追加 `ai-history/2026-06-28-配置拆分为多客户端多节点池.md`，并更新 `ai-history/INDEX.md`。

### Code / behavior observed

- Git 当前仍跟踪 `local_group.conf` 和 `isp_local`，但工作树显示这两个文件为 deleted；四份 `r_equ_*` 配置目前为 untracked。
- `r_equ_onlyUS_mac` 当前为 229 行，保留 Shadowrocket / Mac 仅美国优先结构，`FINAL` 指向 `赠送美国`。
- `r_equ_all_countries_mac` 当前为 225 行，使用 `赠送节点`，`FINAL` 指向 `赠送节点`。
- `r_equ_onlyUS_android` 当前为 686 行，保留 Android / Mihomo 仅美国优先结构，`MATCH` 指向 `赠送美国`。
- `r_equ_all_countries_android` 当前为 639 行，使用 `赠送节点`，`MATCH` 指向 `赠送节点`。
- `.gitignore` 当前内容为 `LOGS/`；`logs/` 目录下存在运行态日志和 SQLite 文件。

### Bugs / risks documented

- 旧文件删除和四份新配置新增尚未提交，提交前需要确认这是预期迁移，不是误删。
- 全地区配置中的 `赠送节点` 只做静态结构确认，尚未验证真实客户端规则命中和节点筛选。
- `.DS_Store` 仍显示为未跟踪文件，后续提交前需要排除；`logs/` 运行态文件不得读取内容或提交。

### Not verified

- 未运行 Shadowrocket / Mac 或 Mihomo Android 客户端导入验证。
- 未验证四份 `r_equ_*` 配置的订阅刷新、远程规则刷新、DNS leak、TUN、fake-ip 或真实规则命中。
- 未读取 `logs/` 下 Clash 日志或 SQLite 运行态数据库内容。

## [0.1.5] - 2026-06-15

更新时间：2026-06-15 15:10:14 Asia/Shanghai

### Context documents

- 增量更新项目资料，资料维护锚点从 2026-06-14 01:25 左右的 0.1.4 文档推进到 2026-06-15 15:10:14。
- 记录最新配置提交 `da42b24 让国外兜底流量走赠送美国`。
- 记录 Shadowrocket `Global` 规则和 `FINAL` 最终兜底已从 `PROXY` 改为 `赠送美国`。
- 记录 Mihomo `Global` 规则和 `MATCH` 最终兜底已从 `PROXY` 改为 `赠送美国`。
- 记录 Mihomo 中 `机场悠兔` provider 的订阅更新出口已从 `DIRECT` 改为 `PROXY`，`机场equaldcdn` 仍保持 `DIRECT`。
- 创建 `ai-history/INDEX.md`，并追加 `ai-history/2026-06-15-context-maintenance.md`。

### Code / behavior observed

- `da42b24` 修改范围为 `isp_local` 和 `local_group.conf`，变更量为 `isp_local` 4 行增删、`local_group.conf` 3 行增删。
- `local_group.conf` 当前仍为 229 行，`Global` 规则和 `FINAL` 都指向 `赠送美国`。
- `isp_local` 当前仍为 686 行，`Global` 规则和 `MATCH` 都指向 `赠送美国`。
- `git rev-list --left-right --count origin/main...HEAD` 输出 `0 0`，本轮检查时 `main` 与 `origin/main` 同步。

### Bugs / risks documented

- 国外通用流量和未命中规则流量改走 `赠送美国` 后，是否会影响需要手工选择 `PROXY` 的少数场景仍待真实使用验证。
- `机场悠兔` provider 更新走 `PROXY` 后，订阅刷新是否稳定仍待客户端验证。
- 长期上下文文档和本地运行态文件仍为未跟踪 Git 文件；运行态文件不应直接提交。

### Not verified

- 未运行 Shadowrocket 或 Mihomo 客户端导入验证。
- 未验证 `Global` / `FINAL` / `MATCH` 真实规则命中。
- 未验证 `机场悠兔` provider 通过 `PROXY` 刷新订阅的客户端表现。
- 未读取 Clash 日志或 SQLite 运行态数据库内容。

## [0.1.4] - 2026-06-14

### Context documents

- 增量更新项目资料，锚点从 `41ee9d7 增加apple一个API 为直连， 增加代码注释` 更新为 `5022a34 把悠兔做为赠送美国的备用节点，并且把非美国的节点做成兜底节点`。
- 记录 `赠送美国` 已调整为美国非静态节点主选、非美非静态节点兜底的两层策略结构；高流量业务规则仍指向总组 `赠送美国`。
- 记录 `main` 当前与 `origin/main` 同步，旧文档中的 `ahead 1` 和需要用户手动 push 的状态已过期。
- 记录本地存在未跟踪的 Clash 日志和 SQLite 运行态文件；本轮只确认文件名和大小，未读取内容。
- 追加 `ai-history/2026-06-14-context-maintenance.md`。

### Code / behavior observed

- `local_group.conf` 当前为 229 行，`[Proxy Group]` 中包含 `赠送美国主选`、`赠送非美兜底` 和 fallback 总组 `赠送美国`。
- `isp_local` 当前为 686 行，`proxy-groups` 中对应包含 `赠送美国主选`、`赠送非美兜底` 和 fallback 总组 `赠送美国`。
- `raymond_direct.list` 当前仍为 42 行；本轮未观察到直连规则集内容变化。
- `5022a34` 修改范围为 `isp_local` 和 `local_group.conf`，未修改 `raymond_direct.list` 或备份配置。

### Bugs / risks documented

- `赠送美国` 的 fallback 行为只做静态确认，Shadowrocket 与 Mihomo 客户端是否按预期在主选不可用时切到非美兜底仍待真机验证。
- `isp_local` 中的 provider 名称和订阅入口仍按敏感配置处理；文档只能记录结构，不记录真实订阅 URL。
- 未跟踪的 `clash-*.log.txt` 和 `proxy-*.db*` 可能包含客户端运行态、节点或访问痕迹，后续提交前需要明确排除或脱敏处理。

### Not verified

- 未运行 Shadowrocket 或 Mihomo 客户端导入验证。
- 未验证美国主选全不可用时的 fallback 切换行为。
- 未验证 YouTube、Google Photos、流媒体和游戏平台等高流量服务在真实客户端中的规则命中。
- 未联网刷新远程 rule providers。

## [0.1.3] - 2026-06-10

### Context documents

- 增量更新项目资料，锚点从 `ca06385 Apple 和 iCloud 加入自定义直连规则` 更新为 `41ee9d7 增加apple一个API 为直连， 增加代码注释`。
- 记录 `apple-cloudkit.com` 已加入 `raymond_direct.list`，作为 Apple CloudKit 相关 API 的直连规则。
- 记录 `isp_local` 和 `local_group.conf` 新增注释，解释 DNS 策略、`RaymondDirect`、`静态住宅`、`赠送美国`、规则集意义和规则顺序。
- 追加 `ai-history/2026-06-10-apple-cloudkit-comments.md`。

### Code / behavior observed

- `raymond_direct.list` 当前为 42 行，包含 Apple / iCloud / CloudKit、飞书 / Lark、网易系、QQ、多点和个别字节语音域名。
- `local_group.conf` 当前为 224 行，已补充 Shadowrocket DNS、策略组、远程直连规则和 `[Host]` 系统 DNS 例外的解释性注释。
- `isp_local` 当前为 638 行，已补充 Mihomo DNS、TUN、rule providers、策略组和规则顺序的解释性注释。
- 最新提交锚点为 `41ee9d7 增加apple一个API 为直连， 增加代码注释`，本轮检查时 `main` 相对 `origin/main` ahead 1。

### Bugs / risks documented

- 长期上下文文档截至 2026-06-10 仍为未跟踪 Git 文件，后续需要用户决定是否提交。
- Apple / iCloud / CloudKit 已进入 `raymond_direct.list` 直连，但 Shadowrocket `[Host]` 尚未同步系统 DNS；是否需要同步仍待真机验证。
- `raymond_direct.list` 同时影响直连路由和 Mihomo 系统 DNS，新增域名仍需要评估 DNS 泄露面。

### Not verified

- 未运行 Shadowrocket 或 Mihomo 客户端导入验证。
- 未验证 Apple CloudKit 在真实客户端中的规则命中、连通性和 DNS policy 命中日志。
- 未联网刷新远程 rule providers。

## [0.1.2] - 2026-06-10

### Context documents

- 增量更新项目资料，锚点从 `45deae4 Enable Mihomo sniffer for pure IP traffic` 更新为 `ca06385 Apple 和 iCloud 加入自定义直连规则`。
- 记录 `raymond_direct.list` 已成为两端共享的用户自维护直连规则源。
- 记录 Mihomo `dns.nameserver-policy` 通过 `rule-set:RaymondDirect` 控制系统 DNS 的当前设计。
- 记录 Shadowrocket 需要在 `[Host]` 中手动同步 `server:system` 域名，不能直接引用远程 rule-set 做 DNS policy。
- 追加 `ai-history/2026-06-10-context-maintenance.md`。

### Code / behavior observed

- `raymond_direct.list` 当前为 41 行，包含 Apple / iCloud、飞书 / Lark、网易系、QQ、多点和个别字节语音域名。
- `local_group.conf` 当前为 211 行，已引用 `raymond_direct.list` 并为飞书 / Lark、网易系等自定义直连域名维护 `server:system` 条目。
- `isp_local` 当前为 591 行，`RaymondDirect` provider 同时被 `rules` 和 `dns.nameserver-policy` 引用。
- 最新提交锚点为 `ca06385 Apple 和 iCloud 加入自定义直连规则`，本轮检查时 `main` 与 `origin/main` 同步。

### Bugs / risks documented

- 长期上下文文档截至 2026-06-10 仍为未跟踪 Git 文件，后续需要用户决定是否提交。
- `raymond_direct.list` 现在会同时影响直连路由和 Mihomo 系统 DNS，新增域名时需要评估 DNS 泄露面。
- Codex 多次尝试代推 VPN 配置到 GitHub 时被安全层拒绝；后续若需发布，通常需要用户在本机终端执行。

### Not verified

- 未运行 Shadowrocket 或 Mihomo 客户端导入验证。
- 未验证 `RaymondDirect` 在真实客户端中的 DNS policy 命中日志。
- 未验证 Apple / iCloud、飞书云文档、网易系域名在真实网络下的直连与系统 DNS 效果。

## [0.1.1] - 2026-06-08

### Context documents

- 增量更新项目资料，锚点从 `f8b098e Initial commit` 更新为 `45deae4 Enable Mihomo sniffer for pure IP traffic`。
- 记录 `2e94fb9` 中 YouTube App / 媒体域名与 Google Photos 备份流量改走 `赠送美国` 的分流意图。
- 记录 `45deae4` 中 Mihomo sniffer 针对纯 IP 流量的配置变化。
- 追加 `ai-history/2026-06-08-context-maintenance.md`。

### Code / behavior observed

- `local_group.conf` 当前为 155 行，规则区已包含 YouTube 和 Google Photos 的显式前置规则。
- `isp_local` 当前为 583 行，顶层已包含 `sniffer` 区块。
- `lazy_group_防DNS泄露去广告后的备份.conf` 保持 151 行，本轮未观察到提交后的变化。

### Bugs / risks documented

- 长期上下文文档截至 2026-06-08 仍为未跟踪 Git 文件，后续需要用户决定是否提交。
- sniffer、纯 IP 解析、YouTube / Google Photos 分流只做静态确认，真实客户端行为仍需验证。

### Not verified

- 未运行 Shadowrocket 或 Mihomo 客户端导入验证。
- 未验证 YouTube、Google Photos、Google API、AI 工具之间的规则命中优先级。
- 未验证 Mihomo sniffer 对纯 IP 流量的实际效果。

## [0.1.0] - 2026-06-07

### Context documents

- 初始化 `AGENTS.md`、`README.md`、`docs/PROJECT_CONTEXT.md`、`docs/TESTING.md`、`docs/HANDOFF.md`。
- 初始化 `ai-history/2026-06-07-context-maintenance.md`。
- 明确 Shadowrocket 与 Mihomo 配置文件职责。
- 记录敏感订阅入口处理规则。

### Code / behavior observed

- 观察到仓库包含 3 个配置文件，无脚本目录、无自动化测试、无既有长期上下文文档。
- `local_group.conf` 和备份配置为 Shadowrocket 格式。
- `isp_local` 为 Clash Meta for Android / Mihomo 配置，并从文件头注释可确认由 `local_group.conf` 重建。

### Bugs / risks documented

- `isp_local` 包含代理订阅入口，公开分享或文档引用前必须脱敏。
- 当前没有自动化配置校验；规则引用、客户端兼容性和 DNS leak 需要人工验证。

### Not verified

- 未运行 Shadowrocket 或 Mihomo 客户端导入验证。
- 未联网刷新远程 rule providers。
- 未验证真实节点连通性、DNS leak 或 QUIC block 行为。
