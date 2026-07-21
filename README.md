# VPN

最近更新时间：2026-07-20 23:22:00 Asia/Shanghai

## Overview

这是一个个人 VPN / 代理分流配置仓库，用于维护 Shadowrocket / Mac 和 Clash Meta / Mihomo Android 客户端配置。

当前仓库重点是：

- 防 DNS 泄露和统一 DNS 策略。
- 去广告与跟踪拦截。
- AI 工具、Google / GitHub、流媒体、游戏平台、国内常用服务的分流策略。
- Mac / Android 配置按“全地区赠送节点”和“仅美国优先”两类并行维护。

## Current status

已完成：

- `r_equ_onlyUS_mac` 已包含 Shadowrocket / Mac 仅美国优先配置、DNS、策略组、规则、Host、URL Rewrite 和 MITM 节。
- `r_equ_all_countries_mac` 已包含 Shadowrocket / Mac 全地区赠送节点配置，使用 `赠送节点` 作为国外高流量和兜底策略组。
- `r_equ_onlyUS_android` 已包含 Mihomo Android 仅美国优先配置，包括 sniffer、TUN、fake-ip DNS、proxy provider、rule providers、proxy groups 和 rules。
- `r_equ_all_countries_android` 已包含 Mihomo Android 全地区赠送节点配置，使用 `赠送节点` 作为国外高流量和兜底策略组。
- `lazy_group_防DNS泄露去广告后的备份.conf` 已作为 Shadowrocket 备份配置保留。
- `raymond_direct.list` 已作为用户自维护直连规则集被两端配置引用，用于维护 Apple / iCloud / CloudKit、飞书、网易系、QQ、多点等明确要直连的域名。
- Android 配置已通过 `dns.nameserver-policy` 引用 `rule-set:RaymondDirect`，使该规则集命中的域名使用 Android 当前网络的系统 DNS；Mac / Shadowrocket 侧通过 `[Host]` 手动同步部分域名到 `server:system`。
- YouTube App / 媒体域名已在 Google 宽规则前显式进入 `赠送美国`，避免被 `googleapis`、`googleusercontent` 或 `gstatic` 提前命中到 `静态住宅`。
- Google Photos 备份和媒体传输域名已进入 `赠送美国`，用于降低静态住宅节点流量消耗。
- `赠送美国` 已拆成美国非静态节点主选和非美非静态节点兜底的 fallback 结构，高流量服务规则继续指向总组 `赠送美国`。
- 未被更具体规则命中的国外通用流量现在由 Shadowrocket `Global` / `FINAL` 和 Mihomo `Global` / `MATCH` 统一交给 `赠送美国`，不再落到手工 `PROXY` 兜底。
- Mihomo 中 `机场悠兔` 订阅更新出口已改为 `PROXY`，`机场equaldcdn` 订阅更新出口仍保持 `DIRECT`。
- Android 配置中 rule-providers 更新代理已更改为 `静态住宅`。
- `bytedance.com` / `zijieapi.com` 已被放宽加入直连规则，并在 Mac 中设置系统 DNS 直连。
- 配置文件衍生出 `r_equ_all_static` 静态住宅版本和 `r_equ_all_channel_countries`。
- `.gitignore` 已出现并忽略 `LOGS/`，运行态日志和 SQLite 文件位于 `logs/` 目录；本轮只确认存在性和大小，未读取内容。
- 项目长期上下文文档已初始化。

部分完成：

- Android 配置从结构看延续了原 Mihomo 配置设计，但本轮没有运行客户端加载验证。
- Mac 与 Android 的规则语义大体对应，但未逐条做等价性测试。
- 长期上下文文档及最新衍生配置仍显示为未提交文件。
- `.DS_Store` 仍在仓库根目录显示为未跟踪文件；提交前需要排除。

未完成：

- 没有自动化配置校验脚本。
- 没有脱敏后的示例配置模板。
- 没有记录真实客户端导入后的截图或验证日志。

待验证：

- Shadowrocket 真机导入是否仍正常。
- Mihomo Android 客户端是否能成功拉取订阅和远程 rule providers。
- DNS leak、QUIC block、fake-ip 与 TUN strict-route 的实际表现。
- 四份 `r_equ_*` 配置在对应客户端中的导入、订阅刷新、远程规则刷新和规则命中。

## Project goals

- 保持一份可审计、可交接的代理规则仓库。
- 在不泄露订阅、节点或凭据的前提下记录配置意图。
- 让未来 agent 能快速判断哪个文件用于哪个客户端、哪些字段不能随意改、修改后应该怎样验证。

## Main workflow

1. 修改前先阅读 `AGENTS.md` 和 `docs/PROJECT_CONTEXT.md`。
2. 判断目标客户端是 Shadowrocket 还是 Mihomo。
3. 只修改目标配置对应的最小范围。
4. 修改后检查策略组名称是否被规则引用、DNS 策略是否被意外放宽、敏感 URL 是否未被输出。
5. 用户在真实客户端导入验证后，再记录验证结果。

## Directory structure

