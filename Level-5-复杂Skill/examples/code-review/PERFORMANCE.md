# 性能审查要点

## 1. 算法复杂度

### 时间复杂度
- [ ] 核心算法的时间复杂度是否合理
- [ ] 是否有嵌套循环可以优化
- [ ] 是否有更高效的算法可用

```python
# ❌ O(n²) - 低效
def find_duplicates(arr):
    duplicates = []
    for i in range(len(arr)):
        for j in range(i+1, len(arr)):
            if arr[i] == arr[j]:
                duplicates.append(arr[i])
    return duplicates

# ✅ O(n) - 高效
def find_duplicates(arr):
    seen = set()
    duplicates = set()
    for item in arr:
        if item in seen:
            duplicates.add(item)
        seen.add(item)
    return list(duplicates)
```

### 空间复杂度
- [ ] 内存使用是否合理
- [ ] 是否有不必要的数据复制
- [ ] 大数据量是否使用流式处理

## 2. 数据库性能

### N+1 查询问题
- [ ] 是否在循环中执行查询
- [ ] 是否使用了预加载 (eager loading)

```python
# ❌ N+1 问题 - 每个用户一次查询
for user in users:
    orders = db.query(Order).filter(user_id=user.id).all()
    print(user.name, len(orders))

# ✅ 优化 - 一次查询
user_ids = [u.id for u in users]
orders = db.query(Order).filter(Order.user_id.in_(user_ids)).all()
order_map = defaultdict(list)
for order in orders:
    order_map[order.user_id].append(order)
```

### 索引使用
- [ ] 查询条件字段是否有索引
- [ ] 是否有不必要的全表扫描
- [ ] 复合索引的字段顺序是否正确

### 查询优化
- [ ] SELECT 是否只查询需要的字段
- [ ] 是否有不必要的 JOIN
- [ ] 分页查询是否使用 LIMIT/OFFSET 或游标

## 3. 内存管理

### 大对象处理
- [ ] 大文件是否流式读取
- [ ] 大列表是否使用生成器

```python
# ❌ 一次性加载全部
def read_large_file(path):
    with open(path) as f:
        return f.read()  # 可能耗尽内存

# ✅ 流式读取
def read_large_file(path):
    with open(path) as f:
        for line in f:
            yield line
```

### 内存泄漏
- [ ] 是否有循环引用
- [ ] 回调/事件监听是否正确清理
- [ ] 缓存是否有大小限制

## 4. 网络与 I/O

### HTTP 请求
- [ ] 是否有不必要的串行请求
- [ ] 是否可以批量请求
- [ ] 是否有超时设置

```python
# ❌ 串行请求
for url in urls:
    response = requests.get(url)
    results.append(response.json())

# ✅ 并行请求
import asyncio
import aiohttp

async def fetch_all(urls):
    async with aiohttp.ClientSession() as session:
        tasks = [fetch(session, url) for url in urls]
        return await asyncio.gather(*tasks)
```

### 文件 I/O
- [ ] 是否使用缓冲区
- [ ] 是否及时关闭文件句柄
- [ ] 是否可以使用异步 I/O

## 5. 缓存策略

### 缓存使用
- [ ] 频繁访问的数据是否有缓存
- [ ] 缓存 key 设计是否合理
- [ ] 缓存是否有过期策略

```python
# 简单的函数结果缓存
from functools import lru_cache

@lru_cache(maxsize=100)
def expensive_computation(n):
    # 复杂计算...
    return result
```

### 缓存失效
- [ ] 数据更新时缓存是否同步失效
- [ ] 是否有缓存穿透/雪崩防护

## 6. 并发性能

### 锁竞争
- [ ] 临界区是否尽量小
- [ ] 是否可以使用读写锁
- [ ] 是否有死锁风险

### 线程池
- [ ] 线程池大小是否合理
- [ ] 是否有任务队列溢出风险

## 常见性能反模式

| 反模式 | 问题 | 解决方案 |
|--------|------|----------|
| 循环内查询 | N+1 问题 | 批量查询 |
| SELECT * | 多余数据传输 | 只选需要的列 |
| 字符串拼接 | 频繁内存分配 | 使用 StringBuilder |
| 大对象复制 | 内存浪费 | 传引用或流式处理 |
| 同步阻塞 | 线程浪费 | 异步/非阻塞 |
