# Level 4: 简单 Skill

## 学习目标

- 理解 Skill 与 Slash Command 的本质区别
- 掌握 SKILL.md 的基本结构
- 学会创建单文件 Skill
- 理解自动发现机制

## 核心概念

### Skill vs Slash Command

| 方面 | Slash Command | Skill |
|------|---------------|-------|
| **触发方式** | 用户输入 `/命令` | Claude 根据上下文自动调用 |
| **入口文件** | `命令名.md` | `SKILL.md` |
| **目录结构** | `.claude/commands/` | `.claude/skills/技能名/` |
| **核心目的** | 快捷提示 | 封装能力 |

### Skill 的自动发现机制

Skill 通过 `description` 字段被 Claude 发现。当用户的请求匹配 description 中的关键词时，Claude 会自动读取并使用该 Skill。

**关键**: description 必须足够具体，包含触发词！

### 最简结构

```
.claude/skills/
└── my-skill/
    └── SKILL.md       ← 必须是这个文件名
```

### SKILL.md 基本格式

```yaml
---
name: skill-name               # 必须: 小写字母、数字、连字符
description: 描述这个 Skill    # 必须: 告诉 Claude 何时使用
---

# Skill 名称

## Instructions
具体指导...
```

## 本级别示例

### 1. commit-helper - Git 提交助手

**目录**: `.claude/skills/commit-helper/SKILL.md`

```yaml
---
name: commit-helper
description: 生成清晰的 git 提交信息。当用户需要提交代码、写 commit message、或审查暂存的变更时使用。
---

# Commit Helper

## Instructions

1. 运行 `git diff --staged` 查看暂存的变更
2. 分析变更内容
3. 生成提交信息，格式为:
   - 首行: 类型(范围): 简短描述
   - 空行
   - 详细描述（可选）

## 提交类型

- feat: 新功能
- fix: Bug 修复
- docs: 文档更新
- style: 代码格式
- refactor: 重构
- test: 测试相关
- chore: 构建/工具

## 最佳实践

- 使用现在时态
- 首行不超过 50 字符
- 解释 what 和 why，而非 how
```

**触发方式**: "帮我写个提交信息" / "审查我的暂存变更"

### 2. code-explainer - 代码解释器

**目录**: `.claude/skills/code-explainer/SKILL.md`

```yaml
---
name: code-explainer
description: 用通俗易懂的语言解释代码。当用户不理解代码、需要代码讲解、或想学习代码原理时使用。
---

# Code Explainer

## Instructions

1. 先理解代码的整体功能
2. 分解成小块逐步解释
3. 使用类比和比喻
4. 指出关键概念

## 解释结构

### 一句话总结
代码做了什么

### 逐步分解
1. 第一步...
2. 第二步...

### 关键概念
- 概念1: 解释
- 概念2: 解释

### 实际应用
这段代码在实际中用于...
```

**触发方式**: "这段代码是干什么的" / "帮我理解这个函数"

### 3. bug-finder - Bug 查找器

**目录**: `.claude/skills/bug-finder/SKILL.md`

```yaml
---
name: bug-finder
description: 查找代码中的 bug 和问题。当用户遇到错误、代码不工作、需要排查问题时使用。
---

# Bug Finder

## Instructions

1. 仔细阅读代码
2. 识别潜在问题
3. 按严重程度排序
4. 提供修复建议

## 检查清单

### 常见 Bug 类型
- 空指针/未定义引用
- 边界条件
- 类型错误
- 资源泄漏
- 并发问题
- 逻辑错误

## 输出格式

### 🔴 严重问题
[描述 + 修复建议]

### 🟡 潜在风险
[描述 + 建议]

### 🟢 改进建议
[描述 + 建议]
```

**触发方式**: "这段代码有 bug" / "帮我找找问题在哪"

### 4. test-writer - 测试编写器

**目录**: `.claude/skills/test-writer/SKILL.md`

```yaml
---
name: test-writer
description: 编写单元测试和测试用例。当用户需要写测试、增加测试覆盖率、或验证代码正确性时使用。
---

# Test Writer

## Instructions

1. 分析被测代码的功能
2. 识别关键路径和边界情况
3. 设计测试用例
4. 编写测试代码

## 测试覆盖

### 基础测试
- 正常输入
- 预期输出

### 边界测试
- 空值
- 最大/最小值
- 边界条件

### 异常测试
- 无效输入
- 错误处理

## 测试框架

根据项目自动选择：
- Python: pytest
- JavaScript: jest
- C++: gtest
- Go: testing
```

**触发方式**: "帮我写测试" / "这个函数需要单元测试"

## allowed-tools 在 Skill 中的使用

与 Slash Command 类似，Skill 也支持 `allowed-tools`:

```yaml
---
name: safe-reader
description: 只读分析代码，不做任何修改。
allowed-tools: Read, Grep, Glob
---
```

当 Skill 被激活时，Claude 只能使用指定的工具。

## 动手练习

### 练习 1：创建你的第一个 Skill

```bash
# 创建目录
mkdir -p .claude/skills/helper

# 创建 SKILL.md
cat > .claude/skills/helper/SKILL.md << 'EOF'
---
name: helper
description: 通用编程助手。当用户需要编程帮助、代码建议、或技术问题解答时使用。
---

# Helper

## Instructions

1. 理解用户的问题
2. 提供清晰的解答
3. 给出代码示例
4. 解释关键点
EOF
```

### 练习 2：测试 Skill 发现

创建 Skill 后，尝试这些提问：
- "帮我理解这段代码"（应该触发 code-explainer）
- "这里有个 bug"（应该触发 bug-finder）
- "写个测试"（应该触发 test-writer）

### 练习 3：查看可用的 Skills

在 Claude Code 中输入：
```
列出所有可用的 Skills
```

或

```
What Skills are available?
```

## Description 最佳实践

### ❌ 太模糊

```yaml
description: 帮助处理代码
```

### ✅ 具体明确

```yaml
description: 查找代码中的 bug 和问题。当用户遇到错误、代码不工作、需要排查问题时使用。
```

**要点**:
1. 说明 Skill 做什么
2. 说明何时使用
3. 包含用户可能说的关键词

## 调试 Skill

如果 Skill 没有被触发：

1. 检查文件路径是否正确
2. 检查 YAML 语法
3. 检查 description 是否包含触发词
4. 运行 `claude --debug` 查看加载信息

## 下一步

学习 [Level-5: 复杂 Skill](../Level-5-复杂Skill/README.md)，构建多文件的完整 Skill！
