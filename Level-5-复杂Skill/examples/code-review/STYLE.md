# 代码风格指南

## 1. 命名规范

### 变量命名
- [ ] 使用有意义的名称，避免单字母变量（循环计数器除外）
- [ ] 布尔变量使用 is/has/can/should 前缀
- [ ] 常量使用全大写下划线分隔

```python
# ❌ 不好的命名
x = 5
flag = True
def calc(a, b):
    pass

# ✅ 好的命名
max_retry_count = 5
is_active = True
def calculate_total_price(unit_price, quantity):
    pass
```

### 函数命名
- [ ] 使用动词开头
- [ ] 名称描述函数的功能
- [ ] 遵循项目的命名约定 (camelCase/snake_case)

### 类命名
- [ ] 使用名词
- [ ] 使用 PascalCase
- [ ] 名称描述类代表的实体

## 2. 代码结构

### 函数长度
- [ ] 函数不超过 50 行
- [ ] 单一职责，只做一件事
- [ ] 可以一眼看出函数的功能

### 嵌套深度
- [ ] 嵌套不超过 3 层
- [ ] 使用早返回减少嵌套

```python
# ❌ 深层嵌套
def process(data):
    if data:
        if data.is_valid:
            if data.type == 'A':
                if data.value > 0:
                    return handle_a(data)

# ✅ 早返回，减少嵌套
def process(data):
    if not data:
        return None
    if not data.is_valid:
        return None
    if data.type != 'A':
        return None
    if data.value <= 0:
        return None
    return handle_a(data)
```

### 类结构
- [ ] 公共方法在前，私有方法在后
- [ ] 相关方法放在一起
- [ ] 类不超过 300 行

## 3. 注释规范

### 何时注释
- [ ] 解释"为什么"，而非"是什么"
- [ ] 复杂逻辑需要注释
- [ ] 临时解决方案标记 TODO

```python
# ❌ 无用的注释
i = i + 1  # 把 i 加 1

# ✅ 有价值的注释
# 这里加 1 是因为数组索引从 0 开始，但业务 ID 从 1 开始
i = i + 1
```

### 文档注释
- [ ] 公共 API 有文档注释
- [ ] 说明参数和返回值
- [ ] 包含使用示例

```python
def calculate_discount(price: float, rate: float) -> float:
    """
    计算折扣后的价格。
    
    Args:
        price: 原价，必须为正数
        rate: 折扣率，0-1 之间，如 0.8 表示 8 折
    
    Returns:
        折扣后的价格
    
    Example:
        >>> calculate_discount(100, 0.8)
        80.0
    """
    return price * rate
```

## 4. 错误处理

### 异常使用
- [ ] 使用具体的异常类型
- [ ] 不要捕获后忽略
- [ ] 异常消息清晰有用

```python
# ❌ 不好的异常处理
try:
    do_something()
except:
    pass

# ✅ 好的异常处理
try:
    do_something()
except ValueError as e:
    logger.error(f"Invalid value: {e}")
    raise BusinessError(f"处理失败: {e}") from e
```

### 空值处理
- [ ] 检查可能为空的值
- [ ] 使用 Optional 类型标注
- [ ] 提供合理的默认值

## 5. 代码重复

### DRY 原则
- [ ] 没有明显的代码复制粘贴
- [ ] 相似逻辑抽取为函数
- [ ] 配置数据与代码分离

```python
# ❌ 代码重复
def process_apple(apple):
    apple.wash()
    apple.peel()
    apple.cut()
    return apple

def process_orange(orange):
    orange.wash()
    orange.peel()
    orange.cut()
    return orange

# ✅ 消除重复
def process_fruit(fruit):
    fruit.wash()
    fruit.peel()
    fruit.cut()
    return fruit
```

## 6. 代码格式

### 格式化
- [ ] 使用项目统一的格式化工具
- [ ] 缩进一致
- [ ] 行长度不超过 120 字符

### 空白使用
- [ ] 运算符两边有空格
- [ ] 逗号后有空格
- [ ] 代码块之间有空行

## 语言特定检查

### Python
- [ ] 遵循 PEP 8
- [ ] 使用类型注解
- [ ] 使用 f-string 而非 % 或 format

### JavaScript/TypeScript
- [ ] 使用 const/let，避免 var
- [ ] 使用 === 而非 ==
- [ ] 异步操作使用 async/await

### C++
- [ ] RAII 资源管理
- [ ] 使用智能指针
- [ ] 避免裸 new/delete
