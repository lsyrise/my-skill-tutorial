# Level 2: 带参数的 Slash Command

## 学习目标

- 学会使用 `$ARGUMENTS` 接收所有参数
- 学会使用 `$1`, `$2`, `$3` 接收位置参数
- 学会使用 `@` 引用文件

## 核心概念

### 参数占位符

| 占位符 | 说明 | 示例 |
|--------|------|------|
| `$ARGUMENTS` | 捕获所有参数 | `/fix 123 high` → `$ARGUMENTS` = `"123 high"` |
| `$1`, `$2`, `$3`... | 位置参数 | `/review-pr 456 high alice` → `$1`=456, `$2`=high, `$3`=alice |
| `@文件路径` | 引用文件内容 | `@src/main.cpp` 会包含该文件内容 |

### 何时使用哪种参数

- **$ARGUMENTS**: 当你需要把所有参数作为一个整体处理时
- **$1, $2, $3**: 当你需要分别处理每个参数时
- **@文件**: 当你需要引用特定文件的内容时

## 本级别示例

### 1. fix-issue.md - 修复问题（使用 $ARGUMENTS）

```markdown
修复问题 #$ARGUMENTS

1. 首先理解问题描述
2. 分析相关代码
3. 提出解决方案
4. 实现修复
```

**使用方式**: `/fix-issue 123` 或 `/fix-issue 123 高优先级`

### 2. compare.md - 比较文件（使用 @文件引用）

```markdown
比较这两个文件的实现：

文件1: @$1
文件2: @$2

分析：
1. 主要差异
2. 各自优缺点
3. 推荐使用哪个
```

**使用方式**: `/compare src/old.cpp src/new.cpp`

### 3. review-pr.md - 审查 PR（使用位置参数）

```markdown
审查 PR #$1

优先级: $2
负责人: $3

审查重点：
1. 代码质量
2. 逻辑正确性
3. 性能影响
4. 测试覆盖
```

**使用方式**: `/review-pr 456 high alice`

### 4. generate.md - 生成代码（结合使用）

```markdown
根据需求生成代码：

需求描述: $ARGUMENTS

参考现有实现: @src/examples/

生成要求：
1. 符合现有代码风格
2. 包含必要注释
3. 考虑边界情况
```

**使用方式**: `/generate 实现用户登录功能`

## 文件引用详解

### 引用单个文件

```markdown
分析这个文件: @src/main.cpp
```

### 引用多个文件

```markdown
比较这两个版本：
- 旧版本: @src/old-version.js
- 新版本: @src/new-version.js
```

### 引用目录

```markdown
分析这个模块下的所有代码: @src/utils/
```

### 结合参数引用文件

```markdown
分析文件 @$1 的实现
```

**使用**: `/analyze src/player.cpp`

## 动手练习

### 练习 1：创建带参数的命令

```bash
# 创建一个翻译命令
cat > .claude/commands/translate.md << 'EOF'
将以下内容翻译成 $1：

$ARGUMENTS
EOF
```

**使用**: `/translate 英文 这是一段中文文本`

### 练习 2：创建文件比较命令

```bash
cat > .claude/commands/diff.md << 'EOF'
比较这两个文件的差异：

文件 A: @$1
文件 B: @$2

列出：
1. 新增的内容
2. 删除的内容
3. 修改的内容
EOF
```

**使用**: `/diff src/v1.cpp src/v2.cpp`

### 练习 3：创建问题追踪命令

```bash
cat > .claude/commands/issue.md << 'EOF'
问题追踪 #$1

优先级: $2
状态: $3

请根据以上信息处理这个问题。
EOF
```

**使用**: `/issue 42 高 进行中`

## 常见问题

### Q: 参数包含空格怎么办？

A: 空格会分隔参数。如果需要包含空格的参数，考虑使用 `$ARGUMENTS` 或用引号包裹。

### Q: 如何处理可选参数？

A: 可以在提示中说明："如果未提供 $2，则使用默认值 xxx"

### Q: 文件不存在会怎样？

A: Claude 会告诉你文件不存在，不会报错。

## 下一步

学习 [Level-3: 高级命令](../Level-3-高级命令/README.md)，掌握 Frontmatter 和 Bash 执行！
