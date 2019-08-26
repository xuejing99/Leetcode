
# <p align="center"> 11. Container With Most Water</p>

## 1. Problem:
Given $n$ non-negative integers $a_1, a_2, ..., a_n$ , where each represents a point at coordinate $(i, a_i)$. $n$ vertical lines are drawn such that the two endpoints of line i is at $(i, a_i)$ and $(i, 0)$. Find two lines, which together with x-axis forms a container, such that the container contains the most water.  
Note: You may not slant the container and n is at least 2.

给定 $n$ 个非负整数$a_1, a_2, ..., a_n$，每个数代表坐标中的一个点 $(i, a_i)$ 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 $(i, a_i)$ 和 $(i, 0)$。找出其中的两条线，使得它们与$x$轴共同构成的容器可以容纳最多的水。  
说明：你不能倾斜容器，且 n 的值至少为 2。

## 2. Example:
1. Input: [1,8,6,2,5,4,8,3,7]  
Output: 49

## 3. Tags:
Array, Two Pointers

## 4. Solutions:

### 1. Two Pointers

思路：最初，我们考虑的是构成最外层线条的面积。为了使面积最大化，我们需要考虑更长的直线之间的面积。如果我们试图将指针移动到较长一行的内部，我们不会得到任何面积的增加，因为它受到较短一行的限制。但是，根据同样的论点，移动较短的行指针可能是有益的，尽管宽度减少了。之所以这样做，是因为移动较短的线的指针所得到的相对较长的线可能会克服宽度减少所造成的面积减少。

1. 定义左右指针，初始化最大盛水容量为0
2. 当 左指针！=右指针 循环：
3. &emsp;当height[left] < height[riight]时，左指针向右移动，反之，右指针向左移动
4. &emsp;更新最大盛水容量

复杂度分析：
- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$


```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        left, right = 0, len(height)-1
        mostWater = min(height[left], height[right])*(right-left)
        while left != right:
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
            mostWater = max(mostWater,  min(height[left], height[right])*(right-left))
        return mostWater
```

存在的问题：
- mostWater = 0 $\to$  mostWater = min(height[left], height[right])\*(right-left)
