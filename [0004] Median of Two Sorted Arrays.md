
# <p align="center"> Median of Two Sorted Arrays </p>

## 1. Problem:
There are two sorted arrays nums1 and nums2 of size m and n respectively.
Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).
You may assume nums1 and nums2 cannot be both empty.

给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。
请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。
你可以假设 nums1 和 nums2 不会同时为空。

## 2. Examples:
1. nums1 = [1, 3]  
nums2 = [2]  
The median is 2.0  
2. nums1 = [1, 2]  
nums2 = [3, 4]  
The median is (2 + 3)/2 = 2.5

## 3. Tags:
Array, Binary Tree, Divide and Conquer 分而治之

## 4. Solutions：

### 1. Easy Way


```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        num = sorted(nums1 + nums2)
        if len(num)%2:
            return num[len(num)//2]            
        else:
            return (num[len(num)//2-1] + num[len(num)//2]) / 2
```

### 2. Recursive Approach

思想：
1. 中位数：将一个集合分成相同大小的两个子集，同时一个子集一定优于另一个子集。
2. $ A = \{x_1,x_2, \cdots, x_{i-1}, x_i, x_{i+1}, \cdots, x_m\} $  
  $ B = \{y_1,y_2, \cdots, y_{j-1}, y_j, y_{j+1}, \cdots, y_n\} $  
  找到A, B集合的中位数，意味着，找到合适的$i$，$j$，使得：  
  （1） $i+j = n+m-i-j$ 或 $i+j = n+m-i-j+1$  $\Rightarrow$ $j = \frac{m+n+1}{2}-i$  
  （2） $B[j-1] \leqslant A[i]$，且 $A[j-1] \leqslant B[j]$  
3. 考虑约束项：$0 \leqslant i \leqslant m$, $0 \leqslant j = \frac{m+n+1}{2}-i \leqslant n$ $\Rightarrow m \leqslant n$
 

1. 考虑约束项：若$m>n$，则将两个数组互换
2. 设 $imim=0, imax=m$，然后开始在[imin,imax]中进行搜索
3. 令 $i=\frac{imin+imax}{2}, j = \frac{m+n+1}{2}-i$，循环二叉树搜索 
4. &emsp; $B[j-1] > A[i]$且$i<m$：$i$过小，且i还能增大，令$imin = i+1$
5. &emsp; $A[i-1] > B[j]$且$i>0$：$i$过大，且i还能减小，令$imax = i-1$
6. &emsp; 循环结束，获取左半部分的最大值与右半部分的最小值  
  &emsp;&emsp; $i==0$:左半部分均由$B$提供，最大值为$B[j-1]$    
  &emsp;&emsp; $i==m$:右半部分均由$B$提供，最小值为$B[0]$  
  &emsp;&emsp; $j==0$:左半部分均由$A$提供，最大值为$A[i-1]$    
  &emsp;&emsp; $j==n$:右半部分均由$A$提供，最小值为$A[0]$  
  &emsp;&emsp; 否则，左半部分最大值 为 max(A[i-1], B[j-1])；右半部分最小值 为 min(A[i], B[j]) 
7. 计算中位数：  
  &emsp;当$m+n$为奇数时：median = maxLeft  
  &emsp;当$m+n$为偶数时：median = (maxLeft + minRight) / 2.0 

复杂度分析：
- 时间复杂度：$O(log(min(m,n))))$
- 空间复杂度：$O(1)$


```python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        m, n = len(nums1), len(nums2)
        if m>n:  nums1, nums2, m, n = nums2, nums1, n, m
        if m == 0: 
            if n%2 == 1: return nums2[n//2]
            else: return ( nums2[n//2-1] + nums2[n//2]) / 2.
        
        imin, imax = 0, m
        while (imin <= imax):
            i = (imin + imax) // 2
            j = (m+n+1)//2 - i
            if i < m and nums2[j-1] > nums1[i]: imin += 1
            elif i > 0 and nums1[i-1] > nums2[j]: imax -= 1
            else:
                if i == 0: maxLeft = nums2[j-1]
                elif j == 0: maxLeft = nums1[i-1]
                else: maxLeft = max(nums1[i-1], nums2[j-1])
                if (m+n)%2 == 1: return maxLeft                
                
                if i == m: minRight = nums2[j]
                elif j == n: minRight = nums1[i]
                else: minRight = min(nums1[i], nums2[j]) 
                return (maxLeft + minRight)/2.0
```
