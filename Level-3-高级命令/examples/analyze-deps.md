---
allowed-tools: Read, Bash(npm:*), Bash(pip:*), Bash(cargo:*)
argument-hint: [check|update|security]
description: 分析项目依赖状态
---

## 依赖文件检测

- package.json: !`cat package.json 2>/dev/null | head -30 || echo "无"`
- requirements.txt: !`cat requirements.txt 2>/dev/null | head -20 || echo "无"`
- Cargo.toml: !`cat Cargo.toml 2>/dev/null | head -20 || echo "无"`

## 操作类型

请求的操作: $1

### 支持的操作

1. **check** - 检查依赖版本和兼容性
   - 列出所有依赖及版本
   - 检查版本兼容性
   - 识别潜在冲突

2. **update** - 建议可更新的依赖
   - 列出过时的依赖
   - 显示最新可用版本
   - 评估更新风险

3. **security** - 检查安全漏洞
   - 扫描已知漏洞
   - 按严重程度排序
   - 提供修复建议

如果未指定操作类型，执行全部检查。

## 输出格式

### 依赖概览
- 总依赖数: X
- 直接依赖: X
- 开发依赖: X

### 问题清单
按优先级列出发现的问题，包含：
- 依赖名称
- 当前版本
- 问题描述
- 建议操作
