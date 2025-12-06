# REST API 设计指南

## 1. REST 核心原则

### 统一接口
- 使用标准 HTTP 方法
- 资源通过 URI 标识
- 通过表示操作资源

### 无状态
- 每个请求包含所有必要信息
- 服务器不保存客户端状态
- 使用 Token 而非 Session

### 可缓存
- 响应应标明是否可缓存
- 使用适当的 Cache-Control 头

## 2. HTTP 方法语义

| 方法 | 语义 | 幂等性 | 安全性 |
|------|------|--------|--------|
| GET | 读取资源 | ✅ 是 | ✅ 是 |
| POST | 创建资源 | ❌ 否 | ❌ 否 |
| PUT | 完整更新 | ✅ 是 | ❌ 否 |
| PATCH | 部分更新 | ❌ 否 | ❌ 否 |
| DELETE | 删除资源 | ✅ 是 | ❌ 否 |

### 使用示例

```http
# 获取用户列表
GET /api/v1/users

# 创建用户
POST /api/v1/users
Content-Type: application/json
{"name": "张三", "email": "zhang@example.com"}

# 获取单个用户
GET /api/v1/users/123

# 完整更新用户
PUT /api/v1/users/123
Content-Type: application/json
{"name": "张三", "email": "new@example.com", "age": 25}

# 部分更新用户
PATCH /api/v1/users/123
Content-Type: application/json
{"email": "new@example.com"}

# 删除用户
DELETE /api/v1/users/123
```

## 3. URL 设计

### 基本规则
- 使用名词复数: `/users` 而非 `/user`
- 使用小写: `/users` 而非 `/Users`
- 使用连字符: `/order-items` 而非 `/orderItems`
- 避免动词: `/users` 而非 `/getUsers`

### 资源层级

```
# 单一资源
/users
/users/{id}

# 子资源
/users/{id}/orders
/users/{id}/orders/{orderId}

# 避免过深嵌套（不超过 3 层）
# ❌ 不推荐
/users/{id}/orders/{orderId}/items/{itemId}/reviews

# ✅ 推荐
/order-items/{itemId}/reviews
```

### 查询参数

```http
# 分页
GET /users?page=1&limit=20

# 排序
GET /users?sort=created_at&order=desc

# 过滤
GET /users?status=active&role=admin

# 字段选择
GET /users?fields=id,name,email

# 搜索
GET /users?q=张三
```

## 4. 版本控制

### 推荐方式：URL 路径

```
/api/v1/users
/api/v2/users
```

### 其他方式

```http
# Header 方式
Accept: application/vnd.myapi.v1+json

# Query 方式
/users?version=1
```

## 5. 响应格式

### 成功响应

```json
// 单个资源
{
  "data": {
    "id": 123,
    "name": "张三",
    "email": "zhang@example.com"
  }
}

// 列表资源
{
  "data": [
    {"id": 1, "name": "张三"},
    {"id": 2, "name": "李四"}
  ],
  "meta": {
    "total": 100,
    "page": 1,
    "limit": 20
  }
}
```

### HTTP 状态码

| 状态码 | 含义 | 使用场景 |
|--------|------|----------|
| 200 | OK | GET 成功 |
| 201 | Created | POST 创建成功 |
| 204 | No Content | DELETE 成功 |
| 400 | Bad Request | 请求格式错误 |
| 401 | Unauthorized | 未认证 |
| 403 | Forbidden | 无权限 |
| 404 | Not Found | 资源不存在 |
| 409 | Conflict | 资源冲突 |
| 422 | Unprocessable Entity | 验证失败 |
| 500 | Internal Server Error | 服务器错误 |

## 6. HATEOAS (可选)

```json
{
  "data": {
    "id": 123,
    "name": "张三"
  },
  "links": {
    "self": "/users/123",
    "orders": "/users/123/orders",
    "delete": "/users/123"
  }
}
```
