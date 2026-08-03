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
- `r_equ_all_channel_countries_mac` / `android`: 全渠道全地区衍生配置。
- `r_equ_all_static_mac` / `android`: 全量静态住宅节点配置。
- `lazy_group_防DNS泄露去广告后的备份.conf`: Shadowrocket 备份配置。
- `raymond_direct.list`: GitHub-hosted 自维护直连规则集，被 `r_equ_*_mac` 和 `r_equ_*_android` 引用。
- `logs/`: 本地客户端运行态产物目录，包含 Clash 日志、SQLite 数据库及系统元数据；可能含敏感运行信息，不应读取内容、直接提交或输出内容。
- `docs/PROJECT_CONTEXT.md`: 项目背景、核心概念和数据流。
- `docs/TESTING.md`: 静态检查和真机验证清单。
- `docs/HANDOFF.md`: 给下一个 agent 的交接文档。
- `ai-history/`: 跨 Agent 历史维护记录与索引。

## How to run

通过 raw.githubusercontent.com 或直接导入客户端配置文件：

- Shadowrocket / Mac: 导入对应 `r_equ_*_mac` 文件。
- Mihomo / Clash Meta for Android: 导入对应 `r_equ_*_android` 文件。

## How to test

详见 `docs/TESTING.md`。

## Next steps

- 持续追踪用户反馈的订阅刷新与分流行为。
- 如果用户确认，可以增加只读静态检查脚本，用于验证策略组引用和敏感字段泄露风险。
