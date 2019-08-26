
# <p align="center"> 17. Letter Combinations of a Phone Number </p>

## 1. Problem:
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.  
A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.  
"2":"abc", "3":"def", "4":"ghi","5":"jkl","6":"mno","7":"pqrs","8":"tuv", "9":"wxyz"

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。  
给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

## 2. Example:
Input: "23"  
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].

## 3. Tags:
String, Backtracking

## 4. Solution:

从前往后依次读入字符，将下一个数字对应的字符依次分别拼接至前面所有的字符串后面。

复杂度分析：
- 时间复杂度：$O(3^N×4^M)$
- 空间复杂度：$O(3^N×4^M)$

### 1. Breadth-first Combination


```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits: return []
        dict = {"2":"abc", "3":"def", "4":"ghi","5":"jkl", 
                "6":"mno","7":"pqrs","8":"tuv", "9":"wxyz"}
        res = list(dict[digits[0]])
        for i in digits[1:]:
            for j in range(len(res)):
                prev = res.pop(0)
                res += [prev + item for item in dict[i]]            
        return res
```

### 2. Depth-first Combination


```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        dict = {"2":"abc", "3":"def", "4":"ghi","5":"jkl", 
                "6":"mno","7":"pqrs","8":"tuv", "9":"wxyz"}
        output = list()
        
        def backtrack(combination, nextDigits):
            if not nextDigits: 
                output.append(combination)
                return
            for letter in dict[nextDigits[0]]:
                backtrack(combination + letter, nextDigits[1:])
        
        if digits: 
            backtrack("", digits)
        return output
```
