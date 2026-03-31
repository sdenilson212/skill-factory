# Skill Factory 架构设计文档

## 项目概览

**Skill Factory** 是一套完整的 AI Skill 自动化生产流水线。将模糊的需求转换为高质量的、经过审核的、可复用的 Skill 包。

### 设计理念

**流水线思想**：每个阶段有明确的输入、处理逻辑和输出，相邻阶段通过严格的验收门禁连接。

```
用户需求
   ↓
Stage 0: 依赖检查
   ↓ (依赖都存在)
Stage 1: 需求挖掘 (prompt-generator)
   ↓
Stage 2: 安全审核 (prompt-auditor)
   ↓ (P0/P1 问题修复)
Stage 3: 包生成 (skill-creator)
   ↓
Stage 4: 质量审计 (skill-auditor)
   ↓ (>= 70分)
Stage 5: 安装启动 (installation, 可选)
   ↓
✅ 可用的 Skill 包
```

---

## 完整目录结构

```
skill-factory/
├── SKILL.md                            ← 根 SKILL（协调层）
├── ARCHITECTURE.md                     ← 本文档
├── SKILL_FACTORY_COMPLETE.md           ← 完整指南
├── SKILL_GENERATOR.md                  ← Stage 3 技术细节
├── README.md                           ← 快速开始 + 双语版本
├── LICENSE
├── CONTRIBUTING.md
├── .github/
│   └── workflows/
│       └── validate.yml                ← CI 结构验证
│
├── stage1-prompt-generator/            ← 【需求挖掘】
│   ├── SKILL.md                        ├─ 阶段式提问框架
│   ├── EXPERIENCE.md                   ├─ 经验积累（10 条）
│   └── scripts/
│       └── diagnose_requirement.py     └─ 诊断工具
│
├── stage2-prompt-auditor/              ← 【安全审核】
│   ├── SKILL.md                        ├─ 三维度审核
│   ├── EXPERIENCE.md                   ├─ 常见漏洞清单
│   └── scripts/
│       └── audit_skill.py              └─ 审核引擎
│
├── stage3-skill-creator/               ← 【包生成】⭐ 核心
│   ├── SKILL.md                        ├─ 包生成全流程
│   ├── EXPERIENCE.md                   ├─ 生成决策指南
│   ├── SKILL_GENERATOR.md              ├─ 技术细节 + 示例
│   └── scripts/
│       ├── generate_skill_package.py   ├─ 主生成器
│       ├── experience_generator.py     ├─ EXPERIENCE.md 生成
│       └── template_selector.py        └─ 模板选择算法
│
├── stage4-skill-auditor/               ← 【质量审计】
│   ├── SKILL.md                        ├─ 7 维度评估框架
│   ├── EXPERIENCE.md                   ├─ 评分标准详解
│   └── scripts/
│       ├── evaluate_skill.py           ├─ 评估引擎
│       ├── priority_matrix.py          ├─ 优先级矩阵
│       └── improvement_planner.py      └─ 改进路径规划
│
└── stage5-installation/                ← 【安装启动】可选
    ├── SKILL.md                        ├─ 安装流程
    └── scripts/
        └── install_skill.py            └─ 安装脚本
```

---

## 五个阶段详解

### Stage 0：依赖检查（根 SKILL 负责）

**职责**：验证四个子技能都已安装

**流程**：
```
检查 ~/.workbuddy/skills/ 中是否存在：
  ✓ prompt-generator/
  ✓ prompt-auditor/
  ✓ skill-creator/
  ✓ skill-auditor/
```

**失败处理**：
- 如有缺失 → 列出清单 → 停止流水线
- 用户安装后重试

---

### Stage 1：需求挖掘（prompt-generator）

**文档位置**：`stage1-prompt-generator/SKILL.md`

**关键文件**：
- `SKILL.md` - 三阶段提问框架、模糊诊断表、版本决策树
- `EXPERIENCE.md` - 10 条经验，覆盖快速识别、模糊回答、理解确认等

