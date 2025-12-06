---
name: api-designer
description: 设计 RESTful API。当用户需要设计 API、创建接口、规划 API 结构、或问 API 应该怎么设计时使用。
---

# API Designer Skill

## Overview

帮助设计符合 REST 规范的 API，提供一致、直观、易用的接口设计。

## 适用场景

- 新系统的 API 设计
- 现有 API 的重构
- API 文档生成
- 接口规范制定

## Workflow

1. **理解需求**: 确认 API 服务的业务目标
2. **资源识别**: 识别核心资源和关系
3. **端点设计**: 设计 URL 结构
4. **方法定义**: 定义 HTTP 方法和语义
5. **数据格式**: 设计请求/响应格式
6. **错误处理**: 定义错误码和消息

## 设计规范

### REST 原则
详见 [REST-GUIDE.md](REST-GUIDE.md)

### 命名规范
详见 [NAMING.md](NAMING.md)

### 错误处理
详见 [ERRORS.md](ERRORS.md)

## 输出格式

### API 设计文档

```yaml
# 资源名称
GET /api/v1/resources
POST /api/v1/resources
GET /api/v1/resources/{id}
PUT /api/v1/resources/{id}
DELETE /api/v1/resources/{id}
```

### 详细规格

对每个端点提供：
- 路径和方法
- 请求参数
- 请求体格式
- 响应格式
- 错误码
- 示例
