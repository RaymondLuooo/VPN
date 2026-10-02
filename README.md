# VPN

最近更新时间：2026-07-31 14:30:00 Asia/Shanghai

## Overview

这是一个个人 VPN / 代理分流配置仓库，用于维护 Shadowrocket / Mac 和 Clash Meta / Mihomo Android 客户端配置。

当前仓库重点是：

- 防 DNS 泄露和统一 DNS 策略。
- 去广告与跟踪拦截。
- AI 工具（支持精准关键词捕获）、Google / GitHub、流媒体、游戏平台、国内常用服务的分流策略。
- Mac / Android 配置按“全地区赠送节点”、“仅美国优先”、“全渠道全地区”及“全静态住宅代理”4类矩阵式（共 8 份）并行维护。

## Current status

已完成：

- `r_equ_onlyUS_mac` 已包含 Shadowrocket / Mac 仅美国优先配置、DNS、策略组、规则、Host、URL Rewrite 和 MITM 节。
- `r_equ_all_countries_mac` 已包含 Shadowrocket / Mac 全地区赠送节点配置，使用 `赠送节点` 作为国外高流量和兜底策略组。
- `r_equ_onlyUS_android` 已包含 Mihomo Android 仅美国优先配置，包括 sniffer、TUN、fake-ip DNS、proxy provider、rule providers、proxy groups 和 rules。
- `r_equ_all_countries_android` 已包含 Mihomo Android 全地区赠送节点配置，使用 `赠送节点` 作为国外高流量和兜底策略组。
- `r_equ_all_channel_countries_*` / `r_equ_all_static_*` 已作为渠道衍生和全静态住宅衍生配置完备维护。
- 全量 8 份 `r_equ_*` 配置文件均已加入 `DOMAIN-KEYWORD` 规则（`anthropic`、`claude`、`openai`、`gemini`、`chatgpt`）强行锁死进入 `静态住宅` 策略组。
- 修复了 Mihomo 下 `exclude-filter` 缺少 `filter` 导致解析出的底层节点列表为空的严重兼容性 bug。
- 解决了“机场悠兔”下发 `type: anytls` 私有协议导致的 Mihomo 核心报错，配置中已接入 `sub.xeton.dev` 云端订阅转换清洗为 `type: trojan`。
- 优化了 Android 下所有 `url-test` 策略组的测速策略：`tolerance: 50`（防延迟抖动频频横跳）、`lazy: true`（减少后台无意义唤醒和耗电）。
- `lazy_group_防DNS泄露去广告后的备份.conf` 已作为 Shadowrocket 备份配置保留。
- `raymond_direct.list` 已作为用户自维护直连规则集被两端配置引用，用于维护 Apple / iCloud / CloudKit、飞书、网易系、QQ、多点等明确要直连的域名。
- 所有 8 份 `r_equ_*` 主配置均已在开头补充统一的 DNS 策略说明注释。
- 项目长期上下文文档与检索凭证已完整持久化。

部分完成：

- “机场悠兔”订阅转换在真实 Android 客户端上经用户实测可正常加载和测速。
- Mac 与 Android 的规则语义大体对应，已补充关键词对齐。

未完成：

- 没有自动化配置校验脚本。
- 没有脱敏后的示例配置模板。

待验证：

- 持续观察 `sub.xeton.dev` 转换链接的长期稳定情况。

## Project goals

- 保持一份可审计、可交接的代理规则仓库。
- 在不泄露订阅、节点或凭据的前提下记录配置意图。
- 让未来 agent 能快速判断哪个文件用于哪个客户端、哪些字段不能随意改、修改后应该怎样验证。

## Main workflow

1. 修改前先阅读 `AGENTS.md`，并按需查阅 `docs/index.md` 检索相关资料（如 `docs/architecture.md`）。
2. 判断目标客户端是 Shadowrocket 还是 Mihomo。
3. 只修改目标配置对应的最小范围。
4. 修改后检查策略组名称是否被规则引用、DNS 策略是否被意外放宽、敏感 URL 是否未被输出。
5. 用户在真实客户端导入验证后，再记录验证结果。

## Directory structure