**核心输出**：
```
SKILL.md 提示词草稿
├── 元数据（name、version、description）
├── 核心职责
├── 执行步骤
├── 输入输出格式
└── 错误处理规则
```

**质量指标**：
- 需求明确度 >= 0.8（简洁版）/ 0.6-0.8（标准版）/ < 0.6（详细版）
- 理解确认通过

---

### Stage 2：安全审核（prompt-auditor）

**文档位置**：`stage2-prompt-auditor/SKILL.md`

**三维度审核**：

1. **结构完整性** (40%)
   - 元数据齐全
   - 步骤覆盖完整
   - 输入输出明确

2. **安全性** (35%)
   - ❌ 注入漏洞
   - ❌ 越狱指令
   - ❌ 信息泄露
   - ⚠️ 权限过高

3. **逻辑一致性** (25%)
   - 输入输出对应
   - 步骤顺序合理
   - 约束明确

**输出**：
```
P0 问题（必须修复）：N 个
P1 问题（应该修复）：N 个
P2 问题（优化建议）：N 个
修复方案：[具体列表]
```

**通过标准**：P0 = 0，且无 P1 问题（或用户确认接受）

---

### Stage 3：包生成（skill-creator）⭐ 核心

**文档位置**：`stage3-skill-creator/SKILL.md` 和 `SKILL_GENERATOR.md`

**这一步的创新**：
- ✅ 自动生成完整 SKILL 包目录结构
- ✅ **自动生成 EXPERIENCE.md 的第一部分（3-5 条基础经验）**
- ✅ 在 SKILL.md 中插入"激活时必读"段落
- ✅ 生成元数据追溯文件（.metadata.json）

**自动生成的经验模板**（根据 SKILL 类型）：

| 类型 | 模板 | 条目 |
|------|------|------|
| 代码生成 | `template_code_generation` | 语言版本、框架选择、错误处理、测试用例、依赖版本 |
| 数据分析 | `template_data_analysis` | 数据源差异、数据清洗、指标定义、可视化、性能权衡 |
| 文案创作 | `template_copywriting` | 受众定义、品牌调性、转化目标、长度限制、参考案例 |
| AI 绘画 | `template_ai_painting` | 风格定义、主体背景、构图、色调、用途分辨率 |

**输出包结构**：
```
[skill-name]/
├── SKILL.md (含"激活时必读"段)
├── EXPERIENCE.md (含 5 条自动经验 + "补充位置"标记)
├── .metadata.json (追溯信息)
└── scripts/
    └── activate.py (激活脚本)
```

---

### Stage 4：质量审计（skill-auditor）

**文档位置**：`stage4-skill-auditor/SKILL.md` 和 `stage4-skill-auditor/EXPERIENCE.md`

**7 维度评估框架**：

| 维度 | 权重 | 说明 |
|------|------|------|
| 完整性 | 20% | 所有必要部分都有 |
| 清晰度 | 15% | 步骤明确可执行 |
| 可行性 | 20% | 真的能执行，依赖可满足 |
| 证据支持 | 15% | 基于最佳实践或案例 |
| 泛化性 | 10% | 适用多个场景 |
| 新颖性 | 10% | 相比现有 Skill 的创新 |
| 风险缓解 | 10% | 边界明确，降级方案完善 |

**打分规则**：
- **>= 70 分** ✅ 通过，可安装
- **60-70 分** ⚠️ 条件通过（需补充说明）
- **< 60 分** ❌ 不通过，需重做

**输出**：
```
审计报告
├── 总体评分：75 分 ✅
├── 各维度分数（with 详细说明）
├── P0/P1/P2 问题分级
└── 改进路径规划
```

---

### Stage 5：安装启动（installation，可选）

**文档位置**：`stage5-installation/SKILL.md`

**流程**：
1. 验证包完整性（SKILL.md、EXPERIENCE.md、.metadata.json 存在）
2. 复制到 `~/.workbuddy/skills/[skill-name]/`
3. 激活测试（运行 activate.py，验证 EXPERIENCE.md 可读）
4. 标记安装完成

---

## 关键特性

### 1. 经验积累体系（EXPERIENCE.md）

