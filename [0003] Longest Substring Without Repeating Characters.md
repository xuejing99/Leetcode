
# <p align="center"> 3. Longest Substring Without Repeating Characters </p>

## 1. Problem:
Given a string, find the length of the longest substring without repeating characters.

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

## 2. Examples:
1. Input: "abcabcbb"  
Output: 3   
Explanation: The answer is "abc", with the length of 3.   
2. Input: "bbbbb"  
Output: 1  
Explanation: The answer is "b", with the length of 1.  
3. Input: "pwwkew"
Output: 3  
Explanation: The answer is "wke", with the length of 3.   
Note that the answer must be a substring, "pwke" is a subsequence and not a substring.  

## 3. Tags:
Hash Table, sliding window, two point, string

## 4. Solutions:

### 1. Brute Force

1. 初始化最大长度
2. 循环遍历string中的所有字符$i, i=1:len(s)$:
3. &emsp; 初始化访问过的元素集合
4. &emsp; 遍历余下元素$j, j=i:len(s)$，求最大长度  
5. &emsp;&emsp; 如果元素s[j]没有在当前元素集合中，则将该元素加入该集合中，当前长度加1  
6. &emsp;&emsp; 若存在于当前元素集合中，更新最大长度，退出内循环
7. 返回最大长度

复杂度分析：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(n)$


```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if not s: return 0
        maxLength = 0
        for i in range(len(s)):
            length = 1
            char_set = set(s[i])
            for j in range(i+1, len(s)):
                if s[j] not in char_set:
                    char_set.add(s[j])
                    length += 1
                else: break
            if length > maxLength: maxLength = length
        return maxLength
```

### 2. Sliding Window

思想：已经验证过没有重复字符的子序列，无需再重复验证。
也即是通过滑窗的方法，依次寻找每个字符对应的最大无重复字符的长度。 

假设有字符串$w1,w2,w3,w4,w5,w6,w7$，若$w1,w2,w3,w4$为无重复字符的子串，则指针right向右移一位，指向$w5$，此时，若$w5$与$w3$相同，则表明，指针left指向的元素$w1$对应的最大长度为4($w1,w2,w3,w4$)。此后，指针left向右移一位，指向$w2$，并将$w1$从hash table的删除。对于$w2,w3,w4$已由上一步验证是没有重复字符的，所以只需要验证$w5$是否存在于$w2$对应的子串中。若不存在，right向右移动一位，并将$w5$添加至hash table中，否则，left向右移一位，并将$w2$删除，以此类推。

1. 初始化左、右指针：left, right = 0, 0
2. 初始化hash set，maxLength = 0
3. 当left，right没有指向列表尾端：
4. &emsp; 判断s[right]是否存在于hash set中：
5. &emsp; &emsp; 若存在，则最大长度为max(right-left, maxLength)，删除hash set中的s[left]，left += 1
6. &emsp; &emsp; 若不存在，right += 1， 将s[right]添加至hash set
7. 返回最大长度

复杂度分析：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$


```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        hashSet = set()
        left, right, maxLength = 0, 0, 0
        while left != len(s) and right != len(s):
            if s[right] not in hashSet:
                hashSet.add(s[right])
                right += 1
            else:
                hashSet.remove(s[left])
                maxLength = max(maxLength, right-left)
                left += 1
        maxLength = max(maxLength, right-left)
        return maxLength
```

### 3. Sliding Window Optimized

思想：方法2中是依次寻找每个字符对应的最大长度。一个更简单的方法则是当遇到重复字符时，并不是从left指向的下一个字符开始寻找，而是直接从重复的字符的下一个字符处考试寻找。

1. 初始化左、右指针：left, right = 0, 0
2. 初始化hash table，maxLength = 0
3. 当left，right没有指向列表尾端：
4. &emsp; 判断s[right]存在于hash table中，且hashTable[s[right]] <= left ?   
 &emsp; 若为真，表明s[right]在子串s[left:right]中重复出现
5. &emsp; &emsp; 若存在且位于子串中：计算最大长度，更新left = hashTable[s[right]] + 1
6. &emsp; &emsp; 更新hashTable[s[right]] = right, right += 1，
7. 返回最大长度

复杂度分析：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$


```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        hashTable = dict()
        left, right, maxLength = 0, 0, 0
        while left != len(s) and right != len(s):
            if s[right] in hashTable and hashTable[s[right]] >= left:
                maxLength = max(maxLength, right-left)
                left = hashTable[s[right]]+1
            hashTable[s[right]] = right
            right += 1
        maxLength = max(maxLength, right-left)
        return maxLength
```
