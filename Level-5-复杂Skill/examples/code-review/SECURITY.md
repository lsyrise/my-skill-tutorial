# 安全审查清单

## 1. 输入验证

### SQL 注入
- [ ] 所有 SQL 查询使用参数化/预编译语句
- [ ] 没有字符串拼接的 SQL

```python
# ❌ 危险 - SQL 注入
query = f"SELECT * FROM users WHERE id = {user_id}"

# ✅ 安全 - 参数化查询
query = "SELECT * FROM users WHERE id = ?"
cursor.execute(query, (user_id,))
```

### 命令注入
- [ ] 系统命令不直接使用用户输入
- [ ] 使用安全的 API 替代 shell 命令

```python
# ❌ 危险 - 命令注入
os.system(f"rm {filename}")

# ✅ 安全 - 使用 API
os.remove(filename)
```

### XSS (跨站脚本)
- [ ] 用户输入在输出前经过转义
- [ ] 使用安全的模板引擎

```javascript
// ❌ 危险 - XSS
element.innerHTML = userInput;

// ✅ 安全 - 转义
element.textContent = userInput;
```

### 路径遍历
- [ ] 文件路径不直接使用用户输入
- [ ] 验证路径在预期目录内

```python
# ❌ 危险 - 路径遍历
with open(f"/data/{user_filename}") as f:
    return f.read()

# ✅ 安全 - 验证路径
safe_path = os.path.normpath(user_filename)
if '..' in safe_path:
    raise SecurityError()
```

## 2. 认证与授权

### 认证
- [ ] 密码使用安全的哈希算法 (bcrypt, Argon2)
- [ ] 有账户锁定机制
- [ ] 有密码强度要求
- [ ] 登录失败有速率限制

### 授权
- [ ] 每个敏感操作都有权限检查
- [ ] 权限检查在服务端进行
- [ ] 使用最小权限原则
- [ ] 敏感操作有审计日志

```python
# ❌ 危险 - 缺少权限检查
def delete_user(user_id):
    db.delete(user_id)

# ✅ 安全 - 有权限检查
def delete_user(user_id, current_user):
    if not current_user.is_admin:
        raise PermissionError()
    db.delete(user_id)
```

## 3. 敏感数据处理

### 存储
- [ ] 密码不明文存储
- [ ] 敏感配置不硬编码
- [ ] 使用环境变量或密钥管理系统

```python
# ❌ 危险 - 硬编码密钥
API_KEY = "sk-1234567890abcdef"

# ✅ 安全 - 环境变量
API_KEY = os.environ.get("API_KEY")
```

### 传输
- [ ] 敏感数据通过 HTTPS 传输
- [ ] 不在 URL 中传递敏感信息

### 日志
- [ ] 日志不包含密码
- [ ] 日志不包含完整的敏感数据
- [ ] 日志有访问控制

```python
# ❌ 危险 - 日志泄露
logger.info(f"User login: {username}, password: {password}")

# ✅ 安全 - 只记录必要信息
logger.info(f"User login: {username}")
```

## 4. 错误处理

- [ ] 错误信息不暴露系统细节
- [ ] 生产环境关闭调试模式
- [ ] 有全局异常处理

```python
# ❌ 危险 - 暴露系统信息
except Exception as e:
    return {"error": str(e)}  # 可能包含堆栈信息

# ✅ 安全 - 通用错误消息
except Exception as e:
    logger.error(e)
    return {"error": "An error occurred"}
```

## 5. 依赖安全

- [ ] 定期检查依赖的安全漏洞
- [ ] 使用 `npm audit` / `pip-audit` 等工具
- [ ] 及时更新有漏洞的依赖
