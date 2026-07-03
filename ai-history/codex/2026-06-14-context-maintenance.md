# 2026-06-14 - Context Maintenance

元信息：
- Agent：Codex
- 创建时间：2026-06-14
- 上下文版本：0.1.4
- 项目版本：TBD
- 资料来源：CHANGELOG、git log、git show、当前项目文件、当前对话

## 摘要

本轮对 `/Users/Shared/Shared_AI_Workspace/SharedProjects/VPN` 做增量上下文维护。上一轮文档锚点为 `41ee9d7 增加apple一个API 为直连， 增加代码注释`，本轮更新到 `5022a34 把悠兔做为赠送美国的备用节点，并且把非美国的节点做成兜底节点`。

本轮没有修改 VPN 配置逻辑，只更新长期上下文文档和 Codex 历史记录。

## 上次项目资料维护后的新增变化

### 变化 1：`赠送美国` fallback 策略结构

#### 目的

让 YouTube、Google Photos、流媒体、社交、游戏平台、Microsoft 等高流量服务继续走成本较低的非静态节点池，同时在美国非静态节点不可用时保留非美兜底。

#### 做出的决定

- Shadowrocket 和 Mihomo 都继续让业务规则引用总组 `赠送美国`。
- 总组内部拆成 `赠送美国主选` 和 `赠送非美兜底`。
- `静态住宅` 继续用于 AI、Google、GitHub 等账号或风控敏感服务，不纳入高流量兜底池。

#### 发现的问题

旧文档仍把 `赠送美国` 描述成较单一的高流量策略组，没有记录主选与兜底结构，也保留了 `main` 相对 `origin/main` ahead 1 的过期状态。

#### 解决方案

更新 `README.md`、`docs/PROJECT_CONTEXT.md`、`docs/TESTING.md` 和 `docs/HANDOFF.md`，把 `5022a34` 的策略结构写入长期上下文，并把真实 fallback 行为标注为 `待验证`。

#### 涉及文件

- `local_group.conf`
- `isp_local`
- `README.md`
- `CHANGELOG.md`
- `docs/PROJECT_CONTEXT.md`
- `docs/TESTING.md`
- `docs/HANDOFF.md`

#### 验证情况

- 已确认：`git show --numstat 5022a34` 显示该提交修改 `isp_local` 和 `local_group.conf`。
- 已确认：`local_group.conf` 中存在 `赠送美国主选`、`赠送非美兜底` 和 fallback 总组 `赠送美国`。
- 已确认：`isp_local` 中存在对应的 `proxy-groups` 结构。
- 已确认：`git rev-list --left-right --count origin/main...HEAD` 输出 `0 0`，当前分支与 `origin/main` 同步。

#### 未验证 / 风险

- 未运行 Shadowrocket 或 Mihomo 客户端导入验证。
- 未验证美国主选全部不可用时是否能自动切到非美兜底。
- 未验证高流量服务真实规则命中日志。

### 变化 2：本地运行态文件边界

#### 目的

把本地未跟踪的 Clash 日志和 SQLite 运行态文件纳入数据安全边界，避免未来 agent 误读、误输出或误提交。

#### 做出的决定

- 只记录这些文件存在和大小。
- 不读取日志或数据库内容。
- 在文档中建议后续提交前明确排除或脱敏处理。

#### 发现的问题

`git ls-files --others --exclude-standard` 显示长期上下文文档之外，还存在 `clash-1780982420336.log.txt` 和 `proxy-2026-06-10-155756.db*` 运行态文件。

#### 解决方案

更新 `AGENTS.md`、`README.md`、`docs/PROJECT_CONTEXT.md`、`docs/TESTING.md` 和 `docs/HANDOFF.md`，明确这些文件不是项目配置源，默认不读取内容，不直接提交。

#### 涉及文件

- `AGENTS.md`
- `README.md`
- `docs/PROJECT_CONTEXT.md`
- `docs/TESTING.md`
- `docs/HANDOFF.md`

#### 验证情况

- 已确认：`ls -lh` 只查看文件大小，未读取文件内容。
- 已确认：这些运行态文件当前仍为未跟踪文件。

#### 未验证 / 风险

- 未确认这些文件是否仍被用户需要。
- 未创建 `.gitignore`，因为本轮目标是维护项目资料，不主动改变仓库忽略规则。

## 跨会话补充

本轮没有导入完整原始对话。内容根据现有 CHANGELOG、git 提交、当前项目文件和当前对话重建。

## 遇到的问题

- 未发现可确认的配置逻辑 bug。
- 项目当前没有业务代码脚本，因此本轮没有触发脚本注释维护流程，也没有读取 `r-coding-guidelines`。

## 输出结果

- 更新长期上下文文档。
- 新增 `ai-history/codex/2026-06-14-context-maintenance.md`。

## 后续事项

- 真机验证 Shadowrocket 与 Mihomo 的 `赠送美国` fallback 行为。
- 用户确认后，可新增 `.gitignore` 规则排除 Clash 日志和 SQLite 运行态文件。
- 用户确认后，可决定是否提交长期上下文文档。

## 原始归档

本文件未导入完整原始对话，内容根据 CHANGELOG、git diff、当前项目文件和当前对话重建。
