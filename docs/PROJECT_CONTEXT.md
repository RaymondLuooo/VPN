# Project Context

最近更新时间：2026-07-31 14:30:00 Asia/Shanghai

## Background

本仓库用于维护个人代理客户端配置。当前配置覆盖 Shadowrocket / Mac 与 Android Mihomo / Clash Meta 两类客户端，并按“仅美国优先”、“全地区赠送节点”、“全渠道全地区”和“全静态住宅代理”拆分为共 8 份配置文件。

项目不是通用 VPN 产品，也不是节点管理系统；它更接近一组可长期维护的本地配置文件。

## Business goal

核心目标是让用户在不同客户端上获得稳定、可解释的代理分流行为：

- 国内、局域网、银行和常用中国服务尽量直连。
- AI（OpenAI/ChatGPT、Anthropic/Claude、Google Gemini 等）、Google、GitHub 等账号和风控敏感服务使用更稳定的 `静态住宅` 策略组，并通过 `DOMAIN-KEYWORD` 优先精准捕获。
- Apple / iCloud / CloudKit、飞书 / Lark、网易系等用户明确要求的服务进入自维护直连规则集。
- 仅美国优先配置中，YouTube、Netflix、Disney、HBO、Spotify、Telegram、PayPal、Twitter、Facebook、Amazon、TikTok、游戏平台、Microsoft 等国外高流量服务使用 `赠送美国` 总策略；当前该策略优先美国非静态节点，主选不可用时再使用非美非静态兜底。
- 全地区及全渠道配置中，国外高流量服务和国外通用兜底使用 `赠送节点`，从指定国家和地区的非静态赠送节点中测速选择。
- 解决 Android Mihomo 底层对于单独使用 `exclude-filter` 导致的解构节点列表为空 bug（补充 `filter: ".*"`）。
- 解决了“机场悠兔”在 Clash Meta 协议下发私有协议 `type: anytls` 导致内核解析为 0 节点的问题，通过 `sub.xeton.dev` 订阅转换清洗为标准的 `type: trojan`。
- 优化 `url-test` 测速策略（设置 `tolerance: 50` 防止节点频繁切换断流，开启 `lazy: true` 减少手机后台耗电与无意义测速）。
- 未被更具体规则命中的国外通用流量也进入对应的赠送策略组，让普通国外兜底流量默认走低成本节点池，而不是落到手工 `PROXY`。
- 广告与跟踪规则优先拦截。
- DNS 尽量经代理解析，降低 DNS 泄露风险。
- 对少数明确要本地化解析的直连域名，允许使用系统 DNS，以换取国内 CDN 和系统服务稳定性。

## User workflow

1. 用户选择目标客户端（Android Mihomo 或 Mac Shadowrocket）。
2. 使用对应配置文件导入客户端（或通过 raw.githubusercontent.com 直连更新）。
3. 客户端拉取订阅节点和远程规则。
4. 用户在客户端里选择可用策略组或节点。
5. 通过连通性测试、DNS leak 检查和典型服务访问确认行为。

## Core concepts

Shadowrocket 配置：

- 使用 `[General]`、`[Proxy Group]`、`[Rule]`、`[Host]`、`[URL Rewrite]`、`[MITM]` 结构。
- `r_equ_onlyUS_mac` 是仅美国优先的 Mac / Shadowrocket 配置。
- `r_equ_all_countries_mac` 是全地区赠送节点的 Mac / Shadowrocket 配置。
- `r_equ_all_channel_countries_mac` 是全渠道全地区赠送节点的 Mac / Shadowrocket 配置。
- `r_equ_all_static_mac` 是全量静态住宅节点配置。
- 备份配置保留更通用的区域节点分组。
- `raymond_direct.list` 通过远程 `RULE-SET` 进入 `DIRECT`。
- `[Host]` 负责 Shadowrocket 的域名到 DNS 服务器映射；它不能直接引用远程 rule-set，因此 `server:system` 需要手动同步。

Mihomo 配置：

- 使用 YAML 格式。
- 关键区块包括 `sniffer`、`tun`、`dns`、`proxy-providers`、`proxy-groups`、`rule-providers` 和 `rules`。
- `r_equ_onlyUS_android` 是仅美国优先的 Android / Mihomo 配置。
- `r_equ_all_countries_android` 是全地区赠送节点的 Android / Mihomo 配置。
- `r_equ_all_channel_countries_android` 是全渠道全地区赠送节点的 Android / Mihomo 配置。
- `r_equ_all_static_android` 是全量静态住宅节点的 Android / Mihomo 配置。
- Android 配置包含订阅入口，不能外泄。
- `RaymondDirect` provider 通过 GitHub raw 拉取 `raymond_direct.list`，并同时用于 `rules` 直连和 `dns.nameserver-policy` 系统 DNS。

