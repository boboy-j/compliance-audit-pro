# ⚖️ Compliance Audit Pro — 合规与内控审计助手

> 面向法律 / 财务 / 采购场景的 AI 风控引擎  
> 逐条映射法规基线 · 自动识别风险条款 · 生成标准化审计底稿

---

## 📌 概述

**Compliance Audit Pro** 是一个 AI 驱动的合规审计工作流（Skill）。将非结构化的业务文本（合同、招标文件、内部制度）输入后，自动完成：

1. **条款提取** — 识别核心义务条款（资质、付款、违约、数据、交付等）
2. **法规映射** — 逐条对照法规基线，标注法条编号与原文
3. **风险评估** — 判定 🔴 禁止性 / 🟡 限制性 / 🟢 提示性 风险等级
4. **底稿生成** — 输出标准化审计底稿，含偏离说明模板、澄清话术、整改责任矩阵
5. **谈判建议** — 针对各风险等级给出答疑、补充协议或备案建议

---

## 🏗 项目结构

```
compliance-audit-pro/
├── V1.0.0/                        # 基础版
│   ├── SKILL.md                   # Skill 定义与工作流指令
│   ├── schema.json                # 入参/出参 JSON Schema
│   ├── examples.json              # 测试用例
│   ├── README.md                  # 版本说明
│   └── knowledge/                 # （基础版无独立知识库）
│
├── V1.1.0/                        # 增强版
│   ├── SKILL.md                   # Skill 定义（含知识库索引）
│   ├── schema.json                # 入参/出参 JSON Schema
│   ├── examples.json              # 测试用例（含验证规则）
│   ├── README.md                  # 版本说明
│   └── knowledge/                 # 内置法规知识库（16 部法规）
│       ├── 01-data-security-law.md
│       ├── 02-cybersecurity-law.md
│       ├── 03-personal-info-protection.md
│       ├── 04-government-procurement.md
│       ├── 05-civil-code-contracts.md
│       ├── 06-anti-unfair-competition.md
│       ├── 07-tendering-bidding.md
│       ├── 08-level-protection.md
│       ├── 09-gdpr.md
│       ├── 10-pip-cross-border.md
│       ├── 11-labor-contract.md
│       ├── 12-antimonopoly.md
│       ├── 13-e-commerce.md
│       ├── 14-copyright.md
│       ├── 15-construction-law.md
│       └── 16-financial-compliance.md
│
└── README.md                      # 本文件（项目总说明）
```

---

## 🧠 内置法规知识库（V1.1.0）

系统内置 **16 部核心法规**的知识库，Agent 执行法规映射时优先检索：

| # | 法规名称 | 文件 |
|:---|:---|:---|
| 一 | 《数据安全法》（2021） | `01-data-security-law.md` |
| 二 | 《网络安全法》（2017） | `02-cybersecurity-law.md` |
| 三 | 《个人信息保护法》（2021） | `03-personal-info-protection.md` |
| 四 | 《政府采购法》（2014 修正） | `04-government-procurement.md` |
| 五 | 《民法典·合同编》（2021） | `05-civil-code-contracts.md` |
| 六 | 《反不正当竞争法》（2019 修正） | `06-anti-unfair-competition.md` |
| 七 | 《招标投标法》（2017 修正） | `07-tendering-bidding.md` |
| 八 | 等保 2.0 / GB/T 22239-2019 | `08-level-protection.md` |
| 九 | GDPR (EU 2016/679) | `09-gdpr.md` |
| 十 | 个保法跨境规则补充（2022） | `10-pip-cross-border.md` |
| 十一 | 《劳动合同法》（2012 修正） | `11-labor-contract.md` |
| 十二 | 《反垄断法》（2022 修正） | `12-antimonopoly.md` |
| 十三 | 《电子商务法》（2019） | `13-e-commerce.md` |
| 十四 | 《著作权法》（2020 修正） | `14-copyright.md` |
| 十五 | 《建筑法》及《建设工程质量管理条例》 | `15-construction-law.md` |
| 十六 | 金融合规专项（资管新规/反洗钱法） | `16-financial-compliance.md` |

> 每条法规文件以表格形式列出：条款编号 → 要点 → 审计关注点 → 典型违规场景。Agent 可精确引用法条，而非模糊匹配。

---

## 🚀 快速使用

该 Skill 可在以下 AI Agent 平台中以标准格式加载：

- **OpenClaw** — 直接导入 `skill.yaml` / `SKILL.md`
- **Dify** — 配置为 Plugin / Workflow 工具
- **Coze** — 导入 Bot Skill
- **SkillHub** — 直接发布

### 配置调用

```json
{
  "skill": "compliance_audit_pro",
  "inputs": {
    "document_content": "<合同/招标文件全文或核心条款>",
    "compliance_scope": "数据安全法/政府采购法/民法典",
    "risk_threshold": "strict"
  }
}
```

| 参数 | 类型 | 必填 | 说明 |
|:---|:---|:---|:---|
| `document_content` | string | ✅ | 待审查的合同/招标文件/制度文本 |
| `compliance_scope` | string | ✅ | 适用法规范围，用 `/` 分隔 |
| `risk_threshold` | string | ❌ | 风险判定阈值：`low` / `medium` / `high` / `strict`（默认 `high`） |

### 输出示例

```markdown
# 🛡️ 合规审计报告

## 1. 风险条款映射表
| 原文条款摘要 | 对应法规 | 风险等级 | 合规状态 |
|:---|:---|:---|:---|
| 中标方需将核心业务数据迁移至指定政务云 | 《数据安全法》第31条 | 🔴 | 偏离 |

## 2. 审计底稿草案
- **事实描述**：约定数据迁移至政务云但未约定分类分级与安全评估
- **法规依据**：《数据安全法》第21条、第27条、第31条
- **偏离说明模板**：[直接复制至正式回函]
- **整改责任人**：项目组 / 法务部

## 3. 澄清与谈判建议
- 高风险项：建议发起书面答疑/补充协议
> 📌 本结果基于公开法规库生成，重大合规决策需经法务/内控部门复核。
```

---

## 📋 版本对比

| 版本 | 核心能力 | 知识库 |
|:---|:---|:---|
| **V1.0.0** | 基础合规审计（风险映射 + 底稿生成） | 无独立知识库文件 |
| **V1.1.0** | + 16 部法规内置知识库 + 精确法条引用 + 典型违规场景库 + 严格模式 | `knowledge/` 独立文件，Agent 按需加载 |

---

## ⚠️ 重要提示

1. **非替代法律意见** — 本工具输出的审计报告和建议仅作为辅助参考，重大合规决策须经法务/内控部门复核
2. **法规时效性** — 知识库基于发布日截面的法律文本编制，如遇法规修订请核实当前有效版本
3. **知识库外法规** — `compliance_scope` 若包含知识库未收录的法规，Agent 应依据公开法律文本自行补充，并在报告中标注 ⚠️

---

## 📄 许可

MIT License