- `r_equ_onlyUS_mac`: Shadowrocket / Mac 仅美国优先配置。
- `r_equ_all_countries_mac`: Shadowrocket / Mac 全地区赠送节点配置。
- `r_equ_onlyUS_android`: Clash Meta for Android / Mihomo 仅美国优先配置。
- `r_equ_all_countries_android`: Clash Meta for Android / Mihomo 全地区赠送节点配置。
- `r_equ_all_static_mac` / `android`: 全量静态住宅节点配置。
- `lazy_group_防DNS泄露去广告后的备份.conf`: Shadowrocket 备份配置。
- `raymond_direct.list`: GitHub-hosted 自维护直连规则集，被 `r_equ_*_mac` 和 `r_equ_*_android` 引用。
- `logs/`: 本地客户端运行态产物目录，包含 Clash 日志、SQLite 数据库及系统元数据；可能含敏感运行信息，不应读取内容、直接提交或输出内容。
- `docs/PROJECT_CONTEXT.md`: 项目背景、核心概念和数据流。
- `docs/TESTING.md`: 静态检查和真机验证清单。
- `docs/HANDOFF.md`: 给下一个 agent 的交接文档。
- `ai-history/codex/`: Codex 维护记录。

## Key configuration files

`r_equ_onlyUS_mac`:

- 已确认是 Shadowrocket / Mac 格式。
- 关注点是 DNS 统一代理、防泄露、广告拦截、AI 与 Google / GitHub 走静态住宅、国外高流量服务走赠送美国、国内和局域网直连。
- `赠送美国` 当前由 `赠送美国主选` 和 `赠送非美兜底` 组成，业务规则继续引用总组，避免每条高流量规则重复关心兜底细节。
- `Global` 规则和 `FINAL` 最终兜底当前都指向 `赠送美国`，让未单独分类的国外代理流量默认进入低成本节点池。
- 通过远程 `RULE-SET` 引用 `raymond_direct.list`；由于 Shadowrocket `[Host]` 不能引用远程 rule-set，系统 DNS 需要在 `[Host]` 中手动维护。

`r_equ_all_countries_mac`:

- 已确认是 Shadowrocket / Mac 格式。
- 使用 `赠送节点` 从指定国家和地区的非静态赠送节点中测速选择；`Global` 规则和 `FINAL` 最终兜底当前都指向 `赠送节点`。
- 适合需要全地区赠送节点池的 Mac / Shadowrocket 场景；真实客户端行为待验证。

`lazy_group_防DNS泄露去广告后的备份.conf`:

- 已确认是 Shadowrocket 格式。
- 关注点是保留原始或更通用的区域节点策略组，适合作为回退参考。

`r_equ_onlyUS_android`:

- 已确认是 Mihomo / Clash Meta YAML 风格配置。
- 已启用 sniffer，并包含纯 IP 流量解析相关配置；实际兼容性需要客户端验证。
- `赠送美国` 当前是 fallback 策略组，优先美国非静态节点主选，主选不可用时再走非美非静态兜底。
- `Global` 规则和 `MATCH` 最终兜底当前都指向 `赠送美国`。
- `机场悠兔` provider 当前通过 `PROXY` 更新订阅；`机场equaldcdn` provider 仍通过 `DIRECT` 更新订阅。
- `RaymondDirect` rule provider 使用 `raymond_direct.list`，并被 `rules` 和 `dns.nameserver-policy` 同时引用。
- 包含 proxy provider 订阅入口，属于敏感信息。
- 不要在聊天、文档或提交说明中输出真实订阅 URL。

`r_equ_all_countries_android`:

- 已确认是 Mihomo / Clash Meta YAML 风格配置。
- 使用 `赠送节点` 从指定国家和地区的非静态节点中测速选择；`Global` 规则和 `MATCH` 最终兜底当前都指向 `赠送节点`。
- 仅确认结构和关键引用，未做客户端导入或 provider 刷新验证。

`raymond_direct.list`:

- 已确认是文本规则列表。
- 当前承担“直连规则源”和“部分系统 DNS 策略源”两类职责。
- 已包含 Apple / iCloud / CloudKit、飞书 / Lark、网易系、QQ、多点和个别字节语音域名。
- 新增域名时要确认是否同时适合系统 DNS，避免把敏感或海外服务误放入运营商 DNS。

## How to run

当前没有本地运行命令。

使用方式取决于客户端：

- Shadowrocket / Mac: 按目标节点池导入 `r_equ_onlyUS_mac` 或 `r_equ_all_countries_mac`。
- Mihomo / Clash Meta for Android: 按目标节点池导入 `r_equ_onlyUS_android` 或 `r_equ_all_countries_android`。

## How to test

详见 `docs/TESTING.md`。当前自动化测试为 `无`。

## Known limitations

- 2026-07-20 本轮维护只做静态上下文更新，没有连接外网更新规则集，也没有修改 VPN 配置。
- 2026-07-20 本轮没有真实启动 Shadowrocket 或 Mihomo 客户端。
- `赠送美国` 的 fallback 结构尚未做主选失败场景实测。
- `赠送节点` 的全地区节点筛选和兜底行为尚未做真机实测。
- 国外通用流量改走 `赠送美国` 兜底后，尚未通过真实规则命中日志验证。
- `机场悠兔` 订阅更新改走 `PROXY` 后，尚未做客户端 provider 刷新实测。
- Android 配置含敏感订阅入口，后续公开分享前必须脱敏。

## Next steps

- 建立脱敏示例配置，例如按 Android / Mac 与节点池拆分的 example 文件。
- 如果用户确认，可以增加只读静态检查脚本，用于验证策略组引用和敏感字段泄露风险。
- 在真实客户端验证后，把结果追加到 `docs/HANDOFF.md`。
- 提交前明确排除或脱敏本地 Clash 日志和 SQLite 运行态文件。
- 真机验证 `Global` / `FINAL` / `MATCH` 兜底进入 `赠送美国` 后的实际规则命中和节点选择。
- 提交配置迁移前确认旧 `local_group.conf` / `isp_local` 删除和四份 `r_equ_*` 新配置是预期状态。
