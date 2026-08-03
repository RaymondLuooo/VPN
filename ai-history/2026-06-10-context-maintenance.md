# 2026-06-10 - Context Maintenance

Metadata:

- Agent: Codex
- Created at: 2026-06-10
- Context version: 0.1.2
- Project version: TBD

## Summary

本轮对 `/Users/Shared/Shared_AI_Workspace/SharedProjects/VPN` 做第二次增量上下文维护。上一轮文档锚点为 `45deae4 Enable Mihomo sniffer for pure IP traffic`，本轮更新到 `ca06385 Apple 和 iCloud 加入自定义直连规则`。

## Goal

把 2026-06-08 之后的配置演进写入长期上下文：

- `raymond_direct.list` 已成为 Shadowrocket 和 Mihomo 共享的用户自维护直连规则源。
- Mihomo `isp_local` 已通过 `dns.nameserver-policy` 引用 `rule-set:RaymondDirect`，使该规则集命中的域名使用系统 DNS。
- Shadowrocket `local_group.conf` 通过 `[Host]` 手动维护部分自定义直连域名的 `server:system`。
- 飞书 / Lark、网易系、Apple / iCloud 已进入当前直连设计。
- Codex 代推 VPN 配置到 GitHub 曾被安全层拒绝，后续发布通常需要用户在本机终端执行。

## Key discussion

用户指定 `r-project-context-maintainer` 维护项目资料。本轮按增量迭代模式执行，重点校正旧文档中“3 个配置文件”和 `45deae4` 锚点已经过期的问题。

## Files affected

新增：

- `ai-history/2026-06-10-context-maintenance.md`

更新：

- `AGENTS.md`
- `README.md`
- `CHANGELOG.md`
- `docs/PROJECT_CONTEXT.md`
- `docs/TESTING.md`
- `docs/HANDOFF.md`

未修改：

- `local_group.conf`
- `lazy_group_防DNS泄露去广告后的备份.conf`
- `isp_local`
- `raymond_direct.list`

## Bugs encountered

未发现可确认的业务代码 bug。项目当前没有脚本或自动化测试代码。

## Decisions made

- 将 `ca06385 Apple 和 iCloud 加入自定义直连规则` 作为本轮上下文锚点。
- 将 `RaymondDirect` 记录为同时影响直连路由和 Mihomo 系统 DNS 的重要规则源。
- 将 Shadowrocket `[Host]` 不能直接引用远程 rule-set 记录为后续维护约束。
- 继续把 `isp_local` 中的代理订阅入口视为敏感信息，不写入文档。

## Output

输出为增量长期上下文文档和 Codex 历史记录。

## Follow-up

- 待用户确认是否提交当前未跟踪文档。
- 待真机验证 Shadowrocket 和 Mihomo 导入结果。
- 待验证 `RaymondDirect` 的真实规则命中和 DNS policy 命中日志。
- 待确认 Apple / iCloud 是否需要在 Shadowrocket `[Host]` 中同步系统 DNS。

## Raw archive

This file is a placeholder. Detailed raw conversation has not been imported.
