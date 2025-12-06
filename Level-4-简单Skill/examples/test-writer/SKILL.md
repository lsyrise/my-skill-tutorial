---
name: test-writer
description: 编写单元测试和测试用例。当用户需要写测试、增加测试覆盖率、验证代码正确性、或问"怎么测试这个"时使用。
---

# Test Writer

## Instructions

1. 分析被测代码的功能和接口
2. 识别关键路径和边界情况
3. 设计覆盖完整的测试用例
4. 编写测试代码

## 测试用例设计

### 正常路径测试
- 标准输入 → 预期输出
- 常见使用场景

### 边界条件测试
- 空值/null
- 最大值/最小值
- 边界临界点
- 空集合/空字符串

### 异常路径测试
- 无效输入
- 类型错误
- 异常情况

### 性能测试（可选）
- 大数据量
- 高并发

## 测试框架选择

根据项目语言自动选择：

| 语言 | 框架 | 示例 |
|------|------|------|
| Python | pytest | `def test_xxx():` |
| JavaScript | Jest | `test('xxx', () => {})` |
| TypeScript | Jest/Vitest | `it('should xxx', () => {})` |
| C++ | Google Test | `TEST(Suite, Case) {}` |
| Go | testing | `func TestXxx(t *testing.T) {}` |
| Java | JUnit | `@Test void testXxx() {}` |

## 测试命名规范

```
test_[被测函数]_[场景]_[预期结果]
```

示例:
- `test_add_positive_numbers_returns_sum`
- `test_divide_by_zero_throws_exception`

## 输出格式

为每个测试用例提供：
1. 测试名称
2. 测试目的
3. 测试代码
4. 预期结果
