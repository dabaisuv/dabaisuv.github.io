---
title: Windows Server 2012 R2 安全加固
date: 2023-11-17 18:36:58
tags:
---

## Windows Server 2012 R2 安全加固

### 一、身份识别
| 基线项名称 | 设置密码使用期限策略 |
| ------------ | ------------ |
| 检查步骤 | 在管理工具打开本地安全策略，依次打开：安全设置--账户策略--密码策略，检查密码最长使用期限与密码最短使用期限两项设置 |
| 基线合规性判定 | 密码最长使用期限设置为30-180之间，密码最短使用期限设置为1-14之间即合规，否则不合规 |
| 加固步骤 | 将密码最长使用期限设置为30-180之间，建议值为90，密码最短使用期限设置为1-14之间，建议值为7。 |

| 基线项名称 | 密码复杂性设置 |
| ------------ | ------------ |
| 检查步骤 | 在管理工具打开本地安全策略，依次打开：安全设置--账户策略--密码策略，检查密码必须符合复杂性要求与密码长度最小值两项设置 |
| 基线合规性判定 | 已启用密码必须符合复杂性要求，且密码长度最小值为8及以上即合规，否则不合规 |
| 加固步骤 | 启用密码必须符合复杂性要求，并设置密码长度最小值为8 |

| 基线项名称 | '强制密码历史'设置为 5 - 24 之间 |
| ------------ | ------------ |
| 检查步骤 | 在管理工具打开本地安全策略，依次打开：安全设置--账户策略--密码策略，检查强制密码历史是否大于等于五 |
| 基线合规性判定 | 已启用强制密码历史，且值大于等于五即合规，否则不合规 |
| 加固步骤 | 将强制密码历史设置为 5-24 之间，推荐为5 |

| 基线项名称 | 配置账户锁定策略 |
| ------------ | ------------ |
| 检查步骤 | 在管理工具打开本地安全策略，依次打开：安全设置--账户策略--账户锁定策略，检查账户锁定阈值，账户锁定时间和重置账户锁定计数器 |
| 基线合规性判定 | 账户锁定阈值设置为3-8之间且账户锁定时间和重置账户锁定计数器设置为10-30之间即合规，否则不合规 |
| 加固步骤 | 将账户锁定阈值设置为3-8之间，建议值为5。将账户锁定时间和重置账户锁定计数器设置为10到30之间，建议值为15。 |