
# <p align="center">8. String to Integer (atoi)</p>

## 1. Problem:
Implement atoi which converts a string to an integer.  
The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.  
The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.  
If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.  
If no valid conversion could be performed, a zero value is returned.  
Note:  
- Only the space character ' ' is considered as whitespace character.  
- Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $[−2^{31},  2^{31} − 1]$. If the numerical value is out of the range of representable values, INT_MAX ($2^{31} − 1$) or INT_MIN ($−2^{31}$) is returned.

请你来实现一个 atoi 函数，使其能将字符串转换成整数。  
首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。  
当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。  
该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。  
注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。  
在任何情况下，若函数不能进行有效的转换时，请返回 0。  
说明：  
- 只有空格被考虑为空白字符
- 假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 $[−2^{31},  2^{31} − 1]$。如果数值超过这个范围，qing返回  INT_MAX ($2^{31} − 1$) 或 INT_MIN ($−2^{31}$) 。


## 2. Example:
1. Input: "42"  
Output: 42
2. Input: "   -42"  
Output: -42  
Explanation: The first non-whitespace character is '-', which is the minus sign.Then take as many numerical digits as possible, which gets 42.
3. Input: "4193 with words"  
Output: 4193  
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
4. Input: "words and 987"  
Output: 0  
Explanation: The first non-whitespace character is 'w', which is not a numerical digit or a +/- sign. Therefore no valid conversion could be performed.
5. Input: "-91283472332"  
Output: -2147483648  
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.Thefore INT_MIN (−231) is returned.

## 3. Tags:
Math, String

## 4. Solutions:

### 1. Convert to character by character

1. 删除起始位置多余的空白字符
2. 判断正负号：注意"+"号的情况
3. 初始化数字num
4. 循环读入字符串中的字符，判断是否为数字，若为数字则进行累加操作，否则，终止循环
5. 恢复正负号，判断是否溢出，返回num

复杂度分析：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$


```python
class Solution:
    def myAtoi(self, str: str) -> int:
        str = str.strip()
        if not str: return 0        
        maximum, minimum = 2**31-1, -2**31       
        if str[0] == '-': 
            minus = -1
            str = str[1:]
        elif str[0] == '+': 
            str = str[1:]
            minus = 1
        else:
            minus = 1
            
        num = 0        
        for char in str:
            if char not in {'0','1','2','3','4','5','6','7','8','9'}:
                break
            num = num * 10 + int(char)
        return min(max(minus * num, minimum), maximum)
```


## 2. Intercept the string and convert it


```python
class Solution(object):
    def myAtoi(self, a):
        """
        :type str: str
        :rtype: int
        """
        # 删除头尾空格，不包括中间
        a = a.strip()
        len_a = len(a)
        
        # aa用来存贮符号
        aa = ''
        c = 0
        i = 0

        if a == '':
            return 0
        elif a[0] == '-':
            aa = '-'
            a = a[1:]
        elif a[0] == '+':
            a = a[1:]

        while a[:i+1].isdigit() and i<= len_a:
            c += 1
            i += 1

        if c==0:
            return 0
        else:
            return min(2**31-1, max(int(aa+a[:c]),-2**31))
```
