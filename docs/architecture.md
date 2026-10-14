# 项目架构

项目名称：VPN

设计状态：已确认、长期维护中。当前已包含 8 份矩阵式分流配置文件（Mac Shadowrocket 与 Android Mihomo 各 4 份）及自维护直连规则集，持续进行规则调优与客户端兼容性维护。

本文件是系统的当前真相（Current Truth，L1）。本文件只描述项目当前要实现什么以及系统当前如何组织。Agent 工作方法、授权边界、知识路径和读取规则见根目录 `AGENTS.md`（L0）；具体方案取舍见 `docs/decisions/`（L2，内嵌 Lessons 与 Validation）；业务主题域知识见按需创建的 `docs/domains/<topic>.md`；项目术语、领域模型、数据源和字段语义按需维护在 Knowledge Map 指定文档；文档清单见 `docs/index.md`。不要在本文件复制这些内容。

## 项目目的与 Scope

### 核心诉求

目标树最多两层：`G1` 是一项长期目标，`G1.1` 是它的子目标。编号是稳定 ID，只在文档和会话中引用，改写措辞或调整顺序都不换号，废弃后保留编号并标为“已废弃”，新目标取未用过的下一个号。子目标按需写以下字段；更细的小要求写成该子目标的验收要点，细则很多时移入 `docs/domains/<topic>.md` 并在此链接。

- 要求：用户能观察到的结果。
- 不可接受：用户明确不能接受的结果。
- 验收要点：判断是否做到的具体检查。
- 涉及范围：受这条目标约束的目录或模块；修 bug、review 或修改这些位置时必须对照本条。
- 状态：待实施 / 已实现 / 已验证 / 已废弃。

G1 让用户在不同客户端（Shadowrocket / Mac 与 Mihomo / Android）上获得稳定、可解释、低成本且防泄露的代理分流行为

- **G1.1 敏感服务高稳防护**
  - 要求：AI（OpenAI/ChatGPT、Anthropic/Claude、Google Gemini 等）、Google、GitHub 等账号与风控敏感服务精准导向高权重的 `静态住宅` 策略组。
  - 不可接受：敏感账号因频繁变动 IP 或被识别为机房 IP 导致封控。
  - 验收要点：在全部配置文件中使用 `DOMAIN-KEYWORD` 优先捕获并路由至 `静态住宅`。
  - 涉及范围：`r_equ_*_mac`, `r_equ_*_android`。
  - 状态：已实现。

- **G1.2 高流量与兜底成本控制**
  - 要求：国外高流量服务和未命中兜底流量走低成本节点池（`赠送美国` 或 `赠送节点`）。
  - 不可接受：高带宽消耗挤占静态住宅或手工 `PROXY` 资源。
  - 验收要点：未匹配流量（`FINAL` / `MATCH`）统一收口进入对应赠送节点策略组。
  - 涉及范围：`r_equ_*_mac`, `r_equ_*_android`。
  - 状态：已实现。

- **G1.3 直连与 CDN 优化**
  - 要求：国内局域网、银行及常用中国服务尽量直连；明确要求本地化的域名使用自维护规则集并通过系统 DNS 保障国内 CDN 与系统服务稳定性。
  - 不可接受：国内服务解析到国外导致访问缓慢或不可用。
  - 验收要点：Apple、飞书、网易等明确要求本地化的服务使用 `raymond_direct.list`，并在 Shadowrocket `[Host]` 映射 `server:system`，Mihomo 绑定 `dhcp://system`。
  - 涉及范围：`raymond_direct.list`, `r_equ_*_mac`, `r_equ_*_android`。
  - 状态：已实现。

- **G1.4 内核与协议兼容兜底**
  - 要求：规避 Mihomo `exclude-filter` bug、云端清洗第三方私有协议、防频繁跳节点。
  - 不可接受：Android 客户端因底层 bug 获取不到节点列表或因频繁测速导致严重耗电。
  - 验收要点：Mihomo 配置中补充 `filter: ".*"`，机场订阅经 `sub.xeton.dev` 洗掉私有 `type: anytls`，配置 `url-test` 测速防抖参数（`tolerance: 50`, `lazy: true`）。
  - 涉及范围：`r_equ_*_android`。
  - 状态：已实现。

- **G1.5 安全与防泄露**
  - 要求：广告与跟踪规则优先拦截，国外流量 DNS 尽量经代理或加密 DoH 解析。
  - 不可接受：真实 DNS 解析轨迹无意中大面积泄露。
  - 验收要点：优先配置 REJECT 规则，Shadowrocket 配置正确的 DNS DoH 与 hijack-dns。
  - 涉及范围：`r_equ_*_mac`, `r_equ_*_android`。
  - 状态：已实现。

### 当前范围与范围之外

- 当前范围：
  1. Shadowrocket / Mac 平台 4 份主配置（`r_equ_onlyUS_mac`, `r_equ_all_countries_mac`, `r_equ_all_static_mac`, `r_equ_all_channel_countries_mac`）与 1 份备份配置（`lazy_group_防DNS泄露去广告后的备份.conf`）。
  2. Clash Meta / Mihomo Android 平台 4 份主配置（`r_equ_onlyUS_android`, `r_equ_all_countries_android`, `r_equ_all_static_android`, `r_equ_all_channel_countries_android`）。
  3. 用户自维护共享直连规则集 `raymond_direct.list`。
- 范围之外：
  1. 本仓库不是通用 VPN 客户端商业软件开发，不提供安装包编译。
  2. 不负责自建/运维底层代理服务器节点，仅维护客户端的分流路由、策略组与 DNS 配置。
  3. 不在公共代码或文档中持久化任何真实订阅 URL、节点密码、Token 或私钥凭据。

### 约束

