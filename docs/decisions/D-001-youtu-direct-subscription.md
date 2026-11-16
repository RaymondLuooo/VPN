# D-001 悠兔 Android 订阅直连

- 状态：Active
- Scope / 匹配线索：`r_equ_onlyUS_android`、`r_equ_all_channel_countries_android`、`机场悠兔`、Mihomo、订阅兼容性
- 读取触发：修改悠兔 Android provider、排查节点加载失败或评估订阅转换服务时
- Supersedes：无；本决定替代此前仅记录在架构和历史资料中的转换服务方案。

## 决定

两份包含“机场悠兔” provider 的 Android Mihomo 配置直接使用上游原始订阅入口，不再经 `sub.xeton.dev` 转换。真实订阅地址属于敏感信息，仅保留在配置中，不写入本文档。

## 原因与边界

- `user_decision`：2026-11-16，用户明确确认直接使用新提供的原始订阅链接。
- `current_fact`：`r_equ_onlyUS_android` 与 `r_equ_all_channel_countries_android` 已更新为直接上游入口；另外两份 Android 配置不含该 provider。
- `historical_process`：此前通过转换服务规避过 `anytls` 协议兼容问题；该方案不再是当前配置路径。
- `validation_result`：已完成 YAML 解析、provider 类型与规则组引用静态检查；尚未在 Android 客户端实际拉取和连接测试。

直接入口下发的节点协议必须被目标 Mihomo / Clash Meta 客户端支持。若出现 0 节点、协议不支持或拉取失败，不应静默改动策略组或规则；应先确认上游响应，并重新评估恢复转换或采用其他兼容方案。

## 替代方案

- 保留 `sub.xeton.dev` 转换：能缓解已知私有协议兼容问题，但增加第三方依赖；本次未采用。
- 自建订阅转换服务：可减少公共服务依赖，但属于新基础设施，未获本次实施授权。

## Validation

在 Android 的 Mihomo / Clash Meta 兼容客户端导入两份目标配置，确认悠兔 provider 能拉取节点、节点数不为 0，并完成一次连通性或测速验证。

## revisit_when

当订阅刷新失败、客户端报告不支持的协议、节点列表为空，或用户决定恢复/替换转换链路时重新评估。

## Lessons

订阅入口变更不会自动证明协议兼容；静态 YAML 检查只能确认配置结构，不能替代目标客户端真机拉取验证。
