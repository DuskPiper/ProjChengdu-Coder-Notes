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

1. 从数列中挑出一个元素，称为“基准”（pivot）。

2. 重新排序，所有比基准小的元素摆在基准前，所有比基准大的元素摆在基准后（相等则到任一边）。分割结束之后，该基准就处于数列的中间。称为分割（partition）操作。

3. 递归地（recursively）把小于基准值元素的子数列和大于基准值元素的子数列排序。

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
    def partition(l, r, pivot_index): # 整理成pivot前后大小关系分别一致
        swap[pivot_index, r] # 暂时移动pivot到末尾
        ptr, pivot = l, lst[r] # ptr相当于边界，ptr前的值一定小于pivot
        for i in range(l, r - 1):
            if lst[i] < pivot:
                swap[ptr, i] # 遇到小于pivot的值则swap到ptr前面去
                ptr += 1
        swap[ptr, r] # 归还pivot
        return ptr # 返回当前pivot index
    def recurse(l, r): # 递归地进行partition子序列大小为1或0
        if l < r:
            pivot_index = partition(l, r, (l + r) // 2)
            recurse(l, pivot_index - 1)
            recurse(pivot_index + 1, r)
    
    recurse(0, len(lst) - 1)
    return lst

# Iteration
def quicksort_i(lst):
    def partition(l, r, pivot_index): # 同上
        swap[pivot_index, r] # 暂时移动pivot到末尾
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
        if pivot_index - 1 > l: stack.push((l, pivot_index - 1)) # 入栈pivot左子序列
        if pivot_index + 1 < r: stack.push((pivot_index + 1, r)) # 入栈pivot右子序列
    return lst
```



## Merge Sort  归并排序

### 简述

归并排序是建立在归并操作上的一种有效的排序算法。该算法是采用分治法(Divide and Conquer)的一个典型应用。归并操作(Merge)，也叫归并算法，指的是将两个已经排序的序列合并成一个序列的操作。归并排序算法依赖归并操作。

### 思路

1. 把 n 个记录看成 n 个长度为 l 的有序子表。
2. 进行两两归并使记录关键字有序，得到 n/2 个长度为 2 的有序子表。
3. 重复第 2 步直到所有记录归并成一个长度为 n 的有序表为止。

### 实现

```python
# Recursion
def merge(l1, l2): # 归并两个有序list
    result = []
    while l1 and l2:
    	if l1[0] < l2[0]: result.append(l1.pop(0))
        else: result.append(l2.pop(0))
	return result + l1 + l2 # l1 l2至少有一个为空，所以连接先后顺序不重要

def merge_sort_r(lst):
    if len(lst) <= 1: return lst
    mid = len(lst // 2)
    l = merge_sort_r(lst[:mid])
    r = merge_sort_r(lst[mid:])
    return merge(l, r)
    
# Iteration
def merge_sort_i(lst):
    step = 1
    while step < len(lst):
        l = 0 # 子序列左index
        while l + step < len(lst):
            mid= l + step # 中index
            r = min(len(lst), l + step + step) # 子序列右index, 用min保证不out of range
            # Now merge: merge(l, mid, r)
            merged, l1, l2 = [], lst[l:mid], lst[mid:r]
            while l1 and l2:
                if l1[0] < l2[0]: merged.append(l1.pop(0))
                else: merged.append(l2.pop(0))
            lst = lst[:l] + merged + l1 + l2 + lst[r:]
            # end of merge
            l = r # 往右merge下面两个序列
        step *= 2 # 循环完一次整个序列，下次循环跟粗粒度的同序列
```



## Shell Sort 希尔排序

### 简述

希尔排序是[插入排序](https://zh.wikipedia.org/wiki/%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)的一种更高效的改进版本。是非稳定排序算法。

希尔排序是基于插入排序的以下两点性质而提出改进方法的：

- 插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到[线性排序](https://zh.wikipedia.org/w/index.php?title=%E7%BA%BF%E6%80%A7%E6%8E%92%E5%BA%8F&action=edit&redlink=1)的效率。
- 但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位。

### 思路

将数组列在一个表中并对列排序（用插入排序）。重复这过程，不过每次用更长的列来进行。最后整个表就只有一列了。将数组转换至表是为了更好地理解这算法，算法本身仅仅对原数组进行排序。列的数量即所谓步长，只要终步长为1则排序有效，而步长的选取对排序效率有极大影响，下面为三种常见选取方式。

| 步长序列                                                     | 最坏情况下复杂度                                             |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| ![{n/2^i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/6c8f4d868de17325e948844f4e4fc58fc0b0393a) | ![\mathcal{O}](https://wikimedia.org/api/rest_v1/media/math/render/svg/d6ae2ed4058fb748a183d9ada8aea50a00d0c89f)![(n^2)](https://wikimedia.org/api/rest_v1/media/math/render/svg/52376d02d45e1ac7c4cd8d8208c2f4ae104e3098) |
| ![2^k - 1](https://wikimedia.org/api/rest_v1/media/math/render/svg/76af5b5bc7d056faef7eef0c6efeea3a16adc156) | ![\mathcal{O}](https://wikimedia.org/api/rest_v1/media/math/render/svg/d6ae2ed4058fb748a183d9ada8aea50a00d0c89f)![(n^{3/2})](https://wikimedia.org/api/rest_v1/media/math/render/svg/0d30640651e80d1f38232384fdf407b253670801) |
| ![2^i 3^j](https://wikimedia.org/api/rest_v1/media/math/render/svg/f2f4f16513454e80cf1f92f279d81914a8ee1e45) | ![\mathcal{O}](https://wikimedia.org/api/rest_v1/media/math/render/svg/d6ae2ed4058fb748a183d9ada8aea50a00d0c89f)![( n\log^2 n )](https://wikimedia.org/api/rest_v1/media/math/render/svg/de727f8c182ebfc7dd8fe3ac86b2b15ec5dd6d5d) |

已知的最好步长序列是由Sedgewick提出的(1, 5, 19, 41, 109,...)

### 实现

```python
def shell_sort(lst):
    gap = len(a) // 2
    while gap:
        for i in range(gap, len(lst)): # 插入排序，
            for j in range(i, 0, -gap):
                if a[j] < a[j - gap]:
                    a[j], a[j - gap] = a[j - gap], a[j]
                else: break
        gap /= 2
```



## Heap Sort 堆排序

### 简述

堆是具有以下性质的完全二叉树：每个结点的值都大于或等于其左右孩子结点的值，称为大顶堆；或每个结点的值都小于或等于其左右孩子结点的值，称为小顶堆。左右子节点之间无固定大小关系。

堆排序是利用堆这种数据结构而设计的一种排序算法，堆排序是一种选择排序，它的最坏，最好，平均时间复杂度均为O(nlogn)，它也是不稳定排序。

### 思路

以升序为例，将大顶堆按层放入数列后，会满足：

```psudocode
arr[i] >= arr[2i+1] && arr[i] >= arr[2i+2]  
```

算法实现步骤是：[图文介绍](https://www.cnblogs.com/chengxiao/p/6129630.html)

1. 将给定无序序列构造成一个大顶堆（降序则为小顶堆）。
2. 将堆顶元素与末尾元素交换，以此将最大元素"沉"到数组末端。此时当前最大元素被记录，可理解为放入另一个已排序序列。
3. 重新调整结构，使其满足堆定义。此时顶部最大的元素是排除了之前更大元素的最大。
4. 继续交换堆顶元素与当前末尾元素，反复执行调整+交换步骤，直到整个序列有序。

