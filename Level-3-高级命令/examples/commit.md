---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git diff:*), Bash(git log:*), Bash(git branch:*)
argument-hint: [提交信息]
description: 创建一个规范的 git 提交
---

## 当前状态

- Git 状态: !`git status --short`
- 暂存区变更: !`git diff --staged --stat`
- 当前分支: !`git branch --show-current`
- 最近提交: !`git log --oneline -5`

## 任务

根据以上变更信息，创建一个 git 提交。

用户提供的提交信息: $ARGUMENTS

## 操作步骤

1. 如果有未暂存的变更，询问是否需要暂存
2. 如果未提供提交信息，根据变更内容自动生成
3. 执行提交

## 提交信息规范

- 格式: `type(scope): description`
- 类型:
  - feat: 新功能
  - fix: 修复 bug
  - docs: 文档更新
  - style: 代码格式调整
  - refactor: 重构
  - test: 测试相关
  - chore: 构建/工具相关
- 描述用中文
- 首行不超过 50 字符
