# Claude Code Skills & Slash Commands 学习教程

本教程将教你如何编写 Claude Code 的 **Slash Commands** 和 **Skills**，从简单到复杂逐步深入。

## 核心概念对比

| 特性 | Slash Commands | Skills |
|------|----------------|--------|
| **复杂度** | 简单提示 | 复杂能力 |
| **结构** | 单个 .md 文件 | 目录 + SKILL.md + 资源文件 |
| **触发方式** | 用户主动调用 `/command` | Claude 自动发现和调用 |
| **文件数量** | 仅一个文件 | 可包含多个文件、脚本、模板 |
| **存储位置** | `.claude/commands/` 或 `~/.claude/commands/` | `.claude/skills/` 或 `~/.claude/skills/` |
| **分享方式** | 通过 git | 通过 git |

## 何时使用 Slash Commands

- 你经常重复调用同一个提示
- 提示可以放在单个文件中
- 你想要明确控制何时运行

**示例场景**：
- `/review` → "审查这段代码的 bug 并提出改进建议"
- `/explain` → "用简单的话解释这段代码"
- `/optimize` → "分析这段代码的性能问题"

## 何时使用 Skills

- Claude 应该自动发现并使用该能力
- 需要多个文件或脚本
- 复杂的工作流程带有验证步骤
- 团队需要标准化的详细指导

**示例场景**：
- PDF 处理 Skill：包含表单填写脚本和验证
- 数据分析 Skill：包含不同数据类型的参考文档
- 文档 Skill：包含样式指南和模板

## 教程目录

### Slash Commands 部分

| 级别 | 目录 | 描述 |
|------|------|------|
| Level-1 | `Level-1-简单命令/` | 最基础的命令，单文件，无参数 |
| Level-2 | `Level-2-带参数命令/` | 使用 `$ARGUMENTS` 和 `$1` 等参数 |
| Level-3 | `Level-3-高级命令/` | 带 Frontmatter、工具权限、Bash 执行 |

### Skills 部分

| 级别 | 目录 | 描述 |
|------|------|------|
| Level-4 | `Level-4-简单Skill/` | 单文件 Skill，基础结构 |
| Level-5 | `Level-5-复杂Skill/` | 多文件 Skill，包含脚本和模板 |

## 快速开始

### 创建你的第一个 Slash Command

```bash
# 创建项目级命令目录
mkdir -p .claude/commands

# 创建一个简单的 review 命令
echo "审查这段代码，找出潜在的 bug 和改进点" > .claude/commands/review.md

# 使用
> /review
```

### 创建你的第一个 Skill

```bash
# 创建项目级 Skill 目录
mkdir -p .claude/skills/code-helper

# 创建 SKILL.md
cat > .claude/skills/code-helper/SKILL.md << 'EOF'
---
name: code-helper
description: 帮助分析和改进代码。当用户需要代码审查、优化建议或代码解释时使用。
---

# Code Helper

## Instructions
1. 分析代码结构和逻辑
2. 识别潜在问题
3. 提供改进建议
EOF

# 自动生效，问 Claude 相关问题时会自动使用
```

## 存储位置说明

### Slash Commands
- **项目级**: `.claude/commands/` - 团队共享，显示 "(project)"
- **用户级**: `~/.claude/commands/` - 个人使用，显示 "(user)"

### Skills
- **项目级**: `.claude/skills/` - 团队共享
- **用户级**: `~/.claude/skills/` - 个人使用

## 学习路径建议

1. **Level-1**: 从最简单的命令开始，理解基本结构
2. **Level-2**: 学习参数传递，使命令更灵活
3. **Level-3**: 掌握 Frontmatter 和高级特性
4. **Level-4**: 理解 Skill 的自动发现机制
5. **Level-5**: 构建完整的多文件 Skill

每个级别都包含：
- `README.md` - 该级别的说明
- 多个示例目录 - 可直接使用的完整示例
