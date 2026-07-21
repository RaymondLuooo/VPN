# Testing

最近更新时间：2026-07-20 23:22:00 Asia/Shanghai

## Test philosophy

本项目的主要风险不是代码编译失败，而是配置语义偏差、客户端不兼容、规则引用失效、DNS 泄露和敏感订阅外泄。

验证应分为静态检查和客户端真机检查。没有真机检查时，结论只能标注为 `待验证`。

## Environment checks

- 确认目标客户端：Shadowrocket 或 Mihomo / Clash Meta for Android。
- 确认目标文件：`r_equ_onlyUS_mac`、`r_equ_all_countries_mac`、`r_equ_onlyUS_android`、`r_equ_all_countries_android`、`r_equ_all_static_mac` / `android`、`r_equ_all_channel_countries_mac` / `android`、备份配置或 `raymond_direct.list`。
- 确认没有把订阅 URL、token、节点凭据复制到聊天、文档或公开仓库说明。

## Static checks

Shadowrocket 配置：

- 检查 `[General]`、`[Proxy Group]`、`[Rule]`、`[Host]`、`[URL Rewrite]`、`[MITM]` 节是否存在。
- 检查规则里引用的策略组名称在 `[Proxy Group]` 中存在。
- 对 `r_equ_onlyUS_mac`，检查 `赠送美国` 是否仍引用 `赠送美国主选` 和 `赠送非美兜底`，且两个子组都排除静态住宅节点。
- 对 `r_equ_all_countries_mac`，检查 `赠送节点` 是否仍筛选指定国家和地区的非静态赠送节点。
- 检查 `Global` 规则和 `FINAL` 最终兜底是否仍指向目标配置对应的赠送策略组。
- 检查最终规则是否仍有兜底策略。
- 检查 DNS 相关字段是否被意外改成系统直连。
- 检查 MITM 是否仍处于用户预期范围。
- 修改 `raymond_direct.list` 后，检查两份 `r_equ_*_mac` 是否仍在原顺序位置引用远程规则集。
- 若新增域名需要系统 DNS，检查 `[Host]` 是否同时覆盖裸域和通配子域，例如 `example.com` 与 `*.example.com`。

Mihomo 配置：

- 检查 YAML 顶层键：`sniffer`、`tun`、`dns`、`proxy-providers`、`proxy-groups`、`rule-providers`、`rules`。
- 检查 YouTube App / 媒体域名和 Google Photos 域名是否仍在宽泛 Google 规则前。
- 检查 `proxy-providers` 中订阅入口没有被输出或提交到公开文档。
- 检查 `rules` 引用的策略组名称存在于 `proxy-groups`。
- 对 `r_equ_onlyUS_android`，检查 `赠送美国` fallback 组是否仍只引用 `赠送美国主选` 和 `赠送非美兜底`，并确认两个子组的 filter / exclude-filter 没有把静态住宅节点纳入高流量池。
- 对 `r_equ_all_countries_android`，检查 `赠送节点` 是否仍筛选指定国家和地区的非静态节点。
- 检查 `Global` 规则和 `MATCH` 最终兜底是否仍指向目标配置对应的赠送策略组。
- 检查 provider 更新出口；不要输出任何 provider URL。
- 检查 `rule-providers` 名称与 `rules` 中的 `RULE-SET` 引用一致。
- 检查 `RaymondDirect` provider 是否仍被 `rules` 和 `dns.nameserver-policy` 同时引用。
- 修改 `raymond_direct.list` 时，检查新增规则是否为域名类规则；非域名类规则不适合作为 DNS policy 匹配依据。
- 检查 `MATCH` 兜底仍存在。

`raymond_direct.list`:

- 检查规则语法只使用目标客户端兼容的规则类型。
- 检查宽后缀规则不会意外覆盖敏感海外服务。
- 检查新增域名是否同时适合直连和系统 DNS。
- 检查 Apple / iCloud / CloudKit、飞书 / Lark、网易系等分组注释是否仍与实际规则一致。

本地运行态文件：

- 只检查 `logs/` 下 `clash-*.log.txt`、`proxy-*.db*`、`.DS_Store` 以及根目录 `.DS_Store` 这类文件的存在性和大小。
- 不读取日志或数据库内容；这些文件可能包含节点、连接或访问痕迹。
- 提交前应确认这些运行态文件没有被纳入 Git。

## Unit tests

当前没有自动化单元测试。

## Manual test checklist

