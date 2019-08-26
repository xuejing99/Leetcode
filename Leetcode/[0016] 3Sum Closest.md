
# <p align="center"> 16. 3Sum Closest</p>

## 1.Problem:
Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

## 2. Example:
Given array nums = [-1, 2, 1, -4], and target = 1.  
The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

## 3. Tags:
Array, Two Pointers

## 4. Solution:

思路：同3 Sum，在此过程中判断距离是否小于当前最小距离，若小于则更新，否则，继续访问下一个元素直到循环完所有可能。

复杂度分析：
- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(1)$


```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums = sorted(nums)
        print (nums)
        res = sum(nums[:3])
        diff = abs(target - res)
        for i in range(len(nums)-2):            
            left, right, complement = i+1, len(nums)-1, target-nums[i]
            while(left != right):
                print (nums[i],nums[left],nums[right])
                if 2*nums[left] > complement: 
                    temp = abs(complement - nums[left] - nums[left+1])
                    if temp < diff: res, diff = nums[i]+nums[left]+nums[left+1], temp
                    break
                elif 2*nums[right] < complement: 
                    temp = abs(complement - nums[right] - nums[right-1])
                    if temp < diff: res, diff = nums[i]+nums[right]+nums[right-1], temp
                    break
                elif nums[left] + nums[right] <  complement:
                    temp = abs(complement - nums[left] - nums[right])
                    if temp < diff: res, diff = nums[i]+nums[right]+nums[left], temp
                    left += 1
                elif nums[left] + nums[right] >  complement:
                    temp = abs(complement - nums[left] - nums[right])
                    if temp < diff: res, diff = nums[i]+nums[right]+nums[left], temp
                    right -= 1
                else: return target
        return res
```
