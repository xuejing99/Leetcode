
# <p align="center">6. ZigZag Conversion</p>

## 1. Problem:
The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)  
```
P   A   H   N  
A P L S I I G  
Y   I   R  
```
And then read line by line: "PAHNAPLSIIGYIR"  
Write the code that will take a string and make this conversion given a number of rows:  
string convert(string s, int numRows);  

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。  
比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：  
```
P   A   H   N  
A P L S I I G  
Y   I   R  
```
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："PAHNAPLSIIGYIR"。  
请你实现这个将字符串进行指定行数变换的函数：  
string convert(string s, int numRows); 
```
P     I    N
A   L S  I G
Y A   H R
P     I
```

## 2. Examples:
1. Input: s = "PAYPALISHIRING", numRows = 3  
Output: "PAHNAPLSIIGYIR"
2. Input: s = "PAYPALISHIRING", numRows = 4  
Output: "PINALSIGYAHRPI"  
Explanation:  
```
P     I    N  
A   L S  I G  
Y A   H R  
P     I  
```

## 3. Tags:
String

## 4. Solutions:

### 1. Sort by Row

思路：顺序读取字符串s，构建一个二维列表$A[1,numRow]$并将相应字符存放置相应的行中，最后将行拼接，输出。

1. 初始化二维列表，获取循环周期 $recurrent=2*(numRow-2)+2$
2. 遍历s中的所有元素s[i], i=0:len(s):
3. &emsp; 计算元素归属的行：$pos = pos \% recurrent$, 当$pos>recurrent$时，调整$pos=recurrent-pos$
4. &emsp; 将元素添加至相应的行中
5. 拼接所有行的元素，返回

复杂度分析：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$


```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1: return s
        res = [''] * numRows
        recurrent = (numRows - 2) * 2 + 2
        for i in range(len(s)):
            pos = i%recurrent
            if pos >= numRows: pos = recurrent - pos
            res[pos] += s[i]
        res = ''.join(res)
        return res
```

### 2. Visit by Row

思想：按行的顺序访问字符串s中的数组
