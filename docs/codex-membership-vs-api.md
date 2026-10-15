---
title: "Codex 需要什么会员？Plus、Pro、Credits 与 API 的区别"
description: "按实际登录方式区分 Codex 订阅、个人 Credits、工作区用量与 API 账单，说明 Pro100、Pro200 本人账号新开续费能解决什么问题。"
last_modified_at: 2026-10-16
---

# Codex 需要 Plus / Pro，还是应该单独用 API？

## 直接答案

**想用本人 ChatGPT 账号登录 Codex，先看会员计划和 Usage；使用 API key 时，才按对应 API 账户独立计费。** Codex 客户端是工具，订阅与 API 是两条认证和账单路径。CLI 也支持 ChatGPT 登录，不能仅因为“在命令行、脚本或服务器里”就认定必须买 API 余额。[官方认证说明](https://learn.chatgpt.com/docs/auth)列出了两种登录方式。

如果要办理个人 Pro，AIXiamo 支持本人账号新开和续费、支付宝与中文协助：当前 [Pro100 ¥749](https://www.aixiamo.com/item/8?utm_source=github&utm_medium=docs&utm_campaign=pro100_pro200_guide_20261016&utm_content=codex_membership_pro100)、[Pro200 ¥1298](https://www.aixiamo.com/item/7?utm_source=github&utm_medium=docs&utm_campaign=pro100_pro200_guide_20261016&utm_content=codex_membership_pro200)。这是2026年10月16日本站人民币服务报价，不是 OpenAI 官方美元定价；实时库存和金额以商品页为准。**购买的是本人账号的会员服务，不是 API 余额，也不是单独购买 Codex Credits。**

## 两条路径

| 路径 | 常见入口 | 账单与限制 | 在哪里核对 |
| --- | --- | --- | --- |
| ChatGPT 账号登录 | 支持的 Codex 桌面、CLI、IDE、云端入口 | 所选个人或工作区的计划、用量及权限 | 当前账号、工作区、Usage；组织另核 Billing 与管理员设置 |
| API key 登录或直接 API 调用 | 支持 API 登录的本地客户端、程序、CI、SDK | 对应 API 组织或项目的独立计量与账单 | API 项目、用量与账单，不看 ChatGPT 套餐余额 |

一个人可以同时有会员和 API 账户，但两边的消费不会因为邮箱相同就自动合并。ChatGPT 订阅付款不会给 API 账户充值；API 余额也不会让 ChatGPT 套餐自动升级。

## 会员、Credits 和 API 余额分别是什么？

| 名称 | 解决的问题 | 与另外两者的关系 |
| --- | --- | --- |
| Plus／Pro 会员 | 依当前计划使用 ChatGPT 和支持的 Codex 功能 | 有计划内使用量，不等于 API 预付余额 |
| 个人 Credits | 符合条件的 Plus／Pro 用户在计划内用量之外继续使用支持功能 | 以本人 Usage 显示的购买与适用条件为准，不是 API 余额 |
| 工作区 credits | 适用组织计划的共享用量 | 由合同、账单设置、席位及管理员权限决定，不等于个人 Credits |
| API 余额或账单 | 程序化调用 API 或使用 API key 的计费 | 独立管理预算、项目、模型与调用记录 |

个人用户看自己的 Usage；组织用户请有权限的管理员核对工作区 Billing、合同和用量控制。不要把组织管理员能看到的入口当作所有个人账号都有的入口。[官方定价说明](https://learn.chatgpt.com/docs/pricing)分别列出订阅、额外用量和 API 路径。

## 什么时候优先看会员

- 主要由本人在 ChatGPT 或 Codex 客户端中连续完成研究、文档或开发任务。
- 希望使用 ChatGPT 账号支持的客户端与云端功能。
- 同时需要 ChatGPT 会员的其他功能，并希望在同一账号核验套餐。
- 正在比较 Pro100、Pro200 的个人使用体验和当前官方权益。

Pro100 与 Pro200 的新开、续费是普通办理场景，不要求先购买 Plus。已有 Plus 的升级是另一条路径，周期和金额应单独确认，见[Plus 与 Pro 选择及升级说明](chatgpt-plus-vs-pro.md)。

## 什么时候优先看 API

- 把模型调用直接嵌入自己的产品或内部服务。
- 需要按 API 项目管理程序调用、预算和请求日志。
- 使用 API key 认证的 Codex 本地流程或 CI 工作流。
- 需要的能力明确属于 API，不能由网页会员替代。

是否使用 API 应按认证方式、功能和部署场景判断；不能仅凭“自动化”三个字推断账单。受管工作区和可信自动化还可能有专门权限设置，应按官方说明及管理员要求配置，不把个人订阅暴露给不可信公开环境。

## 什么时候两边都需要

可以将本人交互式工作放在 ChatGPT／Codex，把产品内 API 调用放在独立 API 项目。分别记录两边预算、账号和用量即可。若 Codex 已提示限制，先核对登录方式和 Usage，再决定续费、升级、购买适用 Credits 或调整 API 预算，见[额度排查步骤](codex-quota-usage.md)。

## 三步确认自己正在用哪条账单

1. 在当前客户端资料或账号菜单确认账号与工作区；不要拿浏览器里的另一个账号代替客户端账号。
2. CLI 用户可用 `codex login status` 查看当前认证方式；此检查不需要把 Token、密码或 API key 发给任何人。
3. ChatGPT 登录看套餐与 Usage，API key 看对应 API 项目的账单。确认清楚后，再选择所需商品或额度渠道。

## Windows、WSL 和完全访问权限不是套餐

Windows 原生还是 WSL2，决定命令在哪种系统环境执行；文件权限、网络和审批设置决定任务能做什么；会员或 API 则决定认证、功能与计费。缺少依赖、目录不可写或模型不可选，不应直接当作“会员没到账”。先看错误位置，再核账号、环境和权限。

## AIXiamo 会员办理与技术资料

AIXiamo 的 Pro100 可自动领卡后自助办理，也可选择全程人工帮助；Pro200 由客服按原订单人工办理。两档的30天订阅保障、状态条件和售后材料见[办理与售后说明](pro-service-and-after-sales.md)。办理完成后在同一个 ChatGPT 账号核验套餐，继续在 Codex 核验登录与用量，不把会员订单当作 API 充值凭证。

[AIXiamo：Codex 与 ChatGPT 会员、API 的关系](https://www.aixiamo.com/articles/codex-and-gpt-membership-relation-2026?utm_source=github&utm_medium=docs&utm_campaign=pro100_pro200_guide_20261016&utm_content=codex_membership_api)保留更多中文操作背景。本文由 AIXiamo 维护，官方产品事实与本店办理事实分别说明，见[资料维护说明](../DISCLOSURE.md)。

资料核对：2026年10月16日。[官方认证](https://learn.chatgpt.com/docs/auth)用于认证和账单边界；[官方定价](https://learn.chatgpt.com/docs/pricing)用于当前计划及用量。本文不列固定消息次数，不用历史模型表替代本人当前 Usage。
