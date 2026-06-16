# 💰 Finance Agent

<p align="center">
  <strong>聊天就是记账，AI帮你分析</strong>
</p>

<p align="center">
  <a href="https://github.com/D1oleft/finance-agent/stargazers"><img src="https://img.shields.io/github/stars/D1oleft/finance-agent?style=social" alt="Stars"></a>
  <a href="https://github.com/D1oleft/finance-agent/blob/main/LICENSE"><img src="https://img.shields.io/github/license/D1oleft/finance-agent" alt="License"></a>
  <img src="https://img.shields.io/badge/version-2.1.0-blue" alt="Version">
  <img src="https://img.shields.io/badge/Claude%20Code-Skill-purple" alt="Claude Code Skill">
  <img src="https://img.shields.io/badge/MiMo%20Code-Skill-orange" alt="MiMo Code Skill">
</p>

---

## 这是什么？

一个 Claude Code / MiMo Code 的记账 Skill。不用装 App，直接在聊天里记账。

**核心能力**：自然语言记账 → 自动分类 → 多账户管理 → 预算追踪 → 消费分析 → 负债管理

## 效果对比

| | 记账 App | Excel | Finance Agent |
|---|---|---|---|
| 记账速度 | 30秒 | 1分钟 | 3秒 |
| 智能分类 | 手选 | 手动 | 自动 |
| 多账户 | 支持 | 手动 | 自动识别 |
| 分析报表 | 基础 | 手动 | AI 分析 |
| 主动提醒 | 部分 | 无 | 推送 |
| 数据隐私 | 云端 | 本地 | 本地 |

## 快速开始

### 安装

```bash
git clone https://github.com/D1oleft/finance-agent.git
cp -r finance-agent ~/.claude/skills/finance-agent
```

### 使用

```
# 记账（自然语言）
午饭25
微信买咖啡18
支付宝交房租2000
工资8000

# 查看报表
这个月花了多少

# 账户管理
账户总览

# 预算
设置预算3000

# 订阅
我有哪些订阅

# 定期收支
设置房租每月1号2000

# 导入账单
导入支付宝账单

# 存钱目标
存钱目标10000

# 负债管理
负债总览
添加花呗欠3200每月9号
还款花呗1000
```

## 功能列表

| 功能 | 命令 | 自然语言触发 |
|------|------|-------------|
| 记账 | `/track` | "午饭25" |
| 账户 | `/accounts` | "账户总览" |
| 定期 | `/recurring` | "设置房租每月1号2000" |
| 导入 | `/import` | "导入支付宝账单" |
| 报表 | `/report` | "这个月花了多少" |
| 预算 | `/budget` | "设置预算3000" |
| 订阅 | `/sub` | "我有哪些订阅" |
| 提醒 | `/remind` | "提醒我明天还款" |
| 目标 | `/goal` | "存钱目标10000" |
| 负债 | `/debt` | "负债"、"欠多少"、"还款" |
| 导出 | `/export` | "导出本月账单" |

## v2.0 新增

- ✅ 多账户支持（支付宝/微信/银行卡/现金）
- ✅ 定期收支（房租工资自动记）
- ✅ 账单导入（支付宝/微信CSV）
- ✅ 数据导出（CSV格式）
- ✅ 转账记录
- ✅ 账户余额追踪
- ✅ 自动初始化（首次使用自动创建数据文件）
- ✅ 错误处理（边界情况全覆盖）
- ✅ 负债管理（添加负债、还款记录、负债总览）

## 文件结构

```
finance-agent/
├── SKILL.md                    # 主入口
├── README.md                   # 本文件
├── templates.md                # 输出模板
├── data-formats.md             # 数据格式定义
├── error-handling.md           # 错误处理
├── commands/
│   ├── track.md                # 记账
│   ├── accounts.md             # 账户管理
│   ├── recurring.md            # 定期收支
│   ├── import.md               # 账单导入
│   ├── report.md               # 消费报表
│   ├── budget.md               # 预算管理
│   ├── sub.md                  # 订阅管理
│   ├── remind.md               # 提醒设置
│   ├── goal.md                 # 存钱目标
│   ├── debt.md                 # 负债管理
│   └── export.md               # 数据导出
└── templates/
    ├── accounts.json           # 账户模板
    ├── budget.json             # 预算模板
    ├── recurring.json          # 定期收支模板
    ├── subscriptions.json      # 订阅模板
    ├── goals.json              # 目标模板
    ├── debts.json              # 负债模板
    └── transactions.jsonl      # 交易记录模板
```

## 支持的工具

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- [MiMo Code](https://mimo.xiaomi.com/mimocode)
- 任何支持 Agent Skills 标准的 AI 工具

## 设计原则

1. **简单优先** — 记账要快，不要繁琐
2. **智能识别** — 自然语言，不用选菜单
3. **多账户** — 区分资金来源
4. **主动提醒** — 不等用户问，主动推送
5. **数据安全** — 本地存储，不上传云端

## License

MIT

---

<p align="center">
  如果这个项目帮到了你，请给个 ⭐ Star 支持一下！
</p>
