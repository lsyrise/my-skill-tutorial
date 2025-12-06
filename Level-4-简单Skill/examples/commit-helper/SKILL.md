---
name: commit-helper
description: 生成清晰的 git 提交信息。当用户需要提交代码、写 commit message、或审查暂存的变更时使用。
---

# Commit Helper

## Instructions

1. 运行 `git diff --staged` 查看暂存的变更
2. 分析变更内容，理解改动的目的
3. 生成符合规范的提交信息

## 提交信息格式

```
type(scope): subject

body (可选)

footer (可选)
```

## 提交类型

| 类型 | 说明 |
|------|------|
| feat | 新功能 |
| fix | Bug 修复 |
| docs | 文档更新 |
| style | 代码格式（不影响功能） |
| refactor | 重构（不是新功能也不是修复） |
| test | 测试相关 |
| chore | 构建过程或辅助工具的变动 |

## 最佳实践

1. **首行简短**: 不超过 50 字符
2. **使用现在时态**: "Add feature" 而不是 "Added feature"
3. **解释 What 和 Why**: 不仅说做了什么，还要说为什么
4. **每个提交只做一件事**: 保持原子性

## 示例

```
feat(player): 添加玩家背包系统

- 实现背包物品的增删改查
- 支持物品堆叠和拆分
- 添加背包容量限制

Closes #123
```
