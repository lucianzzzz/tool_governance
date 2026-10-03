# 工具治理框架（Tool Governance Framework）

> 「AI Agent 全栈工程师训练营」第二章实战作业：为工具治理框架接入「转账（transfer）」工具，跑通完整链路。

## 项目结构

| 文件 | 说明 |
|---|---|
| `tool_governance.py` | 治理框架主体（练习版，含 6 个待补 TODO 任务，见下） |
| `test_tool_governance.py` | 转账工具 5 条链路的验收测试 |

## 框架能力

治理框架本身已完整可运行，覆盖整条调用链路：

| 能力 | 说明 |
|---|---|
| Pydantic 校验 | 工具参数严格校验（StrictArgs，`extra="forbid"` 是防注入的最后屏障） |
| 权限状态机 | `PermissionEngine.decide` 统一决断，优先级顺序固定不可改 |
| 一次性审批 | 审批与参数绑定，一次通过仅对当前调用生效 |
| 超时恢复 | `asyncio.timeout` 掐断慢调用，余额等状态不残留 |
| 结果脱敏 | 敏感字段（如账号）在审计日志中脱敏 |
| 审计追踪 | 所有调用留痕，可审计回溯 |

## TODO 任务

在 `tool_governance.py` 中搜索 `TODO(任务` 定位全部待补位置：

1. 新建 `ACCOUNTS` 模拟账户数据
2. 定义 `TransferArgs` 参数模型
3. 实现 `transfer_precheck` 业务预检
4. 实现 `transfer_handler` 转账处理
5. 在 `build_tools()` 里注册 transfer 工具
6. 在 `_redact` 里追加账号脱敏

## 运行测试

**环境要求：Python 3.11+**（代码用到 `StrEnum`，3.9 / 3.10 无法运行）。

测试代码引用 `tool_governance_demo` 模块（本仓库只含练习版 `tool_governance.py`），运行前先复制一份：

```bash
python -m pytest test_tool_governance.py -v
```

5 个测试全部通过即验收合格：

- 注入参数与非法金额被拒
- 预检拦截超限与余额不足
- 无权限 / 白名单外 / plan 模式下拒绝
- 审批绑定参数 + 账号脱敏生效
- 超时按未知状态上报且余额不动

## 作业守则（不能改动的地方）

1. `PermissionEngine.decide` 一行都不要动，优先级顺序是固定框架。
2. 测试调用必须走 `ToolRuntime.invoke`，不直接调 handler。
3. 不要删除 `TransferArgs` 的 `extra="forbid"`。