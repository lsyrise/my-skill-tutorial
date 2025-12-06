# Level 1: 简单 Slash Command

## 学习目标

- 理解 Slash Command 的基本结构
- 学会创建最简单的命令
- 理解项目级与用户级命令的区别

## 核心概念

### 什么是 Slash Command？

Slash Command 是一种**用户主动调用**的快捷提示。通过在 Claude Code 中输入 `/命令名`，Claude 会读取对应的 Markdown 文件内容作为提示。

### 最简结构

```
.claude/commands/
└── 命令名.md       ← 文件名去掉 .md 就是命令名
```

文件内容就是你想让 Claude 执行的提示词。

### 存储位置

| 类型 | 路径 | 说明 |
|------|------|------|
| 项目级 | `.claude/commands/` | 团队共享，随项目提交 git |
| 用户级 | `~/.claude/commands/` | 个人专用，跨所有项目 |

## 本级别示例

### 1. review.md - 代码审查

最基础的命令，直接执行一个固定的任务。

```markdown
审查这段代码，找出：
1. 潜在的 bug
2. 性能问题
3. 代码风格问题
4. 可改进的地方

用中文回复。
```

**使用方式**: `/review`

### 2. explain.md - 代码解释

```markdown
用通俗易懂的中文解释这段代码：
1. 它做了什么
2. 为什么这样写
3. 关键的技术点

适合给初学者理解的方式来解释。
```

**使用方式**: `/explain`

### 3. todo.md - 添加 TODO

```markdown
扫描当前文件，为缺少注释的重要逻辑添加 TODO 注释。

TODO 格式: // TODO: [描述需要做什么]

只在真正需要补充的地方添加。
```

**使用方式**: `/todo`

### 4. optimize.md - 性能优化

```markdown
分析这段代码的性能：
1. 找出性能瓶颈
2. 提出优化建议
3. 给出优化后的代码示例

重点关注：时间复杂度、空间复杂度、算法选择。
```

**使用方式**: `/optimize`

## 动手练习

### 练习 1：创建你的第一个命令

1. 在你的项目中创建命令目录：
   ```bash
   mkdir -p .claude/commands
   ```

2. 创建一个简单的命令：
   ```bash
   echo "总结这段代码的功能，用一句话概括。" > .claude/commands/summary.md
   ```

3. 在 Claude Code 中测试：
   ```
   > /summary
   ```

### 练习 2：创建个人命令

1. 创建用户级命令目录：
   ```bash
   mkdir -p ~/.claude/commands
   ```

2. 创建个人常用命令：
   ```bash
   echo "用中文回复所有问题。" > ~/.claude/commands/chinese.md
   ```

3. 这个命令在所有项目中都可用！

## 进阶思考

- **Q**: 项目级和用户级命令同名会怎样？
- **A**: 不支持同名冲突，需要使用不同的命令名。

- **Q**: 如何组织多个命令？
- **A**: 可以用子目录组织，如 `.claude/commands/review/security.md` 创建 `/security` 命令，显示为 "(project:review)"。

## 下一步

学习 [Level-2: 带参数的命令](../Level-2-带参数命令/README.md)，让命令更灵活！
