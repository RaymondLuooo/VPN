# 项目架构

项目名称：VPN

设计状态：已确认、长期维护中。当前已包含 8 份矩阵式分流配置文件（Mac Shadowrocket 与 Android Mihomo 各 4 份）及自维护直连规则集，持续进行规则调优与客户端兼容性维护。

本文件只描述项目当前要实现什么以及系统当前如何组织。Agent 工作方法、授权边界、知识路径和读取规则见根目录 `AGENTS.md`；具体方案取舍见 `docs/decisions/`；可复用经验见按需创建的 `docs/lessons/`；项目术语、领域模型、数据源和字段语义按需维护在 Knowledge Map 指定文档；文档清单见 `docs/INDEX.md`。不要在本文件复制这些内容。

## 项目目的与 Scope

### 核心诉求

让用户在不同客户端（Shadowrocket / Mac 与 Mihomo / Android）上获得稳定、可解释、低成本且防泄露的代理分流行为：
- **敏感服务高稳防护**：AI（OpenAI/ChatGPT、Anthropic/Claude、Google Gemini 等）、Google、GitHub 等账号与风控敏感服务精准导向高权重的 `静态住宅` 策略组，通过 `DOMAIN-KEYWORD` 优先捕获。
- **高流量与兜底成本控制**：国外高流量服务（YouTube、Netflix、Disney、HBO、Spotify、TikTok、游戏平台等）和国外未命中兜底流量走低成本节点池（`赠送美国` 或 `赠送节点`），避免高带宽消耗挤占静态住宅或手工 `PROXY` 资源。
- **直连与 CDN 优化**：国内局域网、银行及常用中国服务尽量直连；Apple / iCloud / CloudKit、飞书 / Lark、网易系等明确要求本地化的域名使用自维护规则集 `raymond_direct.list`，并通过系统 DNS 保障国内 CDN 与系统服务稳定性。
- **内核与协议兼容兜底**：解决 Android Mihomo 底层对于单独使用 `exclude-filter` 导致的解构节点列表为空 bug（补充 `filter: ".*"`）；通过 `sub.xeton.dev` 云端清洗“机场悠兔”私有协议 `type: anytls` 为标准 `type: trojan`；优化 `url-test`（`tolerance: 50`, `lazy: true`）防止频繁跳节点与后台耗电。
- **安全与防泄露**：广告与跟踪规则优先 REJECT；国外流量 DNS 尽量经代理或加密 DoH 解析，降低 DNS 泄露风险。

### 当前范围与范围之外

- 当前范围：
  1. Shadowrocket / Mac 平台 4 份主配置（`r_equ_onlyUS_mac`, `r_equ_all_countries_mac`, `r_equ_all_static_mac`, `r_equ_all_channel_countries_mac`）与 1 份备份配置（`lazy_group_防DNS泄露去广告后的备份.conf`）。
  2. Clash Meta / Mihomo Android 平台 4 份主配置（`r_equ_onlyUS_android`, `r_equ_all_countries_android`, `r_equ_all_static_android`, `r_equ_all_channel_countries_android`）。
  3. 用户自维护共享直连规则集 `raymond_direct.list`。
- 范围之外：
  1. 本仓库不是通用 VPN 客户端商业软件开发，不提供安装包编译。
  2. 不负责自建/运维底层代理服务器节点，仅维护客户端的分流路由、策略组与 DNS 配置。
  3. 不在公共代码或文档中持久化任何真实订阅 URL、节点密码、Token 或私钥凭据。

### 完成标准

1. 8 份配置文件能够正常被对应客户端导入与解析，无语法报错。
2. AI 工具相关域名（Claude, ChatGPT, Gemini, OpenAI, Anthropic）被精准分流至 `静态住宅`。
3. 悠兔等第三方机场订阅节点解析正常，无 0 节点丢弃现象。
4. 国外高流量与未匹配国外流量正确进入 `赠送美国` 或 `赠送节点`。
5. 直连域名解析快速，无国外 DNS 污染亦无国内服务异常。
6. 测速策略平稳，无短时间内的频繁节点横跳断流。

## 简短架构概览

