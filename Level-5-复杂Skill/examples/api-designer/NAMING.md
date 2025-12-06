# API 命名规范

## 1. URL 命名

### 基本规则

| 规则 | 正确 ✅ | 错误 ❌ |
|------|---------|---------|
| 使用名词复数 | `/users` | `/user`, `/getUsers` |
| 使用小写 | `/users` | `/Users`, `/USERS` |
| 使用连字符分隔 | `/order-items` | `/orderItems`, `/order_items` |
| 不含文件扩展名 | `/users/123` | `/users/123.json` |

### 资源命名

```
# 基础资源
/users
/products
/orders

# 子资源
/users/{userId}/orders
/products/{productId}/reviews

# 复合词
/shopping-carts      ✅
/shoppingCarts       ❌
/shopping_carts      ❌
```

## 2. 参数命名

### 路径参数
使用驼峰命名法：

```
/users/{userId}
/orders/{orderId}/items/{itemId}
```

### 查询参数
使用下划线命名法：

```
GET /users?page_size=20&sort_by=created_at&is_active=true
```

或驼峰命名法（保持一致即可）：

```
GET /users?pageSize=20&sortBy=createdAt&isActive=true
```

## 3. 请求/响应字段命名

### 推荐：驼峰命名法 (camelCase)

```json
{
  "userId": 123,
  "userName": "张三",
  "emailAddress": "zhang@example.com",
  "createdAt": "2024-01-01T00:00:00Z",
  "isActive": true,
  "orderCount": 5
}
```

### 替代：下划线命名法 (snake_case)

```json
{
  "user_id": 123,
  "user_name": "张三",
  "email_address": "zhang@example.com",
  "created_at": "2024-01-01T00:00:00Z",
  "is_active": true,
  "order_count": 5
}
```

**重要**: 选择一种风格后保持全局一致。

## 4. 常用字段命名

### 标识符
- `id` - 主键
- `userId`, `orderId` - 外键

### 时间戳
- `createdAt` - 创建时间
- `updatedAt` - 更新时间
- `deletedAt` - 删除时间

### 状态
- `status` - 状态码 (PENDING, ACTIVE, COMPLETED)
- `isActive`, `isEnabled` - 布尔状态

### 分页
- `page` - 页码
- `limit` 或 `pageSize` - 每页数量
- `total` - 总数
- `offset` - 偏移量

### 排序
- `sortBy` - 排序字段
- `order` 或 `sortOrder` - 排序方向 (asc/desc)

### 过滤
- `q` 或 `query` - 搜索关键词
- `filter` - 过滤条件

## 5. 版本号命名

```
/api/v1/users    ✅ 推荐
/api/1.0/users   ❌ 避免
```

## 6. 错误字段命名

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "请求参数验证失败",
    "details": [
      {
        "field": "email",
        "message": "邮箱格式不正确"
      }
    ]
  }
}
```

## 7. 常见命名对照表

| 概念 | 推荐命名 |
|------|----------|
| 用户 | user, users |
| 订单 | order, orders |
| 商品 | product, products |
| 购物车 | cart, shopping-cart |
| 收藏 | favorite, favorites |
| 评论 | comment, review |
| 消息 | message, notification |
| 文件 | file, attachment |
| 标签 | tag, label |
| 分类 | category, categories |
