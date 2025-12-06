# Slash Command vs Skill - 实际对比

## 场景：用户要求代码审查

假设你既有一个 Slash Command `/review`，又有一个 Skill `code-review`。

---

## 方式 1：使用 Slash Command

### 目录结构

```
.claude/
├── commands/
│   └── review.md
```

### review.md 内容

```markdown
审查这段代码，找出：
1. 潜在的 bug
2. 性能问题
3. 代码风格问题
4. 可改进的地方

用中文回复。
```

### 用户使用

```
👤 用户: 
  /review

🤖 Claude:
【直接执行 review.md 的内容】

审查结果...
```

### 关键特点

```
✅ 用户明确指定了 /review
✅ Claude 无需判断，直接执行
✅ 成功率 100%
✅ 但用户必须记住命令名
```

---

## 方式 2：使用 Skill

### 目录结构

```
.claude/
└── skills/
    └── code-review/
        └── SKILL.md
```

### SKILL.md 内容

```yaml
---
name: code-review
description: 全面的代码审查。当用户需要审查代码、代码 review、PR 审查时使用。
---

# Code Review

## Instructions

1. 仔细阅读代码
2. 按照检查清单逐项审查
3. 按严重程度对问题分类
4. 提供具体的改进建议

## 检查清单

- [ ] 有 bug 吗？
- [ ] 性能合理吗？
- [ ] 代码风格统一吗？
- [ ] 可维护性如何？
```

### 用户使用 - 方式 A（精确匹配）

```
👤 用户:
  帮我审查这段代码

🤖 Claude:
【自动识别 "审查" ≈ "review"】
【加载 code-review Skill】

审查结果...
```

### 用户使用 - 方式 B（模糊匹配）

```
👤 用户:
  这段代码怎么样？看看有没有问题

🤖 Claude:
【自动识别用户想要代码评价】
【匹配 code-review Skill】

审查结果...
```

### 用户使用 - 方式 C（用户不知道有这个 Skill）

```
👤 用户:
  我想让 AI 检查我的代码质量

🤖 Claude:
【自动识别 "检查代码质量" ≈ "代码审查"】
【加载 code-review Skill】
【用户甚至不知道有这个 Skill，但 Claude 自动使用了】

审查结果...
```

### 关键特点

```
✅ 用户无需记住命令
✅ 用任何相似的表达都能触发
✅ Claude 自动选择，用户无需显式指定
✅ 对用户更友好
❌ 需要 description 足够好才能准确匹配
```

---

## 对比总结

| 方面 | Slash Command | Skill |
|------|---|---|
| **触发方式** | 手动输入 `/command` | 自动发现 |
| **用户体验** | 需要记住命令 | 自然对话 |
| **成功率** | 100%（用户明确指定） | 取决于 description |
| **输入复杂度** | `/review` | "帮我审查这段代码" |
| **发现难度** | 容易（看 `/help`） | 困难（需要知道 description） |
| **场景** | 快捷操作 | 自动化工作流 |

---

## 何时使用哪个

### 使用 Slash Command

```
1️⃣ 频繁使用的快捷操作
   /commit - 生成提交信息
   /test - 生成测试代码
   
2️⃣ 团队需要统一的命令
   /format - 格式化代码
   /lint - 代码检查
   
3️⃣ 参数较多的操作
   /review-pr 456 high alice
   /translate 英文 这段文字
```

### 使用 Skill

```
1️⃣ 复杂的多步骤工作流
   - 代码审查（含多个检查维度）
   - API 设计（含规范文档）
   
2️⃣ 可能会被多种问法触发的功能
   - "代码审查" / "PR 审查" / "代码检查"
   - 所有表达都指向同一个 Skill
   
3️⃣ 不希望用户必须记住命令名
   - 新手用户可以自然提问
   - Claude 自动提供能力
```

---

## 真实场景：混合使用

### 完整的开发流程

```
👤 用户: "帮我写个二分查找"

🤖 Claude:
【没有明确命令，用 Skill 自动发现】
【加载 code-generator Skill】

[生成代码...]

👤 用户: "/test

🤖 Claude:
【用户显式调用 /test 命令】
【直接执行 test.md】

[生成测试...]

👤 用户: "这段代码怎么优化？"

🤖 Claude:
【自动发现 code-optimizer Skill】
【加载 Skill...】

[优化建议...]

👤 用户: "/commit fix binary search implementation

🤖 Claude:
【用户显式调用 /commit 命令】
【执行 commit.md】

[生成提交信息...]
```

---

## 实验：观察区别

### 实验 1：测试 Slash Command

```bash
# 创建一个 Slash Command
mkdir -p .claude/commands
echo "这是一个审查命令" > .claude/commands/review-cmd.md
```

```
👤 用户: /review-cmd

🤖 Claude:
【直接执行 review-cmd.md】
✅ 100% 成功

👤 用户: 帮我审查代码

🤖 Claude:
【不知道 /review-cmd 命令的存在】
❌ 0% 成功
```

### 实验 2：测试 Skill

```bash
# 创建一个 Skill
mkdir -p .claude/skills/code-review-skill
cat > .claude/skills/code-review-skill/SKILL.md << 'EOF'
---
name: code-review-skill
description: 审查代码
---

# Code Review

审查代码的内容...
EOF
```

```
👤 用户: 帮我审查代码

🤖 Claude:
【自动识别 "审查代码"】
【加载 code-review-skill Skill】
✅ 成功

👤 用户: /review-skill

🤖 Claude:
【没有 /review-skill 命令】
❌ 失败
```

---

## 为什么都需要

### Slash Command 的优势

```
1. 显式控制
   /lint 明确表示 "运行 linter"
   
2. 快捷
   输入少，执行快
   
3. 发现容易
   /help 就能看到所有命令
```

### Skill 的优势

```
1. 用户友好
   自然语言就能触发
   
2. 灵活
   同一个能力，多种问法
   
3. 自动化
   Claude 自动选择合适的 Skill
```

---

## 最终建议

### 对于频繁的操作

```yaml
# 使用 Slash Command
.claude/commands/commit.md
.claude/commands/test.md
.claude/commands/review.md
```

### 对于复杂的工作流

```yaml
# 使用 Skill
.claude/skills/code-review/SKILL.md
.claude/skills/api-designer/SKILL.md
.claude/skills/documentation/SKILL.md
```

### 对于两种都需要的情况

```yaml
# 既有快捷命令，又有自动发现能力

# Slash Command: 快速访问
.claude/commands/review.md
.claude/commands/commit.md

# Skill: 自动发现
.claude/skills/code-review/SKILL.md        # 可让用户自然问
.claude/skills/commit-helper/SKILL.md     # 自动处理相关问题
```

这样用户既可以：
- `/review` 快速执行
- 也可以问 "帮我审查代码"，Claude 自动用 Skill

两种方式都可用，灵活应对不同场景。
