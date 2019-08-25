
# <p align="center"> 5. Longest Palindromic Substring </p>

## 1. Problem:
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

## 2. Examples:
1. Input: "babad"  
Output: "bab"  
Note: "aba" is also a valid answer.  
2. Input: "cbbd"  
Output: "bb"  

## 3. Tags:
String, Dynamic Programming

## 4. Solutions:

### 1. Longest Common Substring

思想：Reverse $S$ and become $S'$. Find the longest common substring between $S$ and $S'$, which must also be the longest palindromic substring. 反转字符串$S$为$S'$，找到$S$与$S'$的最大公共子串。

$S = "abacdfgdcaba", S'= "abacdgfdcaba"$. The longest common substring between $S$ and $S'$ is "abacd". Clearly, this is not a valid palindrome.

我们可以看到，当$S$的其他部分存在一个非回文子串的反向副本时，最长公共子串方法就会失败。  
为了纠正这一点，每次找到最长公共子字符串候选项时，我们都要检查子字符串的索引是否与反向子字符串的原始索引相同。如果是，则尝试更新到目前为止发现的最长回文;如果没有，我们跳过这个，找到下一个候选人。

复杂度分析：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n^2)$

### 2. Brute Force

思想：找到所有可能的子串组合，再一一验证是否为回文子串

复杂度分析：
- 时间复杂度：$O(n^3)$
- 空间复杂度：$O(1)$

### 3. Dynamic Programming

思想：在Brute Force中，可以进一步减少计算量，例如对于cbaabc，若已证明baab是回文字符串，则只需验证baab两端的字符串是否相同即可。

1. 构建一个二维的列表A[n,n]，存储字符串中的回文信息
2. 单一元素构成一个回文，即置A[i,i]=True, i = 0:len(s)
3. 考虑两个元素构成回文的情况，若s[i]=s[i+1]，则置A[i,i+1]=True
3. 循环考虑3:m个元素的情况，即是step=2:len(s)：
4. &emsp;i=0:len(s)-step, j=i+step
5. &emsp;当i, j没有超出字符串长度，查找两端元素相等的情况：s[i] == s[j]
6. &emsp;&emsp;若两个元素相等，判断向中间各缩进一位的子串是否为回文子串即A[i+1,j-1]==True  
&emsp;&emsp;若是回文字符串，则表明s[i,j]也构成一个回文子串，则置A[i,j]=True，更新最长回文子字符串。
7. 循环4-6

复杂度分析：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n^2)$


```python
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        length = len(s)
        arr_2d = [[False for _ in range(length)] for _ in range(length)]
        
        max_sub_len = 0
        substring = (0, 0)
        
        # Trivial case
        for i in range(length):
            arr_2d[i][i] = True
        
        # Two chars base case
        for i in range(length-1):
            j = i + 1
            arr_2d[i][j] = s[i] == s[j]
            if arr_2d[i][j] and (2 > max_sub_len):
                max_sub_len = 2
                substring = (i, j)
        
        # General case
        step = 2
        for _ in range(length-2):
            i = 0
            j = i+step
            while (i < length) and (j < length):
                arr_2d[i][j] = (s[i] == s[j]) and arr_2d[i+1][j-1]
                if arr_2d[i][j] and (j-i+1 > max_sub_len):
                    max_sub_len = j-i+1
                    substring = (i, j)
                i += 1
                j += 1
            step += 1
        return s[substring[0]: substring[1]+1]
```

### 4. Expand Around Center

思路：循环遍历每个字符，并按该字符为中心，向两边延申寻找最长的回文字符串

1. 遍历s中的所有元素$s[i], i = 0:len(s)$:
2. &emsp;若s[i-1] == s[i]，若s[i-1] == s[i+1]  
&emsp;表明构成一个回文子串，将以此为中心，向两边扩展，找到最长的回文字符串  
&emsp;接下来循环对应的左、右指针为(left = i-2, right = i+1)与(left = i-1, right = i+1)
3. &emsp;&emsp;循环直到遇到字符串的边界或两边的元素不相同：令left=left-1， right=right+1
4. &emsp;&emsp; 返回元素左右指针的位置，判断是否为最长子串，对其更新。

复杂度分析：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(1)$


```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        start, end = 0, 1
        for i in range(len(s)):
            if i>0 and s[i] == s[i-1]:
                m, n = self.subPalindrom(s, i-2, i+1)
                if n-m > end-start: start, end = m, n
            if i>0 and i<len(s)-1 and s[i-1] == s[i+1]:
                m, n = self.subPalindrom(s, i-1, i+1)
                if n-m > end-start: start, end = m, n
        return s[start: end]
        
    def subPalindrom (self, s, left, right):
        while(left>=0 and right<len(s)):
                if s[right] == s[left]:
                    left -= 1
                    right += 1
                else: break                        
        return left+1, right
```

### 5. Manacher's Algorithm
