---
description: 记账。自然语言记录消费或收入。当用户说"午饭25"、"花了100"、"微信买咖啡18"时使用。
argument-hint: "[消费描述]"
allowed-tools: Read Write Bash
---

# 记账

## 流程

### 1. 初始化检查
检查 `~/finance/data/transactions.jsonl` 是否存在，不存在则执行统一初始化流程（参见 SKILL.md）。

### 2. 解析输入
从 `$ARGUMENTS` 中提取：
- **金额**：数字部分
- **描述**：文字部分
- **账户**：根据关键词判断（可选）
- **类型**：`expense`（支出）、`income`（收入）或 `transfer`（转账），默认 `expense`
- **分类**：根据关键词自动判断

> **注意**：`type` 字段必须使用英文 `expense`、`income` 或 `transfer`，不能使用中文。

### 3. 校验账户
如果指定了账户，检查 `~/finance/data/accounts.json` 中是否存在该账户。如果不存在：
- 花呗、信用卡、借呗 → 自动设为 `credit` 类型创建账户，并在 `debts.json` 中创建对应条目（初始欠款 0）。**创建前先检查是否已存在，已存在则跳过创建。**
- 其他账户 → 询问用户账户类型后添加

### 4. 信用账户消费同步
如果账户类型为 `credit`（花呗、信用卡、借呗），消费时：
- `debts.json`：对应负债的 `current_amount` 增加消费金额（欠更多钱）
- `accounts.json`：对应账户的 `balance` 减少消费金额（信用额度减少，余额变为更负的数）

示例：花呗余额 0，消费 500 后 → balance = -500，current_amount = 500

### 5. 写入记录

> **空文件处理**：如果 `transactions.jsonl` 为空或不存在，直接写入第一条记录。

追加到 `~/finance/data/transactions.jsonl`：
```json
{"id":"tx_{date}_{seq}","time":"{now}","amount":{amount},"type":"{type}","category":"{category}","description":"{desc}","account":"{account}","tags":[]}
```

> **JSONL 损坏处理**：读取 `transactions.jsonl` 时，参考 [error-handling.md](../error-handling.md) 的 JSONL 损坏处理流程。

### 6. 更新账户余额
如果指定了账户，更新 `~/finance/data/accounts.json` 中的余额。

### 7. 返回确认
- 如果指定了账户：
```
✅ 已记录
- {描述}：{金额}元（{账户}）
- 今日支出：{今日总额}元
- {账户}余额：{余额}元
- 本月{分类}：{本月分类总额}元
```
- 如果未指定账户：
```
✅ 已记录
- {描述}：{金额}元
- 今日支出：{今日总额}元
- 本月{分类}：{本月分类总额}元
```

## 分类规则和账户识别

分类规则和账户识别规则统一定义在 [SKILL.md](../SKILL.md) 的「智能识别规则」部分，此处不再重复定义。

## 转账功能

当用户说"从支付宝转500到微信"时：
1. 从源账户扣除金额
2. 向目标账户增加金额
3. 写入一条交易记录（type 为 `transfer`，category 为"转账"），记录源账户（`account`）和目标账户（`to_account`）

```json
{"id":"tx_{date}_{seq}","time":"{now}","amount":500,"type":"transfer","category":"转账","description":"从支付宝转到微信","account":"支付宝","to_account":"微信","tags":[]}
```

## 边界情况

- 金额无法识别 → 询问金额
- 多笔消费 → 要求分开记录
- 账户不存在 → 花呗/信用卡/借呗自动创建（credit 类型+负债条目），其他询问用户
- 退款/收入 → 记录为收入类型（type: `income`）
- 负数金额 → 取绝对值并记录（用户可能误输入负号）
- 零金额 → 不记录，提示金额必须大于0
- 转账源账户和目标账户相同 → 提示不能自己转给自己
- 转账源账户余额不足 → 提示余额不足
