# Dynamic Programming 动态规划

### 经典实例：斐波那契数列

```pseudocode
Fibonacci (0) = 1
Fibonacci (1) = 1
Fibonacci (n) = Fibonacci(n - 1) + Fibonacci(n - 2) if n >= 2
```

常见的思路是递归：

```python
def fib(n):
    if n < 0: return 0
    if n <= 1: return 1
    else: return fib(n - 1) + fib(n - 2)
```

在执行的时候会重复调用相同的函数，浪费了资源。把执行过的子节点保存起来，后面要用就查表的话可节约时间。

## DP的两种形式

### 自上而下/备忘录法

仍然使用经典的递归解决。不过创建n+1大小的数组来保存求出的斐波拉契数列中每一个值，递归时如果发现前面fib(n)的值计算出来了就不再计算，如果未计算出来，则计算出来后保存在数组中。

### 自下而上

从fib(2)开始计算并增长，一直到计算出fib(n)。

## DP的核心/适用情况

### 最优子结构

刻画最优解的结构，如果问题的解结构包含其子问题的最优解，就称此问题具有最优子结构性质。

### 重叠子问题

在动态规划中找到重复进行的子问题并采取手段避免这种局面的出现从而节省计算资源。

### 无后效

某阶段状态一旦确定，就不受这个状态以后决策的影响。也就是说，某状态以后的过程不会影响以前的状态，只与当前状态有关。

## DP的求解思路

1. 划分出子问题，并确定子问题的状态表示
2. 确定决策并确定状态转移方程，比如递归的调用方法或者累计计数的对象集
3. 确定边界条件

## LeetCode DP题目

- **53**. Maximum Subarray
- **62**. Unique Paths
- **63**. Unique Paths II
- **64**. Minimum Path Sum
- **70**. Climbing Stairs
- **72**. Edit Distance
- **87**. Scramble String
- **91**. Decode Ways
- **97**. Interleaving String
- **115**. Distinct Subsequences
- **120**. Triangle
- **139**. Word Break
- **140**. Word Break II
- **152**. Maximum Product Subarray
- **174** . Dungeon Game
- **198**. House Robber
- **213**. House Robber II
- **221**. Maximal Square
- **712**. Minimum ASCII Delete Sum for Two Strings
- **718**. Maximum Length of Repeated Subarray
- **799**. Champagne Tower
- **818**. Race Car



## 参考资料

- [LeetCode动态规划](https://www.hrwhisper.me/leetcode-dynamic-programming/)
- [五大算法之二：动态规划](https://www.cnblogs.com/steven_oyj/archive/2010/05/22/1741374.html)