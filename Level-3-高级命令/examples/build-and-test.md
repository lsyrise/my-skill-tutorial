---
allowed-tools: Bash(npm:*), Bash(yarn:*), Bash(make:*), Bash(cmake:*), Bash(cargo:*), Read
description: 构建项目并运行测试
---

## 项目检测

检测项目类型：

- package.json: !`ls package.json 2>/dev/null && echo "存在" || echo "不存在"`
- Makefile: !`ls Makefile 2>/dev/null && echo "存在" || echo "不存在"`
- CMakeLists.txt: !`ls CMakeLists.txt 2>/dev/null && echo "存在" || echo "不存在"`
- Cargo.toml: !`ls Cargo.toml 2>/dev/null && echo "存在" || echo "不存在"`

## 任务

1. 根据检测到的项目类型，选择合适的构建工具
2. 运行构建命令
3. 运行测试命令
4. 报告构建和测试结果

## 支持的项目类型

| 类型 | 构建命令 | 测试命令 |
|------|----------|----------|
| npm | `npm run build` | `npm test` |
| yarn | `yarn build` | `yarn test` |
| make | `make` | `make test` |
| cmake | `cmake --build .` | `ctest` |
| cargo | `cargo build` | `cargo test` |

## 错误处理

如果构建或测试失败：
1. 分析错误输出
2. 识别错误原因
3. 提供解决方案
4. 给出修复建议