本项目采用 **双平台 × 四象限节点策略** 的分流配置矩阵：
- **双平台适配**：针对 iOS/macOS 的 Shadowrocket 配置语法（`[General]`、`[Proxy Group]`、`[Rule]`、`[Host]` 等）与 Android 的 Clash Meta / Mihomo YAML 语法（`sniffer`、`tun`、`dns`、`proxy-providers`、`proxy-groups`、`rules`）。
- **四类节点矩阵**：
  1. `onlyUS`（仅美国优先）：国外高流量优先美国赠送节点，不可用时 fallback 到非美赠送节点。
  2. `all_countries`（全地区赠送）：国外流量按指定多国（美、日、台、新、韩等）赠送节点自动 url-test 测速选择。
  3. `all_channel_countries`（全渠道全地区）：覆盖多订阅渠道的全地区赠送分流。
  4. `all_static`（全静态住宅）：所有代理流量全部指向静态住宅节点。
- **直连与分流双枢纽**：`raymond_direct.list` 作为共同引用的直连规则集，在 Shadowrocket 中作为远程 RULE-SET 并在 `[Host]` 映射系统 DNS；在 Mihomo 中作为 rule-provider 并与 `dns.nameserver-policy` 联动直连和系统解析。

## 目录结构与职责

```text
/Users/Shared/Shared_AI_Workspace/SharedProjects/VPN/
├── AGENTS.md                  # Agent 共享工作规范、Knowledge Map、权限与阅读规则
├── CHANGELOG.md               # 阶段性变更索引与 Git 锚点（由 r-project-context-maintainer 独占维护）
├── README.md                  # 面向使用者的快速概览与工作流说明
├── docs/                      # 当前知识与测试文档
│   ├── architecture.md        # 当前核心诉求、Scope、架构分流矩阵与模块职责
│   ├── INDEX.md               # 现行知识索引（稳定 ID、匹配线索、读取触发）
│   ├── TESTING.md             # 静态检查、真机测试与回归验证清单
│   ├── HANDOFF.md             # 跨 Agent 交接记录、未完成工作与断点恢复
│   ├── decisions/             # 长期架构决策归档落点（按需创建记录）
│   └── ai-history/            # 选择性历史演进与事故证据链
│       ├── INDEX.md           # 紧凑历史路由索引
│       └── *.md               # 跨 Agent 历史会话归纳与转折记录
├── r_equ_*_mac                # Shadowrocket / Mac 4 份矩阵配置文件
├── r_equ_*_android            # Clash Meta / Mihomo Android 4 份矩阵配置文件
├── raymond_direct.list        # 用户自维护直连规则集（多端共用）
├── lazy_group_*.conf          # Shadowrocket 备份配置
├── logs/                      # 本地运行态日志与临时数据（严禁读取内容，不提交 Git）
└── .context-maintenance/      # 上下文维护批次凭证目录
    └── receipts/              # 维护摘要、发现收据与完成标记
```

| 目录 / 文件 | 存放内容 | 目的与边界 |
|---|---|---|
| `r_equ_*_mac` | Mac / Shadowrocket 分流配置文件 | 负责 Mac 端规则、DNS DoH、Host 映射与策略组；不包含私有密钥 |
| `r_equ_*_android` | Android / Mihomo YAML 配置文件 | 负责 Android 端 sniffer、TUN、fake-ip、provider 与策略组；包含订阅入口（按敏感信息处理） |
| `raymond_direct.list` | 用户自定义直连域名与规则清单 | 纯域名/规则列表，供 Mac 和 Android 共同引用直连及系统 DNS 解析 |
| `docs/` | 现行架构、索引、测试与交接文档 | 存放当前有效的事实、契约与验证边界；不存放历史会话记录 |
| `docs/decisions/` | 跨文件的重大长期架构决策 | 存放存在真实取舍的架构决策（由 `r-project-knowledge-maintainer` 维护） |
| `docs/ai-history/` | 选择性历史演进与会话复盘归纳 | 记录目标演变、方案推翻、复杂事故证据链；不是当前规则的权威来源 |
| `logs/` | 客户端生成的本地日志与临时 SQLite 数据库 | 仅作本地调试参考，严禁提交 Git，严禁 Agent 读取内容 |
| `.context-maintenance/` | 上下文维护机器凭证与临时收据 | 供 maintenance-pipeline 及其子技能读取和断点恢复，日常任务不扫描 |