1. **AI 流量关键词强制直锁静态住宅**：在全部 8 份配置文件中使用 `DOMAIN-KEYWORD`（`anthropic`、`claude`、`openai`、`gemini`、`chatgpt` 等）排在宽泛规则之前，优先锁死进入 `静态住宅`。
2. **国外通用兜底切离手工 PROXY**：未命中规则的国外通用流量（`FINAL` / `MATCH`）统一收口至 `赠送美国` 或 `赠送节点`，避免意外流量击穿高价值节点池。
3. **本地运行态数据严格隔离**：`logs/` 下日志与数据库可能残留节点 IP 与访问痕迹，建立物理隔离护栏，严禁 Agent 读取或提交。

### 完成标准

1. 8 份配置文件能够正常被对应客户端导入与解析，无语法报错。
2. AI 工具相关域名被精准分流至 `静态住宅`。
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
├── AGENTS.md                  # Agent 共享工作规范、Knowledge Map、权限与阅读规则（L0）
├── CHANGELOG.md               # 阶段性变更索引与 Git 锚点（由 r-p-context-maintainer 独占维护）
├── README.md                  # 面向使用者的快速概览与测试/工作流说明
├── docs/                      # 当前知识与历史证据文档
│   ├── architecture.md        # 当前核心诉求、Scope、架构分流矩阵与模块职责（当前唯一法定真相，L1）
│   ├── index.md               # 现行知识索引（稳定 ID、匹配线索、读取触发，L0）
│   ├── decisions/             # 长期架构决策归档落点（内嵌 Lessons 与 Validation，按需创建，L2）
│   └── ai-history/            # 选择性历史演进与事故证据链（L3）
│       ├── index.md           # 紧凑历史路由索引
│       └── *.md               # 跨 Agent 历史会话归纳与转折记录
├── r_equ_*_mac                # Shadowrocket / Mac 4 份矩阵配置文件
├── r_equ_*_android            # Clash Meta / Mihomo Android 4 份矩阵配置文件
├── raymond_direct.list        # 用户自维护直连规则集（多端共用）
├── lazy_group_*.conf          # Shadowrocket 备份配置
├── equal默认配置文件.yaml.txt  # 上游 Equal 原始订阅参考模板
├── 悠兔默认配置文件.yaml        # 上游悠兔原始订阅参考配置（不进 Git）
├── logs/                      # 本地运行态日志与临时数据（严禁读取内容，不提交 Git）
├── tmp/                       # Agent 临时脚本与试验输出（不进 Git，严禁持久化交接/候选）
└── .context-maintenance/      # 上下文维护批次凭证目录
    └── receipts/              # 维护摘要、发现收据与完成标记
```

| 目录 | 存放内容 | 目的与边界 | 服务目标 |
|---|---|---|---|
| `r_equ_*_mac` | Mac / Shadowrocket 分流配置文件 | 负责 Mac 端规则、DNS DoH、Host 映射与策略组；不包含私有密钥 | G1.1, G1.2, G1.3, G1.5 |
| `r_equ_*_android` | Android / Mihomo YAML 配置文件 | 负责 Android 端 sniffer、TUN、fake-ip、provider 与策略组；包含订阅入口（按敏感信息处理） | G1.1, G1.2, G1.3, G1.4, G1.5 |
| `raymond_direct.list` | 用户自定义直连域名与规则清单 | 纯域名/规则列表，供 Mac 和 Android 共同引用直连及系统 DNS 解析 | G1.3 |
| `lazy_group_*.conf` | Shadowrocket 备份配置 | 归档历史配置副本，供回滚或语法比对参考 | — |
| `equal默认配置文件.yaml.txt` | 上游 Equal 原始订阅参考模板 | 供参考上游节点结构与转换规则的模板 | — |
| `悠兔默认配置文件.yaml` | 上游悠兔原始订阅参考配置 | 供排查本地转换与协议格式，不提交 Git | — |
| `docs/` | 现行架构与知识索引文档 | 存放当前有效的事实、契约与验证边界；不存放运行期交接或候选队列 | — |
| `docs/decisions/` | 跨文件的重大长期架构决策 | 存放存在真实取舍的架构决策（内嵌 Lessons 与 Validation） | — |
| `docs/ai-history/` | 选择性历史演进与会话复盘归纳 | 记录目标演变、方案推翻、复杂事故证据链；不是当前规则的权威来源 | — |
| `logs/` | 客户端生成的本地日志与临时 SQLite 数据库 | 仅作本地调试参考，严禁提交 Git，严禁 Agent 读取内容 | — |
| `tmp/` | Agent 临时脚本与试验输出 | 存放一次性调试脚本与输出，不进 Git，严禁持久化交接或候选队列 | — |
| `.context-maintenance/` | 上下文维护机器凭证与临时收据 | 供 maintenance-pipeline 及其子技能读取和断点恢复，日常任务不扫描 | — |

历史证据统一放在项目根目录下的 `docs/ai-history/`（L3），索引为 `docs/ai-history/index.md`。首次生成合格历史记录时再创建，不在启动时预建空目录或索引；历史证据不替代当前架构或正式知识。

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
- **Shadowrocket**：通过 `[General]` 将默认与备用 DoH 固定经 `静态住宅` 访问，并配置 hijack-dns；通过 `[Host]` 配置特定域名的 `server:system`，使 Apple、飞书、网易等服务使用本地 DNS 解析。
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

用户新诉求、方向和重点顾虑的识别与同步流程见 AGENTS 的“用户诉求、方向与重点顾虑的承接”；长期目标改变或新增时的自动更新规则见 AGENTS 的“文档更新规则”。本文件按这些规则保存目标树、约束、完成标准及必要的待验证风险，标明已确认、待实施或待验证状态，不复制 Agent 工作规则。
