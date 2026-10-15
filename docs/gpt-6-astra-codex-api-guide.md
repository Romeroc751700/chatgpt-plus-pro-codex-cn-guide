---
title: "GPT-6 Astra 怎么用？Codex CLI、Responses API 与权限排查"
description: "保留 GPT-6 Astra 历史模型与版本记录，提供按当前账号核对Chat、Codex、API入口的方法、Responses API最小请求、权限额度排查和代码任务模板。"
last_modified_at: 2026-10-16
---

# GPT-6 Astra 怎么用？Codex CLI、Responses API 与模型不可见排查

原 Astra 步骤记录于 2026-09-05；下述 Sol / Luna 补充记录于 2026-09-23（北京时间）。2026-10-16更新内链与可复用排查步骤，未将旧模型开放范围、版本号或参数重新核实为当前规则。本文由 AIXiamo 维护者整理，示例为可自行检查的技术用法，不表示已在读者账号上完成模型调用。

**先确定入口：**Chat、Work / Codex与API分别核对模型和访问权限；不能从其中一个入口的名称推断另一个入口已开放。2026-09记录将Chat中的GPT-6 Pro、Work / Codex的Astra与API模型ID `gpt-6-astra` 分别说明，具体当前名称、账号资格与开放范围请查看 [官方计划说明](https://help.openai.com/en/articles/20001354-gpt-56-and-gpt-6-pro-in-chatgpt)、[Work / Codex 说明](https://help.openai.com/en/articles/20001275-chatgpt-work-and-codex)和本人模型选择器。API使用项目自己的账单，不由ChatGPT订阅支付。

## GPT-6 Sol / Luna 在 Plus、Pro 怎么用？

**直接答案：**先在本人账号的Work / Codex模型选择器和当前官方说明中查可用模型，不以旧5x／20x称呼判断模型资格。2026-09-23的资料记录了GPT-6 Sol / Luna面向Plus、Pro等计划的Work / Codex分批开放，以及当时普通Chat的入口区别；这不是对当前所有账号的开放保证。历史依据：[OpenAI 发布说明](https://openai.com/index/introducing-gpt-6-sol-and-luna/)与 [Work / Codex 模型说明](https://help.openai.com/en/articles/20001275-chatgpt-work-and-codex)。

用本人 ChatGPT 账号登录 Work 或 Codex，查看当前模型选择器；是否显示取决于账号资格、工作区设置和客户端版本。Codex CLI 按 [官方更新记录](https://learn.chatgpt.com/docs/changelog)确认受支持版本，再按客户端帮助选择实际已开放的模型。`0.156.1`（Sol / Luna）与`0.153.0`（Astra）为2026-09资料记录的版本门槛，不代替当前最低要求。使用API key时，按 [API模型目录](https://developers.openai.com/api/docs/models)核对该项目可用的模型ID并独立计费。某个入口没有模型选项，不能单凭这一点判断会员开通失败。

## 1. 在 Codex CLI 中确认 Astra

先检查版本，再用项目目录启动：

```bash
codex --version
codex --help
```

`0.153.0`是2026-09记录的Astra版本要求；现在应以 [官方CLI安装说明](https://learn.chatgpt.com/docs/codex/cli) 和更新记录为准。若最初通过npm安装，可按官方说明升级，然后在项目目录中运行 `codex`：

```bash
npm install -g @openai/codex
codex
```

使用 ChatGPT 登录时，在客户端当前模型列表里选择可用的 Astra，并查看 Usage。使用 API key 登录时，核对 API 项目访问资格。升级客户端只解决版本要求，不能替代模型开放、账号权限或额度。

对于已有仓库，先给 Astra 一个范围明确的任务：

```text
请检查这个仓库中登录失败的原因。
先阅读相关代码与现有测试，给出可复现证据。
只修改与原因直接相关的文件，保留现有未提交改动。
完成后运行能覆盖该问题的检查，并说明修改和验证结果。
不要提交、推送、部署或处理真实账号数据。
```

这里的路径、范围、验证与提交边界可按任务修改。可以先用代码阅读任务确认工具环境，再尝试有明确验收条件的修改；无需用一次大规模重构来验证新模型。

## 2. Responses API 最小请求

前提是API项目已经获得访问权限、拥有可用账单，并满足 [OpenAI API支持地区](https://developers.openai.com/api/docs/supported-countries)要求。将密钥安全配置为`OPENAI_API_KEY`环境变量，将从当前官方模型目录和项目权限中确认的模型ID配置为`OPENAI_MODEL`；不要把密钥写进仓库或截图。

```bash
curl https://api.openai.com/v1/responses \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"model\": \"$OPENAI_MODEL\",
    \"input\": \"解释 HTTP 401 与 403 的区别，用一个 API 项目权限的例子说明。\"
  }"
```

这个请求会产生API用量。`gpt-6-astra`是旧文中的模型ID示例，不要求读者使用未获权限的模型。实际ID、Responses兼容性和可用参数按 [官方模型使用指南](https://developers.openai.com/api/docs/guides/latest-model)核对；示例保留少量参数，便于先确认访问和返回结构，再逐项加入推理设置、工具或长上下文。

读取响应时遍历 `output` 中的消息与文本内容，不能假定第一项一定是最终文本；使用官方 SDK 时可按其文档读取聚合的文本结果。调用记录应保留错误类型、时间和请求 ID，用于排查，避免记录密钥或完整敏感输入。

## 3. 从旧模型迁移，先检查三件事

- **模型和推理参数：**先确认当前模型ID和该模型支持的推理参数，不能把旧文的参数列表套到所有模型。对支持推理设置的模型，按官方文档选一个适用值建立基线，再比较质量、耗时与成本。
- **工具由谁执行：**模型发出调用不等于外部工作已经成功。自定义函数仍由应用执行并回传结果；让请求、执行结果和最终答复可追踪。
- **长输入成本：**上下文上限是容量，不是免费额度。先对真实样本统计输入、输出与工具用量，查看当前模型计价后再扩大批量。

参数与工具行为详见 [官方迁移与功能说明](https://developers.openai.com/api/docs/guides/latest-model)，当前容量和价格详见 [GPT-6 Astra 模型页](https://developers.openai.com/api/docs/models/gpt-6-astra)。

## 4. 看不到模型或请求失败时怎么查？

| 现象 | 优先检查 |
| --- | --- |
| Chat 有 GPT-6 Pro，Codex 没有 Astra | 两个入口的分批开放状态、登录账号和客户端版本 |
| Plus 已开通，某入口没有预期模型 | 核对该模型当前计划资格、开放入口和本人选择器，不从订阅名称推断所有模型都可用 |
| Codex 的 Astra 用量耗尽 | 当前 Usage 与重置提示；Astra 可能比较轻模型更快消耗额度 |
| API 返回 401 | 密钥是否正确、是否失效，以及请求的认证头 |
| API 返回 403 或 404 | 错误正文、项目/模型权限、模型 ID、资源路径与支持地区；不能只凭状态码认定唯一原因 |
| API 返回 429 | 区分速率限制与额度/账单问题，再决定退避重试或处理预算 |

Chat、Work / Codex、API 的额度应分别检查。购买 Credits 不会提前获得分批开放资格；API 的速率限制也不能通过重复购买 ChatGPT 会员解决。API 错误的官方入口为 [Error codes](https://developers.openai.com/api/docs/guides/error-codes)。

## 继续阅读

需要核对会员、Credits与API的关系，查看 [本仓库的计费入口说明](./codex-membership-vs-api.md)。购买前的计划选择和开通后核验可参考 [AIXiamo：GPT-6 Astra 国内怎么用，Plus / Pro 与 API 如何区分](https://www.aixiamo.com/articles/gpt-6-astra-domestic-use-plus-pro-api-2026)。AIXiamo是本文维护者运营的网站；模型功能和访问权限以当前OpenAI官方资料和本人账号为准。
