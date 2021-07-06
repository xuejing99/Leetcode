# 选择排序（Selection Sort）


## 算法步骤
1. 首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置；
2. 再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾；
3. 重复第二步，直到所有元素均排序完毕。


## 算法特性
1. 运行时间和输入无关：无论数据是否有序，都会进行相同次数的比较
2. 数据移动是最少的：仅用交换 N 次，交换次数和数组的大小是线性关系
3. 不稳定排序：相同元素，经排序算法后，相对顺序会发生改变


## 复杂度分析
1. 时间复杂度：$O(n^2)$
2. 空间复杂度：$O(1)$


## 算法图解
![image](https://user-images.githubusercontent.com/48306154/124549630-0101f980-de62-11eb-8544-7badf67861ae.png)


## 代码实现
### Python 实现
```python
def selectionSort(arr):
    for i in range(len(arr) - 1):
        minIndex = i
        for j in range(i + 1, len(arr)):
            if arr[j] < arr[minIndex]:
                minIndex = j
        if i != minIndex:
            arr[i], arr[minIndex] = arr[minIndex], arr[i]
    return arr
```

## 备注：
1. 当 n 较小时，选择排序比冒泡排序的用时少（赋值语句的耗时大于比较语句的耗时）
