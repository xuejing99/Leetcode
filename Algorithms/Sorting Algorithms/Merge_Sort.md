# 归并排序（Merge Sort）

## 简介
归并排序是建立在归并操作上的一种有效，稳定的排序算法，该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。  
将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。  
若将两个有序表合并成一个有序表，称为二路归并。  


## 算法步骤
1. 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列；
2. 设定两个指针，最初位置分别为两个已经排序序列的起始位置；
3. 比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置；
4. 重复步骤 3 直到某一指针达到序列尾；
5. 将另一序列剩下的所有元素直接复制到合并序列尾。


## 复杂度分析
1. 时间复杂度：$O(nlogn)$
2. 空间复杂度：$O(n)$

分析：对半拆分，经过log⁡n  次，将序列拆分为仅包含一个元素；对于每一层的元素合并，需对n个元素都进行操作，复杂度为O(n)，故，总的复杂度为O(nlog n)


## 算法图解
![image](https://user-images.githubusercontent.com/48306154/124686932-8fc85200-df06-11eb-8fb2-fac05f1a2623.png)



## 代码实现
### Python 实现
```python
def mergeSort(arr):
    import math
    if(len(arr)<2):
        return arr
    middle = math.floor(len(arr)/2)
    left, right = arr[0:middle], arr[middle:]
    return merge(mergeSort(left), mergeSort(right))

def merge(left,right):
    result = []
    while left and right:
        if left[0] <= right[0]:
            result.append(left.pop(0))
        else:
            result.append(right.pop(0));
    while left:
        result.append(left.pop(0))
    while right:
        result.append(right.pop(0));
    return result
```


## 备注
1. 速度仅次于快速排序，为稳定排序算法，一般用于对总体无序，但是各子项相对有序的数列


## 优化
1. 当递归到规模足够小时，利用插入排序；
2. 归并前判断一下是否还有必要归并
3. 只在排序前开辟一次空间
