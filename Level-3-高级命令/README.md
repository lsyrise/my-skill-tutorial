# Level 3: 高级 Slash Command

## 学习目标

- 掌握 YAML Frontmatter 配置
- 学会使用 `allowed-tools` 控制工具权限
- 学会使用 `!` 前缀执行 Bash 命令
- 了解其他高级特性

## 核心概念

### Frontmatter 结构

Frontmatter 是 Markdown 文件开头的 YAML 配置块，用 `---` 包裹：

```yaml
---
allowed-tools: Bash(git:*), Read, Write
argument-hint: [message]
description: 创建 git 提交
model: claude-3-5-haiku-20241022
disable-model-invocation: false
---

命令内容...
```

### Frontmatter 字段说明

| 字段 | 说明 | 默认值 |
|------|------|--------|
| `allowed-tools` | 命令可使用的工具列表 | 继承对话权限 |
| `argument-hint` | 参数提示，用于自动补全 | 无 |
| `description` | 命令描述，显示在 `/help` | 使用第一行内容 |
| `model` | 指定使用的模型 | 继承对话模型 |
| `disable-model-invocation` | 禁止 Claude 主动调用此命令 | false |

### Bash 命令执行

使用 `!` 前缀可以在命令运行前执行 Bash 命令，输出会包含在上下文中：

```markdown
当前 git 状态: !`git status`
当前分支: !`git branch --show-current`
```

**重要**: 使用 Bash 执行时，必须在 `allowed-tools` 中声明 Bash 权限。

## 本级别示例

### 1. commit.md - Git 提交（完整示例）

```yaml
---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
argument-hint: [提交信息]
description: 创建一个规范的 git 提交
---

## 当前状态

- Git 状态: !`git status`
- 暂存区 diff: !`git diff --staged`
- 当前分支: !`git branch --show-current`
- 最近提交: !`git log --oneline -5`

## 任务

根据以上变更信息，创建一个 git 提交。

提交信息: $ARGUMENTS

如果未提供提交信息，请根据变更内容自动生成一个符合规范的提交信息。

提交规范：
- 类型: feat/fix/docs/style/refactor/test/chore
- 格式: type(scope): description
- 描述用中文
```

**使用方式**: `/commit` 或 `/commit 修复登录bug`

### 2. security-check.md - 安全检查

```yaml
---
allowed-tools: Read, Grep, Glob
argument-hint: [文件或目录]
description: 对代码进行安全审查
---

## 扫描目标

目标: @$1

如果未指定目标，扫描当前目录下的所有代码文件。

## 安全检查清单

1. SQL 注入风险
2. XSS 漏洞
3. 硬编码的密钥或密码
4. 不安全的 API 调用
5. 权限控制问题
6. 敏感数据暴露

## 输出格式

按严重程度排序：
- 🔴 严重: 必须立即修复
- 🟡 警告: 建议修复
- 🟢 建议: 可以优化
```

**使用方式**: `/security-check src/`

### 3. build-and-test.md - 构建测试

```yaml
---
allowed-tools: Bash(npm:*), Bash(yarn:*), Bash(make:*)
description: 构建项目并运行测试
---

## 项目信息

- package.json: !`cat package.json 2>/dev/null || echo "无 package.json"`
- Makefile: !`cat Makefile 2>/dev/null | head -20 || echo "无 Makefile"`

## 任务

1. 检测项目类型（npm/yarn/make）
2. 运行构建命令
3. 运行测试
4. 报告结果

如果构建或测试失败，分析错误原因并提供解决方案。
```

**使用方式**: `/build-and-test`

### 4. analyze-deps.md - 依赖分析

```yaml
---
allowed-tools: Read, Bash(npm:*), Bash(pip:*)
argument-hint: [check|update|security]
description: 分析项目依赖
---

## 依赖信息

- package.json: !`cat package.json 2>/dev/null || echo "无"`
- requirements.txt: !`cat requirements.txt 2>/dev/null || echo "无"`
- go.mod: !`cat go.mod 2>/dev/null || echo "无"`

## 任务

操作类型: $1

- check: 检查依赖版本和兼容性
- update: 建议可以更新的依赖
- security: 检查已知的安全漏洞

如果未指定操作类型，执行全部检查。
```

**使用方式**: `/analyze-deps security`

## allowed-tools 详解

### 工具权限语法

```yaml
# 允许特定工具
allowed-tools: Read, Write, Grep

# 允许 Bash 执行特定命令
allowed-tools: Bash(git:*)         # 允许所有 git 命令
allowed-tools: Bash(npm run:*)     # 允许所有 npm run 命令
allowed-tools: Bash(ls:*), Bash(cat:*)  # 允许 ls 和 cat

# 组合使用
allowed-tools: Read, Write, Bash(git add:*), Bash(git commit:*)
```

### 常用工具列表

| 工具 | 说明 |
|------|------|
| `Read` | 读取文件 |
| `Write` | 写入文件 |
| `Grep` | 搜索文件内容 |
| `Glob` | 查找文件 |
| `Bash(命令:*)` | 执行特定 Bash 命令 |

## 动手练习

### 练习 1：创建 Git 提交命令

```bash
cat > .claude/commands/gc.md << 'EOF'
---
allowed-tools: Bash(git add:*), Bash(git commit:*), Bash(git status:*)
description: 快速 git 提交
---

状态: !`git status --short`

暂存所有变更并提交。
提交信息: $ARGUMENTS

如果未提供信息，根据变更自动生成。
EOF
```

### 练习 2：创建代码统计命令

```bash
cat > .claude/commands/stats.md << 'EOF'
---
allowed-tools: Bash(wc:*), Bash(find:*), Bash(cloc:*)
description: 统计代码信息
---

代码行数: !`find . -name "*.cpp" -o -name "*.h" | xargs wc -l 2>/dev/null || echo "无"`

分析代码库的统计信息。
EOF
```

## 模型选择

使用 `model` 字段可以为特定命令指定模型：

```yaml
---
model: claude-3-5-haiku-20241022   # 快速任务用 Haiku
---
```

或

```yaml
---
model: claude-sonnet-4-20250514    # 复杂任务用 Sonnet
---
```

## 禁止自动调用

如果不希望 Claude 主动调用某个命令（只允许用户手动调用）：

```yaml
---
disable-model-invocation: true
---
```

## 下一步

学习 [Level-4: 简单 Skill](../Level-4-简单Skill/README.md)，进入自动发现的世界！
