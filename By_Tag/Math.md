## [29. Divide Two Integers](https://leetcode-cn.com/problems/divide-two-integers/)

### Method 1 bit operation
```python
class Solution:
    def divide(self, dividend: int, divisor: int) -> int:
        sign = (dividend > 0) ^ (divisor > 0)
        dividend = abs(dividend)
        divisor = abs(divisor)
        count = 0
        #把除数不断左移，直到它大于被除数
        while dividend >= divisor:
            count += 1
            divisor <<= 1
        result = 0
        while count > 0:
            count -= 1
            divisor >>= 1
            if divisor <= dividend:
                result += 1 << count #这里的移位运算是把二进制（第count+1位上的1）转换为十进制
                dividend -= divisor
        if sign: result = -result
        return result if -(1<<31) <= result <= (1<<31)-1 else (1<<31)-1 
```
- Should consider the overflow
- Not Understood yet

## [50.  Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

### Method 1 Binary search/Recursion/Fast Pow
```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n == 0: return 1
        if n == 1: return x
        if n == -1: return 1 / x
        half = self.myPow(x, n // 2);
        rest = self.myPow(x, n % 2);
        return rest * half * half;
```

