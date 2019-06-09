
# <p align="center">9. Palindrome Number</p>

## 1. Problem:
Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

## 2. Examples:
1. Input: 121  
Output: true
2. Input: -121  
Output: false  
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
3. Input: 10  
Output: false  
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.

## 3. Tags:
Math

## 4. Solutions:

### 1. Convert to String

1. 获取数字的字符串表示
2. 判断正序的字符串与倒序的字符串是否相同

复杂度分析：
- 时间复杂度：$O(1)$
- 空间复杂度：$O(n)$


```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        x = str(x)
        return x == x[::-1]
```

### 2. Revert half of the number

1. 思路：pop and push，查看数字的后半部分倒序以后是否与前一半的数字相同。 
2. 注意：为什么不获取数字完整的倒序，而是只倒置一半？防止数据溢出
3. 如何判断已经反转了一半的数字？当反转后的数字大于原始pop后的数字

1. 负数，一定是非回文
2. 倒置一半的数据
3. 判断倒置后的数据是否与原始pop后的数据相同

复杂度分析：
- 时间复杂度：$O(log_{10}n)$
- 空间复杂度：$O(1)$


```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if (x < 0) or (x % 10 == 0 and x != 0):
            return False
        reverse = 0
        while reverse < x:
            pop = x % 10
            x = x // 10
            reverse = reverse * 10 + pop
        if x == reverse or x == reverse // 10:
            return True
        return False
```
