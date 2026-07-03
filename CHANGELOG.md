# Changelog

最近更新时间：2026-06-28 01:45:54 Asia/Shanghai

## [0.1.6] - 2026-06-28

更新时间：2026-06-28 01:45:54 Asia/Shanghai

### Context documents

- 增量更新项目资料，资料维护锚点从 2026-06-15 15:10:14 的 0.1.5 文档推进到 2026-06-28 01:45:54。
- 记录工作树中旧 `local_group.conf` 和 `isp_local` 已删除，新增 `r_equ_onlyUS_mac`、`r_equ_all_countries_mac`、`r_equ_onlyUS_android`、`r_equ_all_countries_android` 四份配置。
- 记录配置矩阵从“单 Mac / 单 Android 主配置”迁移为 Mac / Android × 仅美国优先 / 全地区赠送节点。
- 记录 `logs/` 目录承载 Clash 日志和 SQLite 运行态文件，根目录和 `logs/` 均存在 `.DS_Store`；本轮只确认文件名和大小，未读取内容。
- 追加 `ai-history/codex/2026-06-28-配置拆分为多客户端多节点池.md`，并更新 `ai-history/INDEX.md`。

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
- 创建 `ai-history/INDEX.md`，并追加 `ai-history/codex/2026-06-15-context-maintenance.md`。

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
- 追加 `ai-history/codex/2026-06-14-context-maintenance.md`。

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
- 追加 `ai-history/codex/2026-06-10-apple-cloudkit-comments.md`。

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
- 追加 `ai-history/codex/2026-06-10-context-maintenance.md`。

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
- 追加 `ai-history/codex/2026-06-08-context-maintenance.md`。

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
- 初始化 `ai-history/codex/2026-06-07-context-maintenance.md`。
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
