# 实战演示：从创建到调用 Skill

## 第一步：创建一个简单的 Skill

```bash
# 创建 Skill 目录
mkdir -p .claude/skills/quick-summary

# 创建 SKILL.md
cat > .claude/skills/quick-summary/SKILL.md << 'EOF'
---
name: quick-summary
description: 快速总结内容。当用户需要摘要、总结、概括信息时使用。
---

# Quick Summary

## Instructions

用户需要你总结他提供的内容。

按以下步骤执行：

1. 识别关键点
2. 提取最重要的信息
3. 用 5 句话以内总结
4. 结构清晰，逻辑顺序

## 输出格式

**核心要点:**
- 点1
- 点2
- 点3

**简短总结:** 
一段话概括
EOF
```

## 第二步：Claude 如何发现这个 Skill

**时间**: Claude Code 启动时

```
Claude Code 启动
    ↓
扫描 ~/.claude/skills/ 和 .claude/skills/
    ↓
找到 quick-summary/ 目录
    ↓
读取 SKILL.md 的 frontmatter
    ↓
提取:
  - name: "quick-summary"
  - description: "快速总结内容。当用户需要摘要、总结、概括信息时使用。"
    ↓
存储在内存中：
{
  "quick-summary": {
    "description": "快速总结内容。当用户需要摘要、总结、概括信息时使用。",
    "file": ".claude/skills/quick-summary/SKILL.md"
  }
}
```

## 第三步：用户提问时的调用流程

### 用户输入 ①：精确匹配

```
👤 用户:
  我有这样一篇文章，能帮我总结一下吗？
  [用户粘贴了长篇文章]
```

**Claude 内部处理**:

```
收到消息: "总结一下"

获取 Skill 列表:
- quick-summary (description: "快速总结内容。当用户需要摘要、总结、概括信息时使用。")
- code-explainer (description: "...")
- bug-finder (description: "...")

计算相似度:
- "总结" vs "摘要、总结、概括" → 99% 匹配 ⭐⭐⭐
- "总结" vs "explain code" → 5% 匹配
- "总结" vs "bug" → 2% 匹配

选择: quick-summary (最高相似度)

加载 SKILL.md 到上下文:
[读取 .claude/skills/quick-summary/SKILL.md 的全部内容]

执行:
按照 Instructions 来处理用户的总结需求
```

**Claude 的回答**:

```
🤖 Claude:
【使用了 quick-summary Skill】

我来帮你总结这篇文章。

核心要点:
- 要点 1
- 要点 2
- 要点 3

简短总结:
这篇文章主要讲...
```

### 用户输入 ②：间接匹配

```
👤 用户:
  能给我一个概括吗？
```

**Claude 的思考**:

```
"概括" 的语义 ≈ "总结"

匹配 quick-summary: 95%

执行 Skill...
```

### 用户输入 ③：不使用 Skill

```
👤 用户:
  这段代码有什么问题
```

**Claude 的思考**:

```
"问题" 的语义与 "总结" 无关

匹配 quick-summary: 5%

改而匹配 bug-finder: 95%

执行 bug-finder Skill
```

---

## 第四步：观察 Skill 是否被使用

### 方法 1：看 Claude 的行为

创建 Skill 后，用相关关键词提问：

```
提问: "帮我总结一下这段话"

观察:
- ✅ 如果 Claude 的回答风格符合 SKILL.md 的 Instructions
- ✅ 如果使用了 Instructions 中指定的格式
- ❌ 如果回答风格与 Instructions 不符，说明 Skill 没被使用
```

### 方法 2：调试模式

```bash
# 运行调试模式
claude --debug

# 观察日志中关于 Skill 加载的信息
```

---

## 第五步：Skill 调用的完整时间线

```
┌─ 启动 Claude Code ─────┐
│  [扫描并加载所有 Skill]  │
└────────────┬────────────┘
             │
             ↓
    ┌─ 用户提问 ──────────┐
    │ "帮我总结一下..."    │
    └────────┬─────────────┘
             │
             ↓
    ┌─ 语义匹配 ──────────┐
    │ 计算与所有 Skill    │
    │ description 的相似度 │
    └────────┬─────────────┘
             │
             ↓
    ┌─ 选择最佳匹配 ──────┐
    │ quick-summary: 99%  │
    │ code-explainer: 5%  │
    │ bug-finder: 2%      │
    └────────┬─────────────┘
             │
             ↓
    ┌─ 加载 Skill ───────┐
    │ 读取 SKILL.md      │
    │ 注入到上下文       │
    └────────┬─────────────┘
             │
             ↓
    ┌─ 执行 Instructions ┐
    │ 按 Skill 的方式    │
    │ 处理用户请求       │
    └────────┬─────────────┘
             │
             ↓
    ┌─ 返回结果 ────────┐
    │ 用户看到的回答    │
    └────────────────────┘
```

