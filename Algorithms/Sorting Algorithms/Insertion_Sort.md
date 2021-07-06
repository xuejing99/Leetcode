# 插入排序（Insertion Sort）

## 算法步骤
1. 将第一待排序序列第一个元素看做一个有序序列，把第二个元素到最后一个元素当成是未排序序列；
2. 从头到尾依次扫描未排序序列，将扫描到的每个元素插入有序序列的适当位置。（如果待插入的元素与有序序列中的某个元素相等，则将待插入元素插入到相等元素的后面。）


## 算法特性
1. 插入排序所需的时间取决于输入中元素的初始顺序
2. 稳定排序


## 复杂度分析
1. 时间复杂度：$O(n^2)$
2. 空间复杂度：$O(1)$


## 算法图解
![image](https://user-images.githubusercontent.com/48306154/124556205-943f2d00-de6a-11eb-9fe6-5e342bcca763.png)


## 算法实现
### Python 实现
```python
def insertionSort(arr):
    for i in range(len(arr)):
        preIndex = i-1
        current = arr[i]
        while preIndex >= 0 and arr[preIndex] > current:
            arr[preIndex+1] = arr[preIndex]
            preIndex-=1
        arr[preIndex+1] = current
    return arr
```


## 备注
1. 插入排序适用于已经有部分数据已经排好，并且排好的部分越大越好。
2. 一般在输入规模大于1000的场合下不建议使用插入排序
