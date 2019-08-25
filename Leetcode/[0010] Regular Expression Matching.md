# <p align="center">10. Regular Expression Matching</p>

## 1. Problem:
Given an input string (s) and a pattern (p), implement regular expression matching with support for '.' and '\*'.  
(1)  '.' Matches any single character.  
(2)  '\*' Matches zero or more of the preceding element.    
The matching should cover the entire input string (not partial).  
Note:    
(1) s could be empty and contains only lowercase letters a-z.
(2) p could be empty and contains only lowercase letters a-z, and characters like . or *.


给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '\*' 的正则表达式匹配。  
（1）'.' 匹配任意单个字符  
（2） '\*' 匹配零个或多个前面的那一个元素  
所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。  
说明:  
（1）s 可能为空，且只包含从 a-z 的小写字母。  
（2）p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。

## 2. Examples:
1. Input:  
s = "aa"  
p = "a"  
Output: false  
Explanation: "a" does not match the entire string "aa".  
2. Input:  
s = "aa"  
p = "a\*"  
Output: true  
Explanation: '\*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".  
3. Input:  
s = "ab"  
p = ".\*"  
Output: true  
Explanation: ".\*" means "zero or more (\*) of any character (.)".
4. Input:  
s = "aab"  
p = "c\*a\*b"  
Output: true  
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".
5. Input:  
s = "mississippi"  
p = "mis\*is\*p\*."  
Output: false  

## 3. Tags:
String, Dynamic Programming, Backtracking

## 4. Solutions:

### 1. Recursion

1. 递归判断每个字母是否相同
2. 结束条件：
    - 全部匹配完毕：返回True
    - pattern已结束，但string还没有匹配完毕：返回False
3. 循环体：
    - 当前是否匹配
    - 若下一个字符是"\*": 则当前字符匹配0次，1次或多次
    - 否则，继续后续的匹配

复杂度分析：
- 时间复杂度：$O((T+P)2^{T+ \frac{P}{2}})$.
- 空间复杂度：$O((T+P)2^{T+ \frac{P}{2}})$.


```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        if not p: return not s        
        first_match = bool(s) and p[0] in {s[0], '.'}  
        if len(p) > 1 and p[1] == "*":
            return self.isMatch(s, p[2:]) or first_match and self.isMatch(s[1:], p)
        else: return first_match and self.isMatch(s[1:], p[1:])        
```

存在的问题：
- line 3: if not p: return not s更为简洁
- line 4: s and p[0] in {s[0], '.'} $\to$ bool(s) and p[0] in {s[0], '.'}

### 2. Dynamic Programming

思路：在方法1的基础上，进一步减少计算量与空间复杂度。对于"\*"匹配时，需要考虑两种情况（匹配0次，多次），由此后续相同的字符串需要匹配两次，由此，可通过构建一个hash table，用于存放已经检查过的子串的匹配情况。

复杂度分析：
- 时间复杂度：$O(TP)$.
- 空间复杂度：$O(TP)$.

#### 1. Top-Down Variation 

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        memo = {}
        def dp(i, j):
            if (i, j) not in memo:
                if j == len(p):
                    ans = i == len(s)
                else:
                    first_match = i < len(s) and p[j] in {s[i], '.'}
                    if j+1 < len(p) and p[j+1] == '*':
                        ans = dp(i, j+2) or first_match and dp(i+1, j)
                    else:
                        ans = first_match and dp(i+1, j+1)

                memo[i, j] = ans
            return memo[i, j]

        return dp(0, 0)  
```

#### 2. Bottom-Up Variation

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        dp = [[False] * (len(p) + 1) for _ in range(len(s) + 1)]

        dp[-1][-1] = True
        for i in range(len(s), -1, -1):
            for j in range(len(p) - 1, -1, -1):
                first_match = i < len(s) and p[j] in {s[i], '.'}
                if j+1 < len(p) and p[j+1] == '*':
                    dp[i][j] = dp[i][j+2] or first_match and dp[i+1][j]
                else:
                    dp[i][j] = first_match and dp[i+1][j+1]

        return dp[0][0]    
```
