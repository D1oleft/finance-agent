# 数据格式定义

## 交易记录 (transactions.jsonl)

每行一条记录，JSON格式：

```json
{
  "id": "tx_20260616_001",
  "time": "2026-06-16T12:30:00+08:00",
  "amount": 25.00,
  "type": "expense",
  "category": "餐饮",
  "description": "午饭",
  "account": "微信",
  "tags": []
}
```

字段说明：
- `id`: 唯一标识，格式 `tx_{日期}_{序号}`
- `time`: ISO 8601 格式时间
- `amount`: 金额（正数）
- `type`: `expense`（支出）或 `income`（收入）
- `category`: 分类（餐饮/交通/娱乐/购物/居住/订阅/医疗/学习/转账）
- `description`: 描述
- `account`: 账户名称（可选）
- `tags`: 标签数组（可选）

## 账户 (accounts.json)

```json
{
  "accounts": [
    {"name": "支付宝", "balance": 2500.00, "type": "e-wallet"},
    {"name": "微信", "balance": 1800.00, "type": "e-wallet"},
    {"name": "银行卡", "balance": 15000.00, "type": "bank"},
    {"name": "现金", "balance": 300.00, "type": "cash"}
  ]
}
```

类型：`e-wallet`（电子钱包）、`bank`（银行卡）、`cash`（现金）、`investment`（投资）

## 定期收支 (recurring.json)

```json
{
  "recurring": [
    {
      "id": "rec_001",
      "description": "房租",
      "amount": 2000,
      "type": "expense",
      "category": "居住",
      "account": "银行卡",
      "day": 1,
      "enabled": true
    }
  ]
}
```

- `day`: 每月几号执行（1-31）
- `enabled`: 是否启用

## 预算 (budget.json)

```json
{
  "monthly_budget": 3000,
  "categories": {
    "餐饮": 1000,
    "交通": 300,
    "娱乐": 500,
    "购物": 800,
    "居住": 0,
    "医疗": 0,
    "学习": 200,
    "订阅": 200,
    "其他": 0
  }
}
```

## 订阅 (subscriptions.json)

```json
{
  "subscriptions": [
    {
      "name": "B站大会员",
      "amount": 25,
      "cycle": "monthly",
      "next_date": "2026-06-18",
      "category": "娱乐",
      "account": "支付宝"
    }
  ]
}
```

周期：`monthly`（月）、`yearly`（年）

## 存钱目标 (goals.json)

```json
{
  "goals": [
    {
      "name": "旅行基金",
      "target": 5000,
      "saved": 2000,
      "deadline": "2026-12-31",
      "account": "支付宝"
    }
  ]
}
```

## 负债 (debts.json)

```json
{
  "debts": [
    {
      "name": "花呗",
      "type": "huabei",
      "initial_amount": 5000,
      "current_amount": 3200,
      "repayment_day": "每月9号",
      "repayment_history": [
        {
          "date": "2026-06-01",
          "amount": 1000,
          "note": ""
        }
      ],
      "created_at": "2026-05-01T00:00:00+08:00"
    }
  ]
}
```

类型：`credit-card`（信用卡）、`huabei`（花呗）、`jiebei`（借呗）、`loan`（贷款）、`other`（其他）

字段说明：
- `name`: 负债名称
- `type`: 负债类型
- `initial_amount`: 初始欠款金额
- `current_amount`: 当前剩余欠款
- `repayment_day`: 还款日描述
- `repayment_history`: 还款记录数组
- `created_at`: 创建时间