- `r_equ_onlyUS_mac`: Shadowrocket / Mac 仅美国优先配置。
- `r_equ_all_countries_mac`: Shadowrocket / Mac 全地区赠送节点配置。
- `r_equ_onlyUS_android`: Clash Meta for Android / Mihomo 仅美国优先配置。
- `r_equ_all_countries_android`: Clash Meta for Android / Mihomo 全地区赠送节点配置。
- `r_equ_all_channel_countries_mac` / `android`: 全渠道全地区衍生配置。
- `r_equ_all_static_mac` / `android`: 全量静态住宅节点配置。
- `lazy_group_防DNS泄露去广告后的备份.conf`: Shadowrocket 备份配置。
- `raymond_direct.list`: GitHub-hosted 自维护直连规则集，被 `r_equ_*_mac` 和 `r_equ_*_android` 引用。
- `logs/`: 本地客户端运行态产物目录，包含 Clash 日志、SQLite 数据库及系统元数据；可能含敏感运行信息，不应读取内容、直接提交或输出内容。
- `docs/architecture.md`: 项目核心诉求、分流架构、策略组与模块职责（当前唯一法定真相）。
- `docs/index.md`: 现行知识索引枢纽（稳定 ID、匹配线索、读取触发）。
- `docs/decisions/`: 长期架构决策落点目录（内嵌 Lessons 与 Validation）。
- `docs/ai-history/`: 跨 Agent 历史演进与事故证据链目录（索引为 `docs/ai-history/index.md`）。

## How to run

通过 raw.githubusercontent.com 或直接导入客户端配置文件：

- Shadowrocket / Mac: 导入对应 `r_equ_*_mac` 文件。
- Mihomo / Clash Meta for Android: 导入对应 `r_equ_*_android` 文件。

## Testing

本项目的主要风险不是代码编译失败，而是配置语义偏差、客户端不兼容、规则引用失效、DNS 泄露和敏感订阅外泄。验证分为静态检查和客户端真机检查；未做真机检查时，结论标注为 `待验证`。

### 环境与敏感信息检查
- 确认目标客户端：Shadowrocket 或 Mihomo / Clash Meta for Android。
- 确认目标文件：`r_equ_onlyUS_mac`、`r_equ_all_countries_mac`、`r_equ_onlyUS_android`、`r_equ_all_countries_android`、`r_equ_all_static_mac` / `android`、`r_equ_all_channel_countries_mac` / `android`、备份配置或 `raymond_direct.list`。
- 确认没有把订阅 URL、token、节点凭据复制到聊天、文档或公开仓库说明。

### 静态检查清单

**Shadowrocket 配置**：
- 检查 `[General]`、`[Proxy Group]`、`[Rule]`、`[Host]`、`[URL Rewrite]`、`[MITM]` 节是否存在。
- 检查规则里引用的策略组名称在 `[Proxy Group]` 中存在。
- 对 `r_equ_onlyUS_mac`，检查 `赠送美国` 是否仍引用 `赠送美国主选` 和 `赠送非美兜底`，且两个子组都排除静态住宅节点。
- 对 `r_equ_all_countries_mac`，检查 `赠送节点` 是否仍筛选指定国家和地区的非静态赠送节点。
- 检查 `DOMAIN-KEYWORD` 中的 `anthropic`、`claude`、`openai`、`gemini`、`chatgpt`、`grok`、`muse` 等是否准确指向 `静态住宅` 策略组。
- 检查 `Global` 规则和 `FINAL` 最终兜底是否仍指向目标配置对应的赠送策略组。
- 检查最终规则是否仍有兜底策略。
- 检查 DNS 相关字段是否被意外改成系统直连。
- 检查全部 `r_equ_*_mac` 的 `dns-server` 与 `fallback-dns-server` 都明确绑定 `静态住宅`，不能回退为跟随首页默认节点的 `PROXY`。
- 检查 MITM 是否仍处于用户预期范围。
- 修改 `raymond_direct.list` 后，检查两份 `r_equ_*_mac` 是否仍在原顺序位置引用远程规则集。
- 若新增域名需要系统 DNS，检查 `[Host]` 是否同时覆盖裸域和通配子域，例如 `example.com` 与 `*.example.com`。

