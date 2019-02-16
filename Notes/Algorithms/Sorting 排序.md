# Sorting 排序

## 综述

排序算法众多，这里只挑常见的几种算法作分析和coding。下面是常见排序算法的复杂度。

![排序算法复杂度比较](https://raw.githubusercontent.com/DuskPiper/ProjChengdu-Coder-Notes/master/Illustration/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95%E5%A4%8D%E6%9D%82%E5%BA%A6.png?token=Aih2uIWb3QtD5SEyQ0oYX7NVBTn2gwXiks5ccHt2wA%3D%3D)



## Quick Sort 快速排序

### 简述

平均排序n个项目要O(nlog n)}次比较。最坏则需要O(n^2)次比较，但并不常见。事实上，快速排序通常明显比其他算法更快，因为它的内部循环可以在大部分的架构上很有效率地达成。

### 思路

使用分治法（Divide and conquer）策略来把一个序列（list）分为两个子序列（sub-lists）。

步骤为：

- 从数列中挑出一个元素，称为“基准”（pivot）。
- 重新排序，所有比基准小的元素摆在基准前，所有比基准大的元素摆在基准后（相等则到任一边）。分割结束之后，该基准就处于数列的中间。称为分割（partition）操作。
- 递归地（recursively）把小于基准值元素的子数列和大于基准值元素的子数列排序。
- 递归到最底部时，数列的大小是零或一，也就是已经排序好了。这个算法一定会结束，因为在每次的迭代（iteration）中，它至少会把一个元素摆到它最后的位置去。

### 实现

```python
# Recursion
def quicksort_r(lst):
    if len(lst) <= 1: return lst
    pivot = lst[len(lst) // 2] # pivot别选list[0]或[-1]，已排序情况会带来最坏复杂度
    greater, less, equal = [], [], []
    for element in lst:
        if element > pivot: greater.append(element)
        elif element < pivot: less.append(element)
        else: equal.append(element)
	return quicksort_r(less) + equal + quicksort_r(greater)

# Recursion, In-place, saves RAM
def quicksort_r_i(lst):
    if len(lst) <= 1: return lst
    def swap(i, j): lst[i], lst[j] = lst[j], lst[i]
    def partition(l, r, pivotIndex): # 整理成pivot前后大小关系分别一致
        swap[pivotIndex, r] # 暂时移动pivot到末尾
        ptr, pivot = l, lst[r] # ptr相当于边界，ptr前的值一定小于pivot
        for i in range(l, r - 1):
            if lst[i] < pivot:
                swap[ptr, i] # 遇到小于pivot的值则swap到ptr前面去
                ptr += 1
        swap[ptr, r] # 归还pivot
        return ptr # 返回当前pivot index
    def recurse(l, r): # 递归地进行partition子序列大小为1或0
        if l < r:
            pivotIndex = partition(l, r, (l + r) // 2)
            recurse(l, pivotIndex - 1)
            recurse(pivotIndex + 1, r)
    
    recurse(0, len(lst) - 1)
    return lst

# Iteration
def quicksort_i(lst):
    def partition(l, r, pivotIndex): # 同上
        swap[pivotIndex, r] # 暂时移动pivot到末尾
        ptr, pivot = l, lst[r] # ptr相当于边界，ptr前的值一定小于pivot
        for i in range(l, r - 1):
            if lst[i] < pivot:
                swap[ptr, i] # 遇到小于pivot的值则swap到ptr前面去
                ptr += 1
        swap[ptr, r] # 归还pivot
        return ptr # 返回当前pivot index
    
    stack = []
    stack.push((0, len(lst - 1))) # 按tuple(left, right)的格式入栈整个lst
    while stack:
        l, r = stack.pop() # 出栈一段序列的头尾index
        pivotIndex = partition(l, r, (l + r) // 2) # 对序列进行partition
        if pivotIndex - 1 > l: stack.push((l, pivotIndex - 1)) # 入栈pivot左子序列
        if pivotIndex + 1 < r: stack.push((pivotIndex + 1, r)) # 入栈pivot右子序列
    return lst
    
```



