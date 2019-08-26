
# <p align="center"> 20. Valid Parentheses </p>

## 1. Problem:
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.  
An input string is valid if:  
Open brackets must be closed by the same type of brackets.  
Open brackets must be closed in the correct order.  
Note that an empty string is also considered valid.  

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。  
有效字符串需满足：  
左括号必须用相同类型的右括号闭合。  
左括号必须以正确的顺序闭合。  
注意空字符串可被认为是有效字符串。  

## 2. Example:
1. Input: "()"  
Output: true
2. Input: "()[]{}"  
Output: true
3. Input: "(]"  
Output: false
4. Input: "([)]"  
Output: false
5. Input: "{[]}"  
Output: true

## 3. Tags:
String, Stack

## 4. Solution:

思路：
1. 构建左、右括号配对的词典
2. 若是左括号，则入栈，若是右括号，则出栈，判断两个括号是否匹配，若不匹配，则返回False，否则，继续判断
3. 完成遍历后，若堆栈中还有数值，则表明还有左括号没有被匹配，返回False，否则，返回True


```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        left = []
        dict = {')':'(', ']':'[', '}':'{'}
        for i in s:
            if i in {'(','[','{'}: left.append(i)
            elif (not left) or (dict[i] != left.pop()): return False
        if left: return False
        else: return True
```