**Mihomo 配置**：
- 检查 YAML 顶层键：`sniffer`、`tun`、`dns`、`proxy-providers`、`proxy-groups`、`rule-providers`、`rules`。
- 检查 `proxy-groups` 中的 `url-test` 策略组参数是否为 `tolerance: 50` 且 `lazy: true`。
- 检查 `赠送节点` 等组合使用 `exclude-filter` 的策略组是否明确包含 `filter: ".*"`，避免出现解构节点列表为空的内核 bug。
- 检查 `proxy-providers` 中的“机场悠兔”订阅 URL 是否使用了 `sub.xeton.dev` 转换链接（以避免 `type: anytls` 被内核拒绝）。
- 检查 `rules` 中是否已包含 `DOMAIN-KEYWORD`（`anthropic` / `claude` / `openai` / `gemini` / `chatgpt` / `grok` / `muse`）并优先指向 `静态住宅`。
- 检查 YouTube App / 媒体域名和 Google Photos 域名是否仍在宽泛 Google 规则前。
- 检查 `proxy-providers` 中订阅入口没有被输出或提交到公开文档。
- 检查 `rules` 引用的策略组名称存在于 `proxy-groups`。
- 对 `r_equ_onlyUS_android`，检查 `赠送美国` fallback 组是否仍只引用 `赠送美国主选` 和 `赠送非美兜底`，并确认两个子组的 filter / exclude-filter 没有把静态住宅节点纳入高流量池。
- 对 `r_equ_all_countries_android`，检查 `赠送节点` 是否仍筛选指定国家和地区的非静态节点。
- 检查 `Global` 规则和 `MATCH` 最终兜底是否仍指向目标配置对应的赠送策略组。
- 检查 `rule-providers` 名称与 `rules` 中的 `RULE-SET` 引用一致。
- 检查 `RaymondDirect` provider 是否仍被 `rules` 和 `dns.nameserver-policy` 同时引用。
- 修改 `raymond_direct.list` 时，检查新增规则是否为域名类规则；非域名类规则不适合作为 DNS policy 匹配依据。
- 检查 `MATCH` 兜底仍存在。

**`raymond_direct.list` 检查**：
- 检查规则语法只使用目标客户端兼容的规则类型。
- 检查宽后缀规则不会意外覆盖敏感海外服务。
- 检查新增域名是否同时适合直连和系统 DNS。
- 检查 Apple / iCloud / CloudKit、飞书 / Lark、网易系等分组注释是否仍与实际规则一致。

**本地运行态文件检查**：
- 只检查 `logs/` 下 `clash-*.log.txt`、`proxy-*.db*`、`.DS_Store` 以及根目录 `.DS_Store` 这类文件的存在性和大小。
- 不读取日志或数据库内容；这些文件可能包含节点、连接或访问痕迹。
- 提交前应确认这些运行态文件没有被纳入 Git。

### 客户端真机检查清单

**Shadowrocket**：
- 按目标节点池导入配置文件。
- 做客户端连通性测试。
- 检查 AI 平台（Claude / OpenAI / Gemini / Anthropic / ChatGPT / Grok / Muse 等关键词）是否被精确拦截并导向 `静态住宅` 策略组。
- 检查未被具体服务规则命中的国外通用域名是否进入 `赠送美国` 或 `赠送节点`。
- 检查 `raymond_direct.list` 中的域名是否进入 `DIRECT`。
- 检查 `[Host]` 中的飞书 / Lark、网易系等域名是否使用系统 DNS。
- 检查国内服务和局域网是否直连。
- 检查广告拦截规则是否可用。
- 检查 DNS leak 测试结果。

**Mihomo / Clash Meta for Android**：
- 按目标节点池导入 Android 配置文件。
- 确认“机场悠兔”在通过 `sub.xeton.dev` 转换后能够正常拉取到标准的 Trojan 节点。
- 确认策略组（如 `赠送节点`）在开启 `filter: ".*"` 后节点列表不为空，且可正常发起连通性测试与测速。
- 确认远程 rule providers 拉取成功。
- 启用 TUN 后检查系统流量是否被接管。
- 检查 fake-ip DNS 是否正常解析。
- 检查 `RaymondDirect` 命中的域名是否使用 `dhcp://system`。
- 检查 sniffer 对纯 IP 流量和目标域名还原是否符合预期。
- 检查 AI 关键字请求是否精准走 `静态住宅`。
- 检查 `Global` 和 `MATCH` 命中的国外兜底流量是否进入对应赠送策略组。

## Next steps

- 持续追踪用户反馈的订阅刷新与分流行为。
- 如果用户确认，可以增加只读静态检查脚本，用于验证策略组引用和敏感字段泄露风险。