**两部分结构**：
- **Part 1（自动生成）**：5 条基础经验（Stage 3 生成）
- **Part 2（用户反馈）**：N 条用户反馈经验（Stage 5 后补充）

**格式**：每条经验都是"见过 / 因此知道 / 现在会"的三段式。

### 2. 元数据追溯

每个生成的 SKILL 包都包含 `.metadata.json`：
```json
{
  "skill_name": "example-skill",
  "version": "1.0.0",
  "generated_by": "skill-factory Stage 3",
  "generated_at": "2026-03-31T18:45:00Z",
  "based_on": {
    "stage_1_at": "...",
    "stage_2_at": "...",
    "stage_3_at": "..."
  },
  "confidence": 0.85,
  "experience_parts": {
    "auto_generated": 5,
    "user_feedback": 0
  }
}
```

### 3. 质量门禁

每个阶段都有明确的通过标准：

```
Stage 1 → 理解确认通过
      ↓
Stage 2 → P0 = 0，无未修复的 P1
      ↓
Stage 3 → 包结构完整
      ↓
Stage 4 → 评分 >= 70 分
      ↓
Stage 5 → 激活测试通过
```

---

## 实际使用场景

### 场景 1：用户要创建一个新 Skill

```
用户："帮我创建一个 SKILL，用来分析 Excel 文件并输出 JSON"

流程：
1. 根 SKILL 触发 Stage 0（检查依赖）
2. → Stage 1 启动三阶段提问（快速识别 → 按类型深入 → 理解确认）
3. → Stage 2 审核生成的提示词（安全性、结构完整性、逻辑一致性）
4. → Stage 3 生成包（自动生成 EXPERIENCE.md 中的"数据处理类"经验）
5. → Stage 4 审计打分（>= 70 分）
6. → Stage 5 安装到 skills 目录

完成：用户得到 ~/.workbuddy/skills/excel-to-json/
```

### 场景 2：用户反馈后改进 Skill

```
用户："这个 SKILL 生成的代码没有错误处理"

流程：
1. Stage 4 的"经验积累"触发
2. 问题被记录到 EXPERIENCE.md Part 2
3. 下次激活这个 SKILL 时，自动读取 EXPERIENCE.md（含用户反馈）
4. 执行时自动采用改进方案

结果：SKILL 越用越聪明
```

---

## 与 Adaptive Skill System 的关联

本项目（Skill Factory）是**生产阶段**，与 Adaptive Skill System 中的 Layer 3（自动生成 Skill）配合使用：

```
Adaptive Skill System 的 Layer 3
        ↓
"无法组合，需要自动生成新 Skill"
        ↓
调用 Skill Factory 完整流水线
        ↓
生成的 SKILL 保存到 KB
        ↓
下次遇到类似问题可直接调用
```

---

## 文件清单

### 根目录文档
- `SKILL.md` - 流水线协调层，定义 Stage 0-5 流程
- `ARCHITECTURE.md` - 本文档，系统设计说明
- `SKILL_FACTORY_COMPLETE.md` - 完整使用指南（含示例）
- `SKILL_GENERATOR.md` - Stage 3 的技术细节 + 代码示例
- `README.md` - 快速开始 + 双语版本

### 五个 Stage 目录
每个都包含：
- `SKILL.md` - 该阶段的执行手册
- `EXPERIENCE.md` - 该阶段的经验积累
- `scripts/` - 执行脚本

### CI 配置
- `.github/workflows/validate.yml` - 结构验证

---

## 版本历史

### v1.0.0 (2026-03-17)
- 初始版本，五阶段流水线完整设计
- 四个依赖技能 + 一个协调 SKILL

### v1.1.0 (2026-03-31)
- 🆕 完整的目录结构（五个 stage 子项目）
- 🆕 Stage 3 的 EXPERIENCE.md 自动生成功能
- 🆕 Stage 1 的三阶段提问框架 + 模糊诊断表
- 🆕 本架构设计文档
- 🆕 SKILL_GENERATOR.md 技术细节

---

## 许可证

MIT License - 见 `LICENSE` 文件

