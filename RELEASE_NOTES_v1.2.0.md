# Skill Factory v1.2.0 — 首次发布

## 🎯 项目介绍

**Skill Factory** 是一个 **AI Skill 自动化生成工具包**。它解决的核心问题是：

> **当 AI 无法直接回答用户问题时，如何自动生成一个新的"Skill"（能力模块）来解决它？**

### 核心设计理念

Skill Factory 采用 **五阶段流水线**，完全自动化地从用户需求生成高质量的 AI Skill 包：

```
需求分析
    ↓
提示词生成
    ↓
提示词审核
    ↓
Skill 包生成
    ↓
质量审计
```

每个阶段都由专门的 Skill 驱动，形成**自洽的工具链**。

---

## 🚀 五阶段工作流详解

### Stage 1: 需求分析 → 提示词生成
**使用工具**: `prompt-generator`

将用户的模糊需求转化为**结构化的提示词**。

**输入**: 用户问题
```
"我想创建一个分析跑步数据的工具"
```

**输出**: 结构化提示词 (JSON 格式)
```json
{
  "task_type": "data_analysis",
  "domain": "sports",
  "requirements": [
    "解析 Garmin 数据格式",
    "生成训练效果评分",
    "提供个性化建议"
  ],
  "constraints": ["显存 6GB", "实时性要求中等"],
  "version": "standard"
}
```

---

### Stage 2: 提示词审核 & 优化
**使用工具**: `prompt-auditor`

对 Stage 1 生成的提示词进行**多维度评估和改进**。

**评估维度**（7 个）:
- ✅ 完整性（是否覆盖所有需求）
- ✅ 清晰度（表述是否明确）
- ✅ 可行性（是否能真正执行）
- ✅ 证据支持（是否有依据）
- ✅ 泛化性（是否适用更广泛场景）
- ✅ 新颖性（是否有创新角度）
- ✅ 风险缓解（是否识别了问题）

**输出**: 改进后的提示词 + 评分（0-1）

**通过标准**: 综合评分 ≥ 0.70

---

### Stage 3: Skill 包自动生成
**使用工具**: `skill-creator`

基于审核通过的提示词，**自动生成完整的 Skill 包目录**。

**生成内容**:
```
运动数据分析 Skill/
├── SKILL.md              # Skill 完整定义文档
├── EXPERIENCE.md         # 经历提示词（质地知识）
├── scripts/              # 可执行脚本
│   ├── main.py
│   └── utils.py
└── references/           # 参考资料
    ├── garmin_format.md
    └── analysis_methods.md
```

**SKILL.md 结构**（约 1500 字）:
- 定义 & 用途
- 激活时必读（关键要点）
- 输入/输出规范
- 核心逻辑
- 使用示例
- 边界说明

**EXPERIENCE.md 结构**（约 500 字）:
- 见过什么（真实场景）
- 因此知道什么（规律认知）
- 现在会怎么做（执行方式）

---

### Stage 4: 质量审计 & 自动修复
**使用工具**: `skill-auditor`

对生成的 Skill 包进行**全面质量检查和修复建议**。

**检查清单**:
- 代码质量（PEP8 / 结构）
- 文档完整度
- 使用示例是否可运行
- 异常处理是否充分
- 性能指标是否合理

**输出**:
- 问题清单（P0 ~ P3 优先级）
- 自动修复建议
- 分数改进路径

**通过标准**: 综合评分 ≥ 0.70 且无 P0 问题

---

### Stage 5: 安装后验证 & 经验积累
**使用工具**: 内置验证逻辑

Skill 安装后，**自动触发一次经验扫描**：
- 询问用户是否踩坑过
- 记录首次使用的问题点
- 追加到 EXPERIENCE.md 第二部分
- 不达标则降级为试验版

---

## 📦 项目结构

```
skill-factory/
├── SKILL.md                    # 项目定义
├── EXPERIENCE.md               # 质地知识
├── README.md                   # 快速参考
├── CHANGELOG.md                # 版本历史
├── RELEASE_NOTES_v1.2.0.md    # 本文件
├── scripts/
│   ├── validate.py             # 结构验证脚本
│   └── install.py              # 安装脚本
└── references/
    ├── PROMPT_GENERATOR.md     # Stage 1 依赖
    ├── PROMPT_AUDITOR.md       # Stage 2 依赖
    ├── SKILL_CREATOR.md        # Stage 3 依赖
    └── SKILL_AUDITOR.md        # Stage 4 依赖
```

---

## 🔧 四个核心依赖 Skill

Skill Factory 本身由四个核心 Skill **驱动**，每个负责一个阶段：

