# Level 5: 复杂 Skill（多文件）

## 学习目标

- 掌握多文件 Skill 的结构设计
- 学会组织辅助文件（参考文档、脚本、模板）
- 理解渐进式上下文加载
- 构建完整的专业 Skill

## 核心概念

### 多文件 Skill 结构

```
my-skill/
├── SKILL.md           # 必须: 入口文件
├── REFERENCE.md       # 可选: 参考文档
├── EXAMPLES.md        # 可选: 示例集合
├── CHECKLIST.md       # 可选: 检查清单
├── scripts/           # 可选: 脚本目录
│   ├── validate.py
│   └── generate.sh
└── templates/         # 可选: 模板目录
    ├── template1.txt
    └── template2.txt
```

### 渐进式加载

Claude 不会一次性加载所有文件。SKILL.md 是入口，其他文件按需加载：

1. Claude 首先读取 SKILL.md
2. 根据需要，通过链接引用读取其他文件
3. 只加载当前任务需要的内容

### 文件引用方式

在 SKILL.md 中引用其他文件：

```markdown
详细的安全检查清单请参考 [SECURITY.md](SECURITY.md)。

运行验证脚本:
```bash
python scripts/validate.py input.txt
```
```

## 本级别示例

### 1. code-review - 完整代码审查 Skill

**目录结构**:
```
code-review/
├── SKILL.md
├── SECURITY.md      # 安全检查清单
├── PERFORMANCE.md   # 性能检查要点
├── STYLE.md         # 代码风格指南
└── CHECKLIST.md     # 综合检查清单
```

### 2. api-designer - API 设计 Skill

**目录结构**:
```
api-designer/
├── SKILL.md
├── REST-GUIDE.md    # REST API 设计规范
├── NAMING.md        # 命名规范
├── ERRORS.md        # 错误处理规范
└── templates/
    ├── openapi.yaml
    └── response.json
```

### 3. doc-generator - 文档生成 Skill

**目录结构**:
```
doc-generator/
├── SKILL.md
├── STYLE-GUIDE.md   # 文档风格指南
├── templates/
│   ├── api-doc.md
│   ├── readme.md
│   └── changelog.md
└── examples/
    ├── good-doc.md
    └── bad-doc.md
```

## 完整示例：code-review Skill

### SKILL.md

```yaml
---
name: code-review
description: 全面的代码审查。当用户需要审查代码、代码 review、PR 审查、或代码质量检查时使用。
allowed-tools: Read, Grep, Glob
---

# Code Review

## Overview

这是一个全面的代码审查 Skill，涵盖：
- 代码质量
- 安全性
- 性能
- 可维护性

## Workflow

1. 首先理解代码的功能和上下文
2. 按照检查清单逐项审查
3. 按严重程度对问题分类
4. 提供具体的改进建议

## 审查维度

### 安全性
详见 [SECURITY.md](SECURITY.md)

### 性能
详见 [PERFORMANCE.md](PERFORMANCE.md)

### 代码风格
详见 [STYLE.md](STYLE.md)

### 综合检查
详见 [CHECKLIST.md](CHECKLIST.md)

## 输出格式

### 摘要
- 总体评价: [优秀/良好/需要改进/需要重写]
- 发现问题: X 个严重 / X 个警告 / X 个建议

### 详细问题列表
按优先级排序...

### 改进建议
...
```

### SECURITY.md

```markdown
# 安全审查清单

## 输入验证
- [ ] 用户输入是否经过验证
- [ ] 是否有 SQL 注入风险
- [ ] 是否有 XSS 风险
- [ ] 是否有命令注入风险

## 认证授权
- [ ] 认证逻辑是否正确
- [ ] 权限检查是否完整
- [ ] 敏感操作是否有二次确认

## 敏感数据
- [ ] 密码是否明文存储
- [ ] 是否有硬编码的密钥
- [ ] 日志中是否打印敏感信息
- [ ] 敏感数据传输是否加密

## 常见漏洞模式

### SQL 注入
```python
# ❌ 危险
query = f"SELECT * FROM users WHERE id = {user_id}"

# ✅ 安全
query = "SELECT * FROM users WHERE id = ?"
cursor.execute(query, (user_id,))
```
```

