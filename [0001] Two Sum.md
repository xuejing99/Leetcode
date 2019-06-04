
# <p align="center"> 1. Two Sum</p>

## 1. Problem:
Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

## 2. Example:
Given nums = [2, 7, 11, 15], target = 9,  
Because nums[0] + nums[1] = 2 + 7 = 9,  
return [0, 1].

## 3. Tags:
Array, Hash Table

## 4. Solutions:

### 1. Brute Force

1. 从头开始遍历元素: $i = 0:len(nums)$
2. &emsp; 从余下的元素中筛查是否存在元素$j$，使得$nums[i]+nums[j]==target$，其中，$j = i:len(nums)$
3. &emsp; &emsp; 若存在，则返回$[i,j]$
4. 若遍历完后，不存在$nums[i]+nums[j]==target$， 则返回空列表

复杂度分析：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(1)$


```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        for i in range(len(nums)):
            for j in range(i+1, len(nums)):
                if nums[i] + nums[j] == target:
                    return [i,j]
```



### 2. Two-pass Hash Table

1. 第一次遍历，构建hash table，其中key为元素值，value为对应元素的index索引  
2. 第二次遍历，判断列表中的元素$i, i=0:len(nums)$，是否存在补数$target-nums[i]$存在于之前构建的字典中，且二者的索引不同，确保不是同一元素重复使用。


- 注意：当有重复数值$n$时，字典保存的是key为$n$，value为靠后的$n$的索引。与之对应的是第二次遍历过程中，从前开始访问元素，对应的是靠前的$n$的索引。

复杂度分析：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$


```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dict = {num:pos for pos, num in enumerate(nums)}
        print (dict)
        for i in range(len(nums)):
            complement = target - nums[i]
            if (complement) in dict and dict[complement] != i:
                return [i, dict[complement]]
```


### 3. One-pass Hash Table

1. 构建词典dict
2. 遍历列表中的所有元素 $i, i = 0:len(nums)$:
3. &emsp; 若元素$target-nums[i]$存在于列表中，则返回$[i, dict[target-nums[i]]]$
4. &emsp; 若元素$target-nums[i]$不存在于列表中，则将该元素添加至词典dict中


- 备注：由第二个元素检索第一个元素

复杂度分析：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(1)$


```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dict = {}
        for index, num in enumerate(nums):
            if (target-num) in dict:
                return [dict[target-num], index]
            else:
                dict[num] = index
```