策略组：

- `PROXY` 是手工选择入口；当前不再承担 `Global` / `FINAL` / `MATCH` 的默认国外兜底。
- `静态住宅` 用于 AI（包含新增 `DOMAIN-KEYWORD`）、Google、GitHub 等敏感服务。
- `赠送美国主选` 用于名字匹配美国且排除静态住宅的非静态节点。
- `赠送非美兜底` 用于美国非静态节点不可用时的日本、台湾或新加坡等非静态节点兜底。
- `赠送美国` 是高流量服务继续引用的总策略组；它把美国主选和非美兜底封装成 fallback 结构。
- `赠送节点` 是全地区配置使用的高流量和国外兜底策略组；当前按美国、日本、台湾、新加坡、韩国等非静态赠送节点测速选择。
- `RaymondDirect` 代表用户自维护直连域名集合；当前包含 Apple / iCloud / CloudKit、飞书 / Lark、网易系、QQ、多点和个别字节语音域名。

## Core data flow

Shadowrocket：

1. 客户端读取本地 Mac / Shadowrocket 配置文件。
2. 本地规则引用远程规则集。
3. DNS 根据 `[General]` 中的 DoH、hijack 和 `[Host]` 中的 `server:system` 条目处理。
4. 命中规则（包括关键词 DOMAIN-KEYWORD,anthropic/claude/openai/gemini/chatgpt）后流量进入对应策略组。
5. 未命中规则进入对应配置的 `FINAL` 兜底：仅美国优先为 `赠送美国`，全地区为 `赠送节点`。

Mihomo：

1. 客户端读取目标 Android / Mihomo 配置文件。
2. `proxy-providers` 拉取订阅节点（对于悠兔采用 `sub.xeton.dev` 转换拉取）。
3. `rule-providers` 拉取远程规则。
4. sniffer、TUN、fake-ip DNS 和 `nameserver-policy` 共同影响域名还原、DNS 解析和系统流量接管。
5. `rules` 按顺序选择 REJECT、DIRECT、指定策略组；`Global` 和最终 `MATCH` 当前兜底到对应赠送策略组。

## Important assumptions

- 已确认：当前仓库没有自动化测试脚本。
- 已确认：`r_equ_*_android` 中存在代理订阅入口或 provider 配置。
- 已确认：`r_equ_*_android` 顶层已包含 sniffer 配置。
- 已确认：`raymond_direct.list` 已被 `r_equ_*_mac` 和 `r_equ_*_android` 引用。
- 已确认：`r_equ_*_android` 已通过 `dns.nameserver-policy` 引用 `rule-set:RaymondDirect`，使该规则集命中的域名使用系统 DNS。
- 已确认：`r_equ_*_mac` 与 `r_equ_*_android` 均已把 YouTube App / 媒体域名和 Google Photos 相关域名放在宽泛 Google 规则前。
- 已确认：已把 `anthropic`、`claude`、`openai`、`gemini`、`chatgpt` 的 `DOMAIN-KEYWORD` 添加到全部 8 份 `r_equ_*` 配置文件中。
- 已确认：已在 `r_equ_all_channel_countries_android` 等配置中为 `exclude-filter` 的策略组补充 `filter: ".*"`，解决了 Mihomo 下发节点列表为空的问题。
- 已确认：已将 Android 配置中的 `url-test` 策略组参数更新为 `tolerance: 50` 和 `lazy: true`。
- 已确认：已将 Android 配置中的“机场悠兔”订阅 URL 升级为通过 `sub.xeton.dev` 订阅转换，解决了 `type: anytls` 不被内核支持的故障。
- 已确认：长期上下文文档及最新衍生配置文件已被纳入 Git 版本控制并推送 GitHub。
- 部分确认：Android 配置延续原 Mihomo 配置设计；本轮未验证生成脚本或转换过程。
- 待验证：Shadowrocket 与 Mihomo 两份配置在真实客户端中的行为是否完全等价。
- 待验证：Mihomo sniffer 对纯 IP 流量的实际识别效果。
- 待验证：`RaymondDirect` 在真实客户端中是否按预期同时控制直连和 DNS policy。
- 待验证：`赠送美国` 在主选不可用时是否能按客户端预期切换到非美兜底。
- 待验证：`Global` / `FINAL` / `MATCH` 改走 `赠送美国` 后的真实规则命中和用户体验。

## Open questions

- `sub.xeton.dev` 公益转换服务的长期稳定性及隐私安全，如需自建可后续推进。
- 是否需要新增脱敏模板和静态检查脚本，需要用户确认后再改。
