# AGENTS.md

最近更新时间：2026-06-28 01:45:54 Asia/Shanghai

## Project identity

本项目是个人 VPN / 代理分流配置仓库，当前包含 Shadowrocket / Mac 配置和 Clash Meta / Mihomo Android 配置。

当前已确认文件：

- `r_equ_onlyUS_mac`: Shadowrocket / Mac 仅美国优先配置，包含 DNS、防泄露、广告拦截、AI/媒体/国内直连等分流规则。
- `r_equ_all_countries_mac`: Shadowrocket / Mac 全地区赠送节点配置，使用 `赠送节点` 作为高流量与国外兜底策略组。
- `r_equ_onlyUS_android`: Clash Meta for Android / Mihomo 仅美国优先配置，包含 sniffer、TUN、fake-ip DNS、proxy provider、rule providers、proxy groups 和 rules。
- `r_equ_all_countries_android`: Clash Meta for Android / Mihomo 全地区赠送节点配置，使用 `赠送节点` 作为高流量与国外兜底策略组。
- `lazy_group_防DNS泄露去广告后的备份.conf`: Shadowrocket 备份配置，保留更接近通用区域节点分组的规则结构。
- `raymond_direct.list`: 用户自维护的直连规则集，被 Shadowrocket 和 Mihomo 共同引用；在 Mihomo 中同时参与 `nameserver-policy`，影响对应域名的 DNS 解析出口。

Git 当前仍跟踪旧文件名 `local_group.conf` 和 `isp_local`，但 2026-06-28 工作树显示它们已被删除并由 `r_equ_*` 四份配置替代；提交前必须确认这是用户预期的迁移。

## Required reading order

1. `README.md`
2. `docs/PROJECT_CONTEXT.md`
3. `docs/TESTING.md`
4. `docs/HANDOFF.md`
5. `CHANGELOG.md`
6. 目标配置文件本身
7. **历史与踩坑上下文检索**：不用先读 `ai-history/`。在需要排查复杂 Bug、追溯历史决策、理解特定配置的动机或遇到当前文档无法解释的逻辑冲突时，再去阅读 `ai-history/INDEX.md`。

## Allowed work

- 维护项目文档、交接记录和已知风险。
- 检查配置结构、规则分组、注释和明显冲突。
- 在用户明确确认后，修改 Shadowrocket 或 Mihomo 配置。
- 在用户明确确认后，生成新配置或兼容格式转换。

## Forbidden work

- 未经用户确认，不得修改现有 VPN 配置逻辑、节点分组、订阅地址、DNS 策略、规则顺序或 MITM 设置。
- 不得读取、输出、提交或复制代理订阅 URL、token、cookie、密码、证书私钥、节点凭据。
- 不得访问 `/Users/raymond/`、个人 Chrome profile、钥匙串、SSH 私钥、浏览器 cookie / 历史 / 密码数据库。
- 不得执行破坏性命令、强制推送、删除文件、安装依赖或修改系统网络设置。

## Coding rules

本仓库目前没有业务代码脚本。若未来新增脚本，修改代码或脚本注释前必须先读取 `/Users/rai/.agents/skills/r-coding-guidelines/SKILL.md`，并按对应语言 reference 执行。

## Testing rules

- 当前没有自动化测试。
- 配置修改后至少做静态检查：节标题、规则引用、策略组名称、DNS 字段、敏感值是否泄露。
- 修改 `raymond_direct.list` 后必须同时检查 `r_equ_*_mac` 的远程 `RULE-SET` 引用、`r_equ_*_android` 的 `RaymondDirect` provider 引用，以及是否需要同步 Shadowrocket / Mac `[Host]` 中的 `server:system` 条目。
- 修改 `赠送美国`、`赠送美国主选`、`赠送非美兜底` 或 `赠送节点` 后必须同时检查 Shadowrocket / Mac `[Proxy Group]` 和 Mihomo `proxy-groups` 的引用关系，并把真实客户端 fallback / url-test 行为标注为 `待验证`，直到完成真机验证。
- 真机验证必须在目标客户端中完成：Shadowrocket 用 iOS 客户端导入验证，Mihomo 用 Android / Clash Meta 兼容客户端验证。
- 若未做真机验证，必须在回复和文档中标注 `待验证`。

## Data safety rules

- `r_equ_*_android` 包含代理订阅入口或 provider 配置，按敏感信息处理。
- 文档只能写“存在订阅入口”或“需要订阅 URL”，不能写真实 URL 或可还原片段。
- `logs/` 下的 `clash-*.log.txt`、`proxy-*.db*` 和 `.DS_Store` 等客户端日志、SQLite 运行态文件或系统元数据可能包含节点、连接、访问痕迹或本地环境信息；默认只记录存在性和用途，不读取内容，不提交。
- `.env`、证书、私钥、账号凭据如出现，只能记录存在性和用途，不能读取真实值。

## Git rules

- 默认只做本地修改，不自动 commit、push 或发布。
- 发现用户已有改动时，不得回滚；必须先区分本次改动和既有改动。
- 不使用 `git reset --hard`、`git clean`、`git push --force`。
