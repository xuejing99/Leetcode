
# <p align="center"> 7. Reverse Integer </p>

## 1. Problem:
Given a 32-bit signed integer, reverse digits of an integer.    
Note:
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $[−2^{31},  2^{31} − 1]$. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。  
注意:
假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为  $[−2^{31},  2^{31} − 1]$。请根据这个假设，如果反转后整数溢出那么就返回 0。


## 2. Example:
1. Input: 123  
Output: 321  
2. Input: -123  
Output: -321  
3. Input: 120  
Output: 21  

## 3. Tags:
Math

## 4. Solutions:

### 1. Convert to String

1. 获取正负号，并全转换成正数
2. 将整数转换成字符串，并将字符串倒置
3. 将反转的字符串转换成整数，并乘上正负号
4. 判断是否溢出，若溢出则返回0，否则，返回该数。

复杂度分析：
- 时间复杂度：
- 空间复杂度：$O(n)$


```python
class Solution:
    def reverse(self, x: int) -> int:
        if x < 0: 
            minus = -1
            x = -x
        else: minus = 1
        
        x = str(x)[::-1]
        x = minus * int(x)
        
        if x < -2**31 or x > 2**31-1:
            return 0
        else: return x
```

### 2. Pop and Push Digits & Check before Overflow

思路：$ pop \Leftrightarrow x\%10, x=x/10$; push $\Leftrightarrow x*10 $；在此基础上考虑边界条件。 

复杂度分析：
- 时间复杂度：$O(log(x))$
- 空间复杂度：$O(1)$


```python
class Solution:
    def reverse(self, x: int) -> int:
        maximum, minimum = 2**31-1, -2**31
        if x < 0: 
            minus = -1
            x = -x
        else: minus = 1
        rev = 0
        while (x != 0):
            pop = x % 10
            x = x // 10
            if (rev > maximum/10): return 0
            if (rev == maximum/10 and minus==-1 and pop > 7) :return 0
            if (rev == minimum/10 and minus==1 and pop > 8): return 0
            rev = rev * 10 + pop
        return minus * rev
```

存在的问题：
- $-8\%10 = 2$，需要先将x化为正数
