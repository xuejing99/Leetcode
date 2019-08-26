
# <p align="center"> 12. Integer to Roman </p>

## 1. Problem:
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.
```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.  
Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:  
(1) I can be placed before V (5) and X (10) to make 4 and 9.   
(2) X can be placed before L (50) and C (100) to make 40 and 90.   
(3)C can be placed before D (500) and M (1000) to make 400 and 900.  
Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

罗马数字包含以下七种字符： I， V， X， L，C，D 和 M。    
例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。  
通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：  
（1）I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。  
（2）X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。   
（3）C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。  
给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。  

## 2. Example:
1. Input: 3  
Output: "III" 
2. Input: 4  
Output: "IV"
3. Input: 9  
Output: "IX"
4. Input: 58  
Output: "LVIII"  
Explanation: L = 50, V = 5, III = 3.
5. Input: 1994  
Output: "MCMXCIV"  
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.

## 3. Tags:
Math, String

## 4. Solutions

### 1. Roman numeral composition

思路：将数字与对应的罗马字符存储在hash table中。将可选字符对应的数字item由大到小的顺序排列，再用num整除item，判断是否比item大，若比item大，则尽可能减去最多的item (即num//item)，再将item对应的字符（重复num//item次）拼接至返回的字符串中。


```python
class Solution:
    def intToRoman(self, num: int) -> str:
        if num == 0: return ""
        lookup = {1:"I", 4:"IV", 5:"V", 9:"IX", 10:"X", 40:"XL", 50:"L", 90:"XC",
                  100:"C", 400:"CD", 500:"D", 900:"CM", 1000:"M"}
        res = '' 
        for item in sorted(lookup,reverse=True):
            if num // item:
                res += lookup[item]*(num // item)
                num -= item * (num // item)                
        return res
```

存在的问题：
- line 9 与line 10的位置不能互换

### 2. Maps Roman numerals to decimal Numbers

思路：构建映射表：个、十、百、千位对应的罗马数字按照阿拉伯数字的构成映射至罗马字符中


```python
class Solution:
    def intToRoman(self, num: int) -> str:
        M = ['','M','MM','MMM']
        C = ['','C','CC','CCC','CD','D','DC','DCC','DCCC','CM']
        X = ['','X','XX','XXX','XL','L','LX','LXX','LXXX','XC']
        I = ['','I','II','III','IV','V','VI','VII','VIII','IX']
        
        return M[num//1000] + C[(num%1000)//100] + X[(num%100)//10] + I[(num%10)]
```