### PERFORMANCE.md

```markdown
# 性能审查要点

## 算法复杂度
- [ ] 时间复杂度是否合理
- [ ] 空间复杂度是否合理
- [ ] 是否有更优的算法

## 数据结构
- [ ] 数据结构选择是否恰当
- [ ] 是否有不必要的复制

## 数据库
- [ ] 是否有 N+1 查询问题
- [ ] 索引使用是否合理
- [ ] 是否有不必要的全表扫描

## 缓存
- [ ] 是否需要添加缓存
- [ ] 缓存策略是否合理
- [ ] 缓存失效机制

## 常见性能问题

### N+1 查询
```python
# ❌ N+1 问题
for user in users:
    orders = db.query(Order).filter(user_id=user.id)
    
# ✅ 优化后
orders = db.query(Order).filter(user_id.in_([u.id for u in users]))
```
```

## 动手练习

### 练习 1：创建你的多文件 Skill

```bash
# 创建目录结构
mkdir -p .claude/skills/my-review
cd .claude/skills/my-review

# 创建入口文件
cat > SKILL.md << 'EOF'
---
name: my-review
description: 我的代码审查助手
---

# My Review

## 检查清单
详见 [CHECKLIST.md](CHECKLIST.md)
EOF

# 创建检查清单
cat > CHECKLIST.md << 'EOF'
# 审查检查清单

## 基础检查
- [ ] 代码能编译通过
- [ ] 没有明显的 bug
- [ ] 有必要的注释

## 进阶检查
- [ ] 错误处理完整
- [ ] 边界情况考虑
EOF
```

### 练习 2：添加脚本支持

```bash
mkdir -p .claude/skills/my-review/scripts

cat > .claude/skills/my-review/scripts/count-lines.sh << 'EOF'
#!/bin/bash
# 统计代码行数
find . -name "*.py" -o -name "*.js" | xargs wc -l
EOF

chmod +x .claude/skills/my-review/scripts/count-lines.sh
```

### 练习 3：添加模板

```bash
mkdir -p .claude/skills/my-review/templates

cat > .claude/skills/my-review/templates/review-report.md << 'EOF'
# 代码审查报告

## 项目信息
- 项目: {{project}}
- 日期: {{date}}
- 审查人: {{reviewer}}

## 审查结果

### 总体评价
{{overall}}

### 发现的问题
{{issues}}

### 改进建议
{{suggestions}}
EOF
```

## 最佳实践

### 1. 入口清晰
SKILL.md 应该提供清晰的导航，让 Claude 知道何时查阅哪个文件。

### 2. 文件职责单一
每个辅助文件专注于一个方面：
- SECURITY.md → 只讲安全
- PERFORMANCE.md → 只讲性能

### 3. 使用链接引用
```markdown
更多细节请参考 [DETAILS.md](DETAILS.md)
```

### 4. 脚本要有执行权限
```bash
chmod +x scripts/*.sh
chmod +x scripts/*.py
```

### 5. 模板使用占位符
```
{{变量名}}
```

## 何时使用多文件 Skill

| 场景 | 推荐 |
|------|------|
| 简单任务 | 单文件 SKILL.md |
| 需要检查清单 | 添加 CHECKLIST.md |
| 需要参考文档 | 添加 REFERENCE.md |
| 需要执行脚本 | 添加 scripts/ 目录 |
| 需要生成内容 | 添加 templates/ 目录 |
| 复杂专业领域 | 完整多文件结构 |

## 总结

你已经学完了 Skills 和 Slash Commands 的全部内容！

### 能力对比

| | Slash Command | 单文件 Skill | 多文件 Skill |
|---|---|---|---|
| 复杂度 | 低 | 中 | 高 |
| 触发方式 | 手动 `/xxx` | 自动发现 | 自动发现 |
| 文件数量 | 1 | 1 | 多个 |
| 适用场景 | 快捷提示 | 简单能力 | 专业领域 |

### 下一步

1. 回顾所有示例，选择一个实际项目应用
2. 根据你的工作流程，设计自己的 Skill
3. 与团队分享，收集反馈并迭代
