# API 错误处理规范

## 1. 错误响应格式

### 标准格式

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "人类可读的错误描述",
    "details": []
  }
}
```

### 详细格式（含验证错误）

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "请求参数验证失败",
    "details": [
      {
        "field": "email",
        "code": "INVALID_FORMAT",
        "message": "邮箱格式不正确"
      },
      {
        "field": "password",
        "code": "TOO_SHORT",
        "message": "密码长度不能少于8位"
      }
    ]
  }
}
```

## 2. HTTP 状态码使用

### 4xx 客户端错误

| 状态码 | 名称 | 使用场景 |
|--------|------|----------|
| 400 | Bad Request | 请求格式错误、参数类型错误 |
| 401 | Unauthorized | 未提供认证信息、Token 过期 |
| 403 | Forbidden | 已认证但无权访问 |
| 404 | Not Found | 资源不存在 |
| 405 | Method Not Allowed | HTTP 方法不支持 |
| 409 | Conflict | 资源冲突（如重复创建） |
| 422 | Unprocessable Entity | 请求语法正确但语义错误 |
| 429 | Too Many Requests | 请求频率超限 |

### 5xx 服务器错误

| 状态码 | 名称 | 使用场景 |
|--------|------|----------|
| 500 | Internal Server Error | 服务器内部错误 |
| 502 | Bad Gateway | 上游服务不可用 |
| 503 | Service Unavailable | 服务暂时不可用 |
| 504 | Gateway Timeout | 上游服务超时 |

## 3. 错误码设计

### 错误码格式

```
[模块]_[错误类型]

例如:
AUTH_INVALID_TOKEN
USER_NOT_FOUND
ORDER_ALREADY_PAID
```

### 常用错误码

#### 认证类 (AUTH_)
```
AUTH_REQUIRED          - 需要登录
AUTH_INVALID_TOKEN     - Token 无效
AUTH_TOKEN_EXPIRED     - Token 已过期
AUTH_INVALID_CREDENTIALS - 用户名或密码错误
```

#### 授权类 (AUTHZ_)
```
AUTHZ_FORBIDDEN        - 无权访问
AUTHZ_INSUFFICIENT_PERMISSIONS - 权限不足
```

#### 验证类 (VALIDATION_)
```
VALIDATION_ERROR       - 验证失败（通用）
VALIDATION_REQUIRED    - 字段必填
VALIDATION_INVALID_FORMAT - 格式错误
VALIDATION_TOO_LONG    - 超过长度限制
VALIDATION_TOO_SHORT   - 低于长度要求
```

#### 资源类 (RESOURCE_)
```
RESOURCE_NOT_FOUND     - 资源不存在
RESOURCE_ALREADY_EXISTS - 资源已存在
RESOURCE_CONFLICT      - 资源冲突
RESOURCE_LOCKED        - 资源被锁定
```

#### 业务类 (自定义)
```
ORDER_ALREADY_PAID     - 订单已支付
ORDER_CANNOT_CANCEL    - 订单无法取消
INVENTORY_INSUFFICIENT - 库存不足
```

## 4. 错误处理示例

### 401 Unauthorized

```json
HTTP/1.1 401 Unauthorized

{
  "error": {
    "code": "AUTH_INVALID_TOKEN",
    "message": "认证令牌无效或已过期"
  }
}
```

### 404 Not Found

```json
HTTP/1.1 404 Not Found

{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "用户不存在",
    "details": [
      {
        "field": "userId",
        "value": "123"
      }
    ]
  }
}
```

### 422 Validation Error

```json
HTTP/1.1 422 Unprocessable Entity

{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "请求参数验证失败",
    "details": [
      {
        "field": "email",
        "code": "VALIDATION_INVALID_FORMAT",
        "message": "请输入有效的邮箱地址"
      }
    ]
  }
}
```

### 429 Rate Limited

```json
HTTP/1.1 429 Too Many Requests
Retry-After: 60

{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "请求过于频繁，请稍后再试",
    "details": [
      {
        "retryAfter": 60,
        "limit": 100,
        "remaining": 0
      }
    ]
  }
}
```

## 5. 最佳实践

### ✅ 做
- 返回一致的错误格式
- 提供清晰的错误消息
- 记录详细的服务端日志
- 对敏感错误信息脱敏

### ❌ 不要做
- 暴露堆栈信息给客户端
- 返回不同结构的错误响应
- 使用模糊的错误消息
- 把所有错误都返回 500
