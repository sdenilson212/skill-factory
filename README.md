# Skill Factory

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub Actions: Validate](https://github.com/sdenilson212/skill-factory/actions/workflows/validate.yml/badge.svg)](https://github.com/sdenilson212/skill-factory/actions)

**SKILL 自动化生产流水线** — 从需求描述到高质量 Agent Skill 包的完整制作流程。

[English](#english) | 中文

---

## 为什么需要 Skill Factory？

当你需要批量生产高质量 Agent Skill 时，手写容易出现：

- 结构混乱（缺少步骤、frontmatter 不规范）
- 安全漏洞（注入风险、越狱指令未被检测）
- 逻辑不一致（步骤前后矛盾、输入输出不对应）
- 无法追溯（没有审核记录，不知道谁改过）

**Skill Factory 提供了一套经过验证的五阶段流水线**，确保每一个 Skill 都经过需求确认、安全审核和代码审计，可追溯、可复用、有质量保障。

---

## 特性

- **五阶段质量门禁**：前置检查 → 需求挖掘 → 安全审核 → 包生成 → 质量审计
- **P0/P1/P2 分级机制**：严重问题强制回退，重要问题就地修复，优化建议直接改进
- **三维度安全审核**：结构完整性 + 安全性（注入/越狱/信息泄露）+ 逻辑一致性
- **全程人工确认**：所有文件写入前需用户确认，可随时中断

---

## 快速开始

```bash
# 1. 将 skill-factory 放入你的 skills 目录
cp -r skill-factory/ ~/.workbuddy/skills/

# 2. 在 AI 对话中激活 skill-factory
#   描述你的需求，例如：
#   "帮我创建一个 SKILL，用来分析 Excel 文件并输出 JSON"

# 3. skill-factory 自动执行流水线
#   - Stage 0: 检查依赖
#   - Stage 1: 需求挖掘（多轮询问）
#   - Stage 2: 安全审核
#   - Stage 3: 生成 SKILL 包
#   - Stage 4: 代码审计
#   - Stage 5: 安装（可选）

# 环境变量（可选）
export WORKBUDDY_SKILLS_DIR=~/.workbuddy/skills/  # 默认值
```

---

## vs 其他方案

| 方案 | 优点 | 缺点 |
|------|------|------|
| 手写 SKILL.md | 灵活 | 无审核、无标准、容易出错 |
| 单次 prompt 生成器 | 快速 | 无质量保障、无分级审核 |
| **Skill Factory** | **五阶段门禁 + P0/P1/P2 审核 + 可追溯** | **依赖四个子技能** |

---

## 架构

```
Stage 0: 前置检查       →  检查四个子技能是否已安装
Stage 1: prompt-generator  →  强制多轮需求挖掘 + 生成 SKILL 提示词草稿
Stage 2: prompt-auditor    →  安全性和逻辑完整性审核
Stage 3: skill-creator     →  生成完整 SKILL 包
Stage 4: skill-auditor     →  代码逻辑分析 + 安全扫描 + 修复
Stage 5: 安装             →  从临时目录安装到正式技能目录（可选）
```

---

## 前置依赖

运行本流水线需要以下子技能已安装：

- `prompt-generator`（需求挖掘和提示词生成）
- `prompt-auditor`（SKILL.md 安全性审核）
- `skill-creator`（SKILL 包结构生成）
- `skill-auditor`（SKILL 包代码审计）

> 注：以上四个技能目前基于 OpenClaw/WorkBuddy 生态实现。如需独立运行，请自行实现符合各 Stage 规范的工具。

---

## 目录结构

```
skill-factory/
├── SKILL.md                    # 流水线编排器（主入口）
├── README.md                   # 本文件
├── LICENSE                     # MIT License
├── CONTRIBUTING.md             # 贡献指南
├── .gitignore                  # Git 忽略配置
├── .github/
│   └── workflows/
│       └── validate.yml        # GitHub Actions 自动验证
└── references/
    ├── skill-template.md       # SKILL.md 标准格式模板
    ├── audit-checklist.md     # Stage 2 审核检查清单
    └── pipeline-log.md        # 流水线运行日志格式（预留）
```

---

## 审核分级

| 级别 | 定义 | 处理方式 |
|------|------|---------|
| P0 | 严重安全漏洞 / 逻辑崩溃（注入攻击、越狱风险、系统信息泄露） | 强制回退 Stage 1 |
| P1 | 重要逻辑缺陷 / 功能缺失（路径错误、矛盾约束、过度授权） | 就地修复，二次审核 |
| P2 | 优化建议（格式问题、歧义表述、示例缺失） | 直接修复，继续流程 |

---

## 自动修复规则（Stage 4）

**可自动修复**：格式错误、拼写错误、缺少可选字段

**需用户确认后修复**：逻辑结构变更、功能需求调整、安全策略修改

---

## 适用场景

- 团队需要批量生产标准化 Agent Skill
- 希望建立统一的 Skill 质量审核流程
- 需要将 Skill 生产过程文档化、可追溯

---

## License

MIT License

---

<a name="english"></a>

## English

### Why Skill Factory?

Building Agent Skills by hand at scale leads to common problems:

- **Inconsistent structure** — missing steps, malformed frontmatter
- **Security gaps** — prompt injection risks, jailbreak vectors left undetected
- **Logic conflicts** — contradictory constraints, mismatched inputs/outputs
- **No traceability** — no audit trail, no way to know what changed or why

**Skill Factory** solves this with a battle-tested five-stage pipeline that takes you from a plain-language requirement all the way to a reviewed, installable Skill package.

---

### Features

- **Five-stage quality gates**: Dependency check → Requirements mining → Security audit → Package generation → Code audit
- **P0/P1/P2 issue classification**: Critical issues force a rollback; important issues are fixed in place; suggestions are applied immediately
- **Three-dimensional security audit**: Structural integrity + Security (injection/jailbreak/data leakage) + Logic consistency
- **Human confirmation at every write**: No files are created or modified without explicit user approval — you can abort at any point

---

### Quick Start

```bash
# 1. Copy skill-factory into your skills directory
cp -r skill-factory/ ~/.workbuddy/skills/

# 2. Activate it in an AI conversation — just describe your need:
#    "Create a Skill that analyzes Excel files and outputs JSON"

# 3. The pipeline runs automatically:
#    Stage 0 — Check dependencies
#    Stage 1 — Mine requirements (multi-round Q&A)
#    Stage 2 — Security audit
#    Stage 3 — Generate SKILL package
#    Stage 4 — Code audit
#    Stage 5 — Install (optional)
```

---

### Pipeline Architecture

```
Stage 0  Dependency Check    Verify the four sub-skills are installed
Stage 1  prompt-generator    Multi-round requirements mining + prompt draft
Stage 2  prompt-auditor      Security & logic integrity review
Stage 3  skill-creator       Generate complete SKILL package
Stage 4  skill-auditor       Code analysis + security scan + auto-fix
Stage 5  Install             Move from temp dir to skills directory (optional)
```

---

### Prerequisites

The following four sub-skills must already be installed:

| Skill | Purpose |
|-------|---------|
| `prompt-generator` | Requirements mining & prompt generation |
| `prompt-auditor` | SKILL.md security & logic audit |
| `skill-creator` | SKILL package structure generation |
| `skill-auditor` | Code audit & security scan |

> These skills are implemented for the OpenClaw / WorkBuddy ecosystem. To use them standalone, implement tools that conform to each stage's interface contract.

---

### Issue Severity Levels

| Level | Definition | Action |
|-------|-----------|--------|
| P0 | Critical security flaw or logic collapse (injection, jailbreak, system info leak) | Force rollback to Stage 1 |
| P1 | Important logic defect or missing functionality (bad paths, conflicting constraints, over-permission) | Fix in place, re-audit |
| P2 | Improvement suggestion (formatting, ambiguous wording, missing examples) | Apply directly, continue |

---

### Repository Structure

```
skill-factory/
├── SKILL.md                    # Pipeline orchestrator (main entry point)
├── README.md                   # This file
├── LICENSE                     # MIT License
├── CONTRIBUTING.md             # Contribution guide
├── .gitignore
├── .github/
│   └── workflows/
│       └── validate.yml        # CI: validates SKILL.md structure
└── references/
    ├── skill-template.md       # Standard SKILL.md format template
    ├── audit-checklist.md      # Stage 2 audit checklist
    └── pipeline-log.md         # Pipeline run log format
```

---

### License

MIT — see [LICENSE](./LICENSE)