| Skill | 阶段 | 功能 |
|-------|------|------|
| **prompt-generator** | Stage 1 | 需求 → 提示词 |
| **prompt-auditor** | Stage 2 | 提示词质量评估 |
| **skill-creator** | Stage 3 | 提示词 → Skill 包 |
| **skill-auditor** | Stage 4 | Skill 包质量审计 |

这四个 Skill 已独立发布，可单独使用或作为 Skill Factory 的组件。

---

## 🧠 EXPERIENCE.md — 质地知识系统

传统 Skill 定义了"能做什么"，但缺少"应该怎么判断质量"。

**EXPERIENCE.md** 补充了这一点，包含三个层次：

### 第一部分：见过什么
```
场景1: 用户问题过于模糊（如"帮我优化营销"）
→ 为什么重要: 模糊需求导致生成的 Skill 无用
→ 解决办法: 阶段式提问，逐步深入
```

### 第二部分：因此知道什么
```
规律1: 好的 Skill 的通用特征
  - 输入/输出规范明确
  - 包含 3+ 真实例子
  - 边界说明清晰
  
规律2: 常见失败模式
  - 生成后立即使用（应先验证）
  - 忽视文档完整度
  - 不考虑用户技能水平
```

### 第三部分：现在会怎么做
```
行动1: Stage 2 审核时，重点检查边界说明
行动2: Stage 5 验证时，必须试运行示例代码
行动3: 遇到模糊需求时，激活 prompt-generator 的"阶段式提问"
```

---

## 📊 项目数据

- **总代码行数**: 2500+ 行
- **文档行数**: 4000+ 行
- **Skill 定义数**: 4 个（prompt-generator / prompt-auditor / skill-creator / skill-auditor）
- **支持的 Skill 类型**: 20+ 种（代码/文案/数据分析/设计/营销等）
- **开发周期**: 3 个月深度迭代

---

## 🎯 使用场景

### 场景 1: 营销 Skill 生成
```
输入: "我要做一个 Z 世代产品的营销分析工具"
     ↓ Stage 1-4
输出: 完整的营销分析 Skill 包
      (包含: 用户画像分析、爆款拆解、渠道策略等)
```

### 场景 2: 数据分析 Skill 生成
```
输入: "分析跑步训练数据并提供个性化建议"
     ↓ Stage 1-4
输出: 运动数据分析 Skill
      (包含: Garmin 数据解析、KPI 评估、训练优化建议等)
```

### 场景 3: 编程 Skill 生成
```
输入: "我需要一个 React 表单验证的最佳实践工具"
     ↓ Stage 1-4
输出: React 表单 Skill
      (包含: 验证规则库、错误处理、测试用例等)
```

---

## 🚀 快速开始

### 前置要求
- Python 3.8+
- 四个核心 Skill 已安装（prompt-generator 等）

### 安装
```bash
git clone https://github.com/sdenilson212/skill-factory.git
cd skill-factory
python scripts/install.py
```

### 基本用法
```python
from skill_factory import SkillFactory

factory = SkillFactory()

# 从用户需求生成 Skill
skill_package = factory.create_skill(
    user_request="我想要一个数据可视化的最佳实践工具",
    domain="data_analysis",
    target_audience="数据分析师"
)

# Skill 包会自动生成在 output/skills/ 目录
print(f"Skill 包已保存: {skill_package.path}")
```

---

## 📖 完整文档

- **[SKILL.md](./SKILL.md)** — Skill Factory 完整定义
- **[EXPERIENCE.md](./EXPERIENCE.md)** — 质地知识文档
- **[README.md](./README.md)** — 快速参考
- **[CHANGELOG.md](./CHANGELOG.md)** — 版本历史

---

## 🔗 相关项目

这个项目是 **AI 员工办公室系统** 的核心组件之一：

- **[ai-memory-system](https://github.com/sdenilson212/ai-memory-system)** — 记忆/知识库管理（v1.2.0）
- **[adaptive-skill-system](https://github.com/sdenilson212/adaptive-skill-system)** — 自适应 Skill 系统（v1.1.0）
- **[workbuddy-skills](https://github.com/sdenilson212/workbuddy-skills)** — Skill 包合集

---

## 📄 License

MIT License — 自由使用、修改和分发。

---

## 👋 反馈 & 贡献

如果你在使用中发现问题或有改进建议，欢迎提交 Issue 或 PR！

---

**发布时间**: 2026-03-31  
**版本**: v1.2.0  
**维护者**: [sdenilson212](https://github.com/sdenilson212)