---

## 第六步：不同的 Skill 一起工作

假设有多个 Skill：

```yaml
# Skill 1: quick-summary
description: 快速总结、摘要、概括内容

# Skill 2: detailed-analysis  
description: 深度分析、详细讲解、原理解释

# Skill 3: code-explainer
description: 解释代码、程序讲解、代码原理

# Skill 4: bug-finder
description: 查找问题、排查错误、代码有问题
```

### 场景 1：用户要总结

```
👤 用户: "总结一下这段代码"

相似度分析:
- quick-summary: 85% (有 "总结")
- code-explainer: 90% (有 "代码")  ⭐ 选择
- detailed-analysis: 50%
- bug-finder: 10%

Claude 选择: code-explainer (最高)
```

### 场景 2：用户要分析

```
👤 用户: "分析一下这段代码"

相似度分析:
- detailed-analysis: 95% ⭐ 选择
- code-explainer: 80%
- quick-summary: 30%
- bug-finder: 10%

Claude 选择: detailed-analysis
```

### 场景 3：用户要找 bug

```
👤 用户: "这段代码有什么问题"

相似度分析:
- bug-finder: 98% ⭐ 选择
- code-explainer: 20%
- detailed-analysis: 15%
- quick-summary: 5%

Claude 选择: bug-finder
```

---

## 关键洞察：Language Understanding

Claude 的 Skill 匹配是基于 **深度语言理解**，而不是关键字匹配。

### 同义表达都能匹配

```
description: "总结内容"

以下所有表达都能触发:
- "总结一下"
- "给我个摘要"
- "概括一下"
- "提炼关键点"
- "浓缩一下"
- "给个简要版本"
- "打个比方说"
```

### 模糊表达也能匹配

```
description: "查找代码中的 bug"

以下表达都能触发:
- "这里有问题"      (问题 ≈ bug)
- "程序崩溃了"      (崩溃 ≈ 有 bug)
- "为什么不对"      (不对 ≈ 有问题)
- "哪里出错了"      (出错 ≈ bug)
```

---

## 实战：优化你的 Skill 描述

### 现状评估

你的 Skill 能被 Claude 发现的程度取决于：

```
发现成功率 = description 质量 × Claude 的理解能力
```

### 测试方法

```bash
# 1. 创建 Skill
mkdir -p .claude/skills/test-skill
cat > .claude/skills/test-skill/SKILL.md << 'EOF'
---
name: test-skill
description: [你的描述]
---

# Test

## Instructions
执行指令
EOF

# 2. 重启 Claude Code
# Cmd+K 退出，重新启动

# 3. 用不同的表达提问，观察是否触发
# - 使用完全相同的词
# - 使用同义词
# - 使用模糊表达
# - 用其他语言

# 4. 如果没有触发，调整 description 并重试
```

### 优化案例

```yaml
# 版本 1 - 不够好
description: 帮助处理数据

# 测试: 提问 "处理数据" ❌ 没有触发
# 分析: description 太模糊

# 版本 2 - 改进
description: 处理 Excel 和 CSV 文件，清洗数据，生成统计报表

# 测试: 提问 "处理数据" ✅ 触发
# 测试: 提问 "Excel 分析" ✅ 触发  
# 测试: 提问 "数据清洗" ✅ 触发
```

---

## 调用机制总结

| 环节 | 发生的事 | 时间 |
|------|---------|------|
| 扫描 | 读取所有 SKILL.md | 启动时 |
| 存储 | 把 name 和 description 存入内存 | 启动时 |
| 接收 | 用户提问 | 实时 |
| 匹配 | Claude 计算相似度 | 实时 |
| 加载 | 读取最匹配的 SKILL.md | 实时 |
| 执行 | 按 Instructions 处理 | 实时 |
| 返回 | 用户看到结果 | 实时 |

---

## 常见问题

**Q: Skill 会影响速度吗？**
A: 不会。Claude 只加载匹配的 Skill，未匹配的 Skill 不会被读取。

**Q: 有多少个 Skill 时会出问题？**
A: 几百个 Skill 都没问题。关键是 description 的质量。

**Q: 怎么强制使用某个 Skill？**
A: 用 Slash Commands。Skill 本来就是自动发现的。

**Q: 同时匹配多个 Skill 怎么办？**
A: Claude 选择相似度最高的那个。如果你有歧义，调整 description。

**Q: Skill 不工作，怎么办？**
A: 
1. 检查 YAML 语法
2. 检查文件路径是否正确
3. 重启 Claude Code
4. 检查 description 是否包含用户会说的关键词
5. 用 `claude --debug` 查看加载日志
