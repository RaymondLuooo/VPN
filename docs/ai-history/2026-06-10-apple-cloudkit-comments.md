# 2026-06-10 - Apple CloudKit Direct Rule And Config Comments

Metadata:
- Agent: Codex
- Created at: 2026-06-10
- Context version: 0.1.3
- Project version: TBD

## Summary

本轮完成一次本地 Git 提交，并按项目长期上下文维护流程记录变更。提交 `41ee9d7` 将 `apple-cloudkit.com` 加入 `raymond_direct.list`，并为 `isp_local` 与 `local_group.conf` 增加解释性注释。

## Goal

- 将 Apple CloudKit 相关 API 域名加入用户自维护直连规则。
- 解释两份客户端配置中 DNS、策略组、规则集和规则顺序的意图。
- 维护 README、CHANGELOG、PROJECT_CONTEXT、TESTING 和 HANDOFF，让下一个 agent 能接上当前状态。

## Key Discussion

- DNS 的含义被明确为“域名到 IP 地址的查询系统”。
- `isp_local` 中系统 DNS 的逻辑由 `dns.nameserver-policy` 控制，当前 `rule-set:RaymondDirect` 使用 `dhcp://system`。
- `local_group.conf` 中系统 DNS 的逻辑由 `[Host]` 的 `server:system` 条目控制，不能自动从远程 `raymond_direct.list` 映射。
- Apple / iCloud / CloudKit 当前只确认进入直连规则，Shadowrocket `[Host]` 是否同步系统 DNS仍需真机验证。

## Files Affected

- `raymond_direct.list`
- `isp_local`
- `local_group.conf`
- `README.md`
- `CHANGELOG.md`
- `docs/PROJECT_CONTEXT.md`
- `docs/TESTING.md`
- `docs/HANDOFF.md`
- `ai-history/2026-06-10-apple-cloudkit-comments.md`

## Bugs Encountered

- 沙箱禁止直接写 `.git/index`，`git add` 和 `git commit` 需要提升权限后执行。
- 未发现可确认的配置逻辑 bug。

## Decisions Made

- 只把 `apple-cloudkit.com` 加入 `raymond_direct.list` 直连，不同步到 Shadowrocket `[Host]`。
- `isp_local` 和 `local_group.conf` 只补充注释，不改变 DNS 字段、规则顺序、策略组名称或订阅入口。
- 不代推 GitHub，只向用户提供 push 命令。

## Output

- 本地提交：`41ee9d7 增加apple一个API 为直连， 增加代码注释`
- 推送命令：`git push origin main`

## Follow-up

- 用户可在本机终端执行 `git push origin main`。
- 真实客户端导入后，验证 Apple CloudKit 是否命中直连、Mihomo 是否经 `RaymondDirect` 使用系统 DNS、Shadowrocket 是否需要额外 `[Host]` 系统 DNS 条目。
- 长期上下文文档当前仍未被 Git 跟踪提交，后续如需纳入仓库需要单独确认。

## Raw archive

This file is a placeholder. Detailed raw conversation has not been imported.