## 模块职责与关键接口

### 1. 策略组（Proxy Groups）职责
- `静态住宅`：专供 AI 服务（Anthropic/Claude, OpenAI/ChatGPT, Google Gemini）、Google、GitHub 等风控严苛平台，保障账号稳定。
- `赠送美国主选`：筛选名称包含美国且排除静态住宅的非静态赠送节点。
- `赠送非美兜底`：筛选日本、台湾、新加坡等非静态赠送节点，作为美国节点不可用时的备用出口。
- `赠送美国`：将“赠送美国主选”和“赠送非美兜底”封装为 `fallback` 策略组，专供国外高流量服务（YouTube, Netflix, Telegram 等）。
- `赠送节点`：全地区及全渠道配置使用的高流量与通用兜底策略组，采用 `url-test` 在美、日、台、新、韩等非静态节点间自动优选。
- `PROXY`：手工节点选择入口，供用户临时调试或强制指定节点；不再承担默认的国外全局兜底角色。

### 2. 外部服务与接口交互
- `sub.xeton.dev`（订阅转换接口）：
  - 职责：将“机场悠兔”订阅中下发的 `type: anytls` 私有协议清洗为 Clash Meta 标准内核支持的 `type: trojan`。
  - 风险与应对：属于第三方公共服务，存在波动风险；若出现长期不可用，需考虑自建 Subconverter。
- `raw.githubusercontent.com`：
  - 职责：作为 `raymond_direct.list` 远程规则集与配置文件的云端拉取端点。

### 3. DNS 分流机制
- **Shadowrocket**：通过 `[General]` 配置 DoH 与 hijack-dns；通过 `[Host]` 配置特定域名的 `server:system`，使 Apple、飞书、网易等服务使用本地 DNS 解析。
- **Mihomo**：开启 TUN 与 fake-ip DNS；通过 `nameserver-policy` 绑定 `rule-set:RaymondDirect` 至 `dhcp://system`，确保直连流量与 CDN 节点正确对应。

## 当前架构约束与关键技术选择

1. **AI 流量关键词强制直锁静态住宅**：在全部 8 份配置文件中使用 `DOMAIN-KEYWORD`（`anthropic`、`claude`、`openai`、`gemini`、`chatgpt`）排在宽泛规则之前，优先锁死进入 `静态住宅`。
2. **国外通用兜底切离手工 PROXY**：未命中规则的国外通用流量（`FINAL` / `MATCH`）统一收口至 `赠送美国` 或 `赠送节点`，避免意外流量击穿高价值节点池。
3. **Mihomo 排除过滤器节点丢失修复**：在策略组仅定义 `exclude-filter` 时，必须显式补全 `filter: ".*"`，规避 Mihomo 内核在缺少正向匹配时解析节点列表为空的底层 Bug。
4. **测速策略防抖与省电优化**：所有 Android 配置的 `url-test` 策略组强制配置 `tolerance: 50`（防节点频繁切流断线）与 `lazy: true`（减少后台常驻耗电与测速风暴）。
5. **本地运行态数据严格隔离**：`logs/` 下日志与数据库可能残留节点 IP 与访问痕迹，建立物理隔离护栏，严禁 Agent 读取或提交。

## 待明确事项

1. `sub.xeton.dev` 公共转换服务的长期可用性与隐私考量；未来是否需要自建轻量级 Subconverter Docker 服务。
2. 是否需要为仓库补充脱敏示例配置文件模板（例如去除敏感订阅 URL）及轻量级语法自动化检查脚本。

## 设计变更

变更审批、文档写入权限和读取规则见根目录 `AGENTS.md`。本文件在用户确认相关变化后只更新受影响部分；已批准、待实施的设计与已完成实现保持区分。局部实现 Decision、专属 Lesson 和历史演变不在这里展开。
