### 欧几里得算法解决最大公约数
**递归版本**
```python
def gcd(x, y):
    """更简洁的递归实现"""
    if y == 0:
        return abs(x)
    return gcd(y, x % y)

# 测试
print(gcd(12, 18))    # 输出: 6
print(gcd(25, 15))    # 输出: 5
```
**非递归版本**
```python
def gcd(x, y):
    """非递归实现，避免递归深度限制"""
    x, y = abs(x), abs(y)
    while y:
        x, y = y, x % y
    return x

# 测试
print(gcd(12, 18))    # 输出: 6
print(gcd(25, 15))    # 输出: 5
print(gcd(1000000, 100000))  # 大数也能处理




def gcd(n,d):
	while n != d:
		n,d=min(n,d),abs(n-d)
	return n
```

## 无穷大
```python
float('inf')  #正无穷大
float('-inf') #负无穷大
```

## 无限循环
```cpp
while True:
```