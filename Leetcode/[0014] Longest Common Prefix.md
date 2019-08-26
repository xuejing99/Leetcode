
# <p align="center"> 14. Longest Common Prefix </p>

## 1. Problem:
Write a function to find the longest common prefix string amongst an array of strings.
If there is no common prefix, return an empty string "".

编写一个函数来查找字符串数组中的最长公共前缀。
如果不存在公共前缀，返回空字符串 ""。

## 2. Example:
1. 输入: ["flower","flow","flight"]  
输出: "fl"
2. 输入: ["dog","racecar","car"]  
输出: ""  
解释: 输入不存在公共前缀。

## 3. Tags:
String

## 4. Solution:

### 1. start to the end
获取最短的字符串长度，也即是最长的前缀长度。遍历第一个字符串的所有元素，判断该元素是否与其他所有字符串的对应位置的元素相同，若都相同，则当前前缀加上当前字符；若存在不同的，则返回当前的前缀值。


```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs: return ""
        length = min([len(s) for s in strs])
        prefix = ""
        for index in range(length):
            curr = strs[0][index]
            for s in strs[1:]:
                if s[index] != curr: return prefix
            prefix += curr
        return prefix
```

### 2. end to the start
以第一个字符串作为前缀prefix，循环遍历所有字符串，对每个字符串进行判断，是否以prefix作为前缀，若不是，则prefix[:-1]


```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs: return ""
        prefix = strs[0]
        for i in range(1, len(strs)):
            s = strs[i]
            while not s.startswith(prefix):
                prefix = prefix[:-1]
        return prefix
```
