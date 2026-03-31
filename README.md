# Skill Factory — SKILL 生产流水线

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub Actions: Validate](https://github.com/sdenilson212/skill-factory/actions/workflows/validate.yml/badge.svg)](https://github.com/sdenilson212/skill-factory/actions)

**自动化 SKILL 包生产系统** — 从需求到高质量 Agent Skill 的完整五阶段流水线。

---

## 问题 What Problem Does It Solve?

手写 SKILL 包存在的问题：

```
❌ 结构混乱     — 缺少标准格式、步骤不完整、frontmatter 不规范
❌ 安全漏洞     — 注入风险、越狱指令、信息泄露无人检测
❌ 逻辑不一致   — 步骤矛盾、输入输出对不上、业务流错误
❌ 无法追溯     — 没有审核记录、不知道谁改了什么、为什么改
❌ 质量不稳定   — 人工操作差异大、没有标准化流程
```

**Skill Factory 的解决方案**：经过验证的五阶段流水线 + 自动化审核 + 可追溯的质量管控

---

## 核心特性

### 🔒 五阶段质量门禁

| Stage | 职责 | 输出 |
|-------|------|------|
| **Stage 0** | 前置检查 | ✅ 验证依赖、环境检查 |
| **Stage 1** | 需求挖掘 | 📝 多轮需求确认、SKILL 提示词草稿 |
| **Stage 2** | 安全审核 | 🔍 结构/逻辑/安全性完整检查（内置） |
| **Stage 3** | 包生成 | 📦 完整 SKILL 目录（.md + scripts/ + references/） |
| **Stage 4** | 质量审计 | 🐛 代码分析 + 安全扫描 + 自动修复提案 |
| **Stage 5** | 安装 | ✨ 从临时目录移到正式技能库（可选） |

### 🎯 P0/P1/P2 分级审核

问题自动分级，处理方式清晰：

| 级别 | 问题类型 | 处理方式 |
|------|---------|---------|
| **P0** | 🚨 严重安全漏洞<br/>—— 注入/越狱/信息泄露/系统崩溃 | ⛔ 强制回退 Stage 1<br/>（必须重新设计） |
| **P1** | ⚠️ 重要逻辑缺陷<br/>—— 路径错误/矛盾约束/功能缺失 | 🔧 就地修复 + 二次审核<br/>（继续流程） |
| **P2** | 💡 优化建议<br/>—— 格式问题/歧义/示例不足 | ✅ 直接修复<br/>（继续流程） |

### 🔄 完整追溯机制

每个 SKILL 都有：
- 生成时间戳
- 执行者信息
- 每个 Stage 的检查结果
- 修改历史和版本号
- 阶段间的数据流

### 🛡️ 内置审核引擎（Stage 2）

不依赖外部工具，直接检查：

- **结构完整性** — SKILL.md 必读项、frontmatter、步骤格式
- **安全性** — 正则检测注入模式、越狱指令、敏感信息泄露
- **逻辑一致性** — 输入输出类型匹配、步骤前后依赖关系、约束无矛盾

---

## 快速开始

### 安装

```bash
# 1. 将 skill-factory 安装到 ~/.workbuddy/skills/
cp -r skill-factory/ ~/.workbuddy/skills/

# 2. 验证安装
cd ~/.workbuddy/skills/skill-factory/
python -m pytest tests/ -v  # 可选，需有测试文件
```

### 使用

在 AI 对话中描述需求：

```
"帮我创建一个 SKILL，用来自动分析用户反馈并提取关键洞察。
需求：
- 输入：CSV 文件中的用户评论
- 处理：情感分析 + 主题提取 + 优先级排序
- 输出：JSON 格式的洞察报告"
```

AI 自动执行流水线：

```
✓ Stage 0: 检查 prompt-generator / skill-creator / skill-auditor 已安装
✓ Stage 1: 多轮问卷确认 → 生成 SKILL 提示词
✓ Stage 2: 安全审核 + 逻辑检查 → 全部通过
✓ Stage 3: 生成完整 SKILL 包
✓ Stage 4: 代码审计 → 2 个 P2 问题修复
✓ Stage 5: 安装到 ~/.workbuddy/skills/user-feedback-analyzer/

🎉 SKILL 已就绪，可直接使用
```

---

## 前置依赖

本流水线依赖以下三个子 Skill（需先安装）：

### 1. `prompt-generator` — 需求挖掘 & 提示词生成
- 功能：多轮问卷确认 + 自动生成高质量提示词草稿
- 触发：Stage 1
- 状态：✅ 来自 WorkBuddy/OpenClaw 生态

### 2. `skill-creator` — SKILL 包结构生成
- 功能：根据提示词自动生成 SKILL.md + 目录结构
- 触发：Stage 3
- 状态：✅ 来自 WorkBuddy/OpenClaw 生态

### 3. `skill-auditor` — 代码审计 & 安全扫描
- 功能：逻辑检查 + 安全漏洞扫描 + 自动修复提案
- 触发：Stage 4
- 状态：✅ 来自 WorkBuddy/OpenClaw 生态

> **注**：Stage 2 的内置审核引擎是 Skill Factory 本身包含的，不需要额外 Skill 支持。

---

## 架构设计

```
┌─────────────────────────────────────────┐
│ 用户需求 (自然语言描述)                  │
└────────────┬────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────┐
│ Stage 0: 前置检查                        │
│  · 验证三个子 Skill 已安装               │
│  · 检查环境变量和工作目录                │
└────────────┬────────────────────────────┘
             │ ✓ 依赖就绪
             ▼
┌─────────────────────────────────────────┐
│ Stage 1: prompt-generator (需求挖掘)    │
│  · 多轮问卷确认需求                     │
│  · 生成 SKILL 提示词草稿                 │
│  · 输出：prompt_draft.md                │
└────────────┬────────────────────────────┘
             │ ✓ 提示词完成
             ▼
┌─────────────────────────────────────────┐
│ Stage 2: 内置审核引擎 (安全 & 逻辑检查) │
│  · 结构完整性验证                       │
│  · 安全性扫描（注入/越狱/泄露）         │
│  · 逻辑一致性检查                       │
│  · 输出：audit_report.md                │
└────────────┬────────────────────────────┘
             │
         ┌───┴────┐
    P0?  │        │ P1/P2?
         ▼        ▼
       ❌         ▼
    回退    ┌─────────────────┐
    Stage1   │ 问题分类 & 排序  │
             └────────┬────────┘
                      │
                      ▼ 修复/继续
┌─────────────────────────────────────────┐
│ Stage 3: skill-creator (包生成)         │
│  · 根据提示词生成完整 SKILL 包           │
│  · 目录结构：SKILL.md + EXPERIENCE.md   │
│           + scripts/ + references/      │
│  · 输出：skill-package/                 │
└────────────┬────────────────────────────┘
             │ ✓ 包生成完成
             ▼
┌─────────────────────────────────────────┐
│ Stage 4: skill-auditor (质量审计)       │
│  · 代码逻辑分析                         │
│  · 安全漏洞扫描                         │
│  · 自动修复提案                         │
│  · 输出：audit_results.json             │
└────────────┬────────────────────────────┘
             │ ✓ 审计完成
             ▼
┌─────────────────────────────────────────┐
│ Stage 5: 安装 (可选)                    │
│  · 从临时目录移到正式技能库              │
│  · 验证安装完成                         │
│  · 输出：安装路径确认                   │
└────────────┬────────────────────────────┘
             │
             ▼
    🎉 SKILL 已就绪，可直接使用
```

---

## 目录结构

```
skill-factory/
├── SKILL.md                         # 流水线编排器（主入口）
├── README.md                        # 本文件
├── LICENSE                          # MIT License
├── CONTRIBUTING.md                  # 贡献指南
├── .gitignore                       # Git 忽略配置
├── .github/
│   └── workflows/
│       └── validate.yml             # GitHub Actions 自动验证
└── references/
    ├── skill-template.md            # SKILL.md 标准格式模板
    ├── audit-checklist.md           # Stage 2 审核检查清单
    ├── p0-p1-p2-examples.md         # 问题分级示例库
    └── best-practices.md            # 最佳实践文档
```

---

## vs 其他方案对比

| 方案 | 灵活性 | 质量保障 | 可追溯 | 学习成本 | 场景适配 |
|------|--------|---------|--------|---------|---------|
| **手写 SKILL.md** | ⭐⭐⭐⭐⭐ | ⭐ | ⭐ | ⭐ | 一次性小需求 |
| **单次提示词生成器** | ⭐⭐⭐ | ⭐⭐ | ⭐ | ⭐⭐ | 快速原型 |
| **Skill Factory** | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | **批量生产 + 团队协作** |

---

## 适用场景

✅ **推荐使用**：
- 团队需要批量生产标准化 Agent Skill
- 建立统一的 Skill 质量管控流程
- Skill 生产过程需要文档化和审计追溯
- 新团队成员快速学习 Skill 设计标准
- 对 Skill 质量和安全性有严格要求

❌ **不必要使用**：
- 一次性临时小需求（直接手写可能更快）
- 已有成熟 Skill 库，只做简单维护（用 skill-auditor 单独审计即可）

---

## 参考资源

- [Skill 标准格式](./references/skill-template.md) — SKILL.md 模板和规范
- [审核检查清单](./references/audit-checklist.md) — Stage 2 检查项详解
- [问题分级示例](./references/p0-p1-p2-examples.md) — 真实案例库
- [最佳实践](./references/best-practices.md) — 设计建议和常见坑点

---

## License

MIT License — 详见 [LICENSE](./LICENSE) 文件

---

## 贡献

欢迎提交 Issue 和 PR！详见 [CONTRIBUTING.md](./CONTRIBUTING.md)
