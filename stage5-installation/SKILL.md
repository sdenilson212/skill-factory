---
name: installation
version: 1.0.0
category: operational
description: SKILL 安装与启动 — 验证包完整性，安装到技能库，测试激活
---

# Stage 5: Installation — SKILL 安装与启动

> **本 SKILL 是 skill-factory 流水线的第五步（可选）**
>
> 职责：验证包完整性 → 安装到技能库 → 测试激活  
> 输出：已安装的 SKILL，可直接使用

---

## 核心职责

### 输入
Stage 4 审计通过的 SKILL 包

### 输出
安装到 `~/.workbuddy/skills/` 的完整 SKILL

### 安装流程

1. **验证包完整性**
   - 检查 SKILL.md 存在
   - 检查 EXPERIENCE.md 存在
   - 检查 .metadata.json 存在

2. **复制到技能库**
   ```bash
   cp -r [skill-name] ~/.workbuddy/skills/
   ```

3. **激活测试**
   - 调用 activate.py 测试
   - 验证 EXPERIENCE.md 可读
   - 确认没有错误

4. **记录安装**
   - 在 .metadata.json 中标记 installed=true
   - 记录安装时间和路径

### 这一步是可选的
如果用户想自己管理安装，可以跳过。但推荐使用本步骤确保标准化。