Shadowrocket：

- 按目标节点池导入 `r_equ_onlyUS_mac` 或 `r_equ_all_countries_mac`。
- 做客户端连通性测试。
- 检查 AI、Google、GitHub 是否进入预期策略组。
- 检查未被具体服务规则命中的国外通用域名是否进入 `赠送美国`。
- 对全地区配置，检查未被具体服务规则命中的国外通用域名是否进入 `赠送节点`。
- 检查 `raymond_direct.list` 中的域名是否进入 `DIRECT`。
- 检查 `[Host]` 中的飞书 / Lark、网易系等域名是否使用系统 DNS。
- 检查国内服务和局域网是否直连。
- 检查广告拦截规则是否可用。
- 检查 DNS leak 测试结果。

Mihomo / Clash Meta for Android：

- 按目标节点池导入 `r_equ_onlyUS_android` 或 `r_equ_all_countries_android`。
- 确认订阅节点拉取成功。
- 确认远程 rule providers 拉取成功。
- 启用 TUN 后检查系统流量是否被接管。
- 检查 fake-ip DNS 是否正常解析。
- 检查 `RaymondDirect` 命中的域名是否使用 `dhcp://system`。
- 检查 sniffer 对纯 IP 流量和目标域名还原是否符合预期。
- 检查 `Global` 和 `MATCH` 命中的国外兜底流量是否进入 `赠送美国`。
- 对全地区配置，检查 `Global` 和 `MATCH` 命中的国外兜底流量是否进入 `赠送节点`。
- 检查 `机场悠兔` provider 通过 `PROXY` 刷新订阅是否成功。
- 检查 AI、Google、GitHub、国外媒体、国内服务和兜底规则命中是否符合预期。

## Known test gaps

- 本轮没有客户端真机验证。
- 本轮没有联网检查远程规则地址可用性。
- 本轮没有 DNS leak 实测。
- 本轮没有规则命中日志。
- 本轮没有 sniffer / 纯 IP 流量实测。
- 本轮没有 Apple / iCloud / CloudKit、飞书云文档、网易系的客户端 DNS policy 实测。
- 本轮没有 `赠送美国` 主选全部不可用时的 fallback 切换实测。
- 本轮没有读取 Clash 日志或 SQLite 运行态数据库内容。
- 本轮没有 `Global` / `FINAL` / `MATCH` 改走 `赠送美国` 后的真实命中日志。
- 本轮没有 `赠送节点` 全地区配置的真实命中日志。
- 本轮没有 `机场悠兔` provider 通过 `PROXY` 刷新订阅的客户端实测。
- 本轮没有四份主力配置及新增衍生配置的客户端导入验证。
- 本轮没有验证“静态住宅”作为 rule-providers 更新出口是否成功。
- 本轮没有验证 Mac 上的 `policy-regex-filter` 是否准确区分了静态与非静态节点。

## Regression cases

- 修改 DNS 后，AI / Google 流量不能回落到系统 DNS 直连。
- 修改策略组名称后，规则引用不能悬空。
- 修改广告规则后，REJECT 规则仍应优先。
- 修改订阅入口时，真实 URL 不能进入文档或聊天。
- 修改 Mihomo TUN / fake-ip 时，国内直连和局域网访问不能被破坏。
- 修改 YouTube、Google Photos 或 Google 宽规则时，必须确认前置规则没有被宽泛 Google 规则覆盖。
- 修改 `赠送美国`、`赠送美国主选` 或 `赠送非美兜底` 时，必须确认高流量服务仍走成本较低的非静态节点池，AI / Google / GitHub 仍走 `静态住宅`。
- 修改 `Global`、`FINAL` 或 `MATCH` 兜底时，必须确认国外通用流量没有误伤国内直连、`RaymondDirect` 或账号敏感服务。
- 修改 `赠送节点` 时，必须确认全地区节点筛选不会误纳入静态住宅或不应承担高流量的节点。
- 修改 provider 更新出口时，必须确认订阅刷新可用，同时不能把真实订阅 URL 写入文档、日志或提交说明。
- 修改 Mihomo sniffer 时，必须确认纯 IP 流量、DNS mapping 和目标地址覆盖行为符合客户端预期。
- 修改 `raymond_direct.list` 时，必须确认对应服务既适合直连，也适合在 Mihomo 中走系统 DNS。
- 修改 Shadowrocket `[Host]` 时，必须确认裸域和通配子域是否都需要覆盖。
