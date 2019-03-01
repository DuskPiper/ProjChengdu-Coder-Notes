# Traversal 遍历

本文讨论二叉树的先中后序遍历，和图的BFS和DFS遍历。

## 二叉树的普通遍历

###简述

遍历的前中后序其实指的是根节点所在的先后次序中的位置，而左一定先于右。

- 前序遍历：**根节点**->左子树->右子树
- 中序遍历：左子树->**根节点**->右子树
- 后序遍历：左子树->右子树->**根节点**

对于BST，中序遍历可以得到其值自然顺序

### 递归实现

```python
def preorder(root):
    print(root.val)
    preorder(root.left)
    preorder(root.right)

def inorder(root):
    inorder(root.left)
    print(root.val)
    inorder(root.right)
    
def postorder(root):
    postorder(root.left)
    postorder(root.right)
    print(root.val)
```

### 迭代实现

迭代实现主要在stack, queue的应用。 

```python
def preorder(root):
    stack = [root]
    while stack:
        cur = stack.pop(-1)
        print(cur.val)
        if cur.right: stack.push(cur.right)
        if cur.left: stack.push(cur.left)
        
def inorder(root):
    stack = []
    cur = root
    while stack or cur:
        while cur: # 一直往左找子树
            stack.push(cur)
            cur = cur.left
        cur = stack.pop(-1)
        print(cur.val)
        cur = cur.right

def postorder(root): # 翻转版前序遍历
    stack1, stack2 = [root], []
    postorderReversed = []
    while stack1:
        cur = stack1.pop(-1)
        if cur.left: stack1.append(cur.left)
        if cur.right: stack1.append(cur.right)
        stack2.append(cur.val)
    for val in postorderReversed[::-1]: print(val)
```



## 二叉树的层序遍历

**二叉树的层序遍历是面试经常会被考察的知识点，甚至要求当场写出实现过程。**

### 简述

层序遍历所要解决的问题很好理解，就是按二叉树从上到下，从左到右依次打印每个节点中存储的数据。

层序遍历与先序、中序、后序遍历不同。层序遍历用到了队列，而先、中、后序需要用到栈。因此，先、中、后序遍历可以采用递归方式来实现，而层序遍历则没有递归实现。

### 普通层序遍历

[LeetCode 102](https://leetcode.com/problems/binary-tree-level-order-traversal/)

每访问一层，是一层外循环，按顺序访问每个孩子并将孩子的孩子放入队列。循环结束时队列中是所有下一层的孩子节点。下次循环则重复此过程。

```python
def level(root):
    queue = [root]
    while queue:
        level_size = len(queue)
        for _ in range(level_size):
            node = queue.pop(0)
            print(node.val) # visit
            if node.left: queue.append(node.left)
            if node.right: queue.append(node.right)
```

### 花式层序遍历

#### zig-zag 层序遍历

[LeetCode 103](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

思路比较简单，就是不急着输出数据而是保存在叫做cur_layer的list中，每次外循环结束需要换层的时候，我们改变一次出list的方向。

```python
def level_zigzag(root):
    queue = [root]
    reverser = -1
    while queue:
        reverser = -reverser
        cur_layer = []
        level_size = len(queue)
        for _ in range(level_size):
            node = queue.pop(0)
            cur_layer.append(node)
            if node.left: queue.append(node.left)
            if node.right: queue.append(node.right)
        for node in cur_layer[::reverser]:
            print(node.val) # visit
```

#### Vertical 层序遍历

[LeetCode 314](Binary Tree Vertical Order Traversal)

定义一个性质：index，左子节点的index是其根的index-1，右则+1。index相同则属同一列，同列按level排序，所以在外进行一个普通层序遍历。

```python
def vertical_level(root):
    queue = [root]
    columns = {} # index -> [node_values]
    indexes = {} # node -> node.index
    indexes[root] = 0
    while queue:
        level_size = len(queue)
        for _ in range(level_size):
            node = queue.pop(0)
            index = indexes[node]
            # Now put(index, node.val) into columns
            if index not in columns: columns[index] = [node.val]
            else: columns[index].append(node.val)
            # push children and record their indexes
            if node.left: 
                queue.append(node.left)
                indexes[node.left] = index - 1
            if node.right:
                queue.append(node.right)
                indexes[node.right] = index + 1
        for k, v in sorted(columns.items()): print(v)
```



## 图的顶点遍历

从图的某个顶点出发访问遍图中所有顶点，且每个顶点仅被访问一次。

### DFS 深度优先搜索

#### 简述

1. 访问指定的起始顶点。

2. 若当前顶点的邻接顶点有未被访问的，则任选一个访问。反之则退回上一顶点。直到与起始顶点相通的全部顶点都访问完毕。通常使用一个集合保存已经访问过的节点，以防止重访。

3. （适用非连通图）若此时图中尚有顶点未被访问，则再选其中一个顶点作为起始顶点并访问之，转 2； 反之，遍历结束。

#### 实现

```python
# Recursion
def DFS_r(start):
    visited = []
    
    def dfs(node):
        print(node.val)
        visited.append(node)
        for next_node in node.neighbors:
            if not n in visited:
                dfs(next_node)
                
    if start: dfs(start)
        
# Iteration
def DFS_i(start):
    visited = []
    if start:
    	stack = [start]
    	while stack:
            node = stack[-1]
            for next_node in node.neighbors:
                if next_node not in visited:
                    print(next_node.val)
                    stack.append(next_node)
                    visited.append(next_node)
                else:
                    stack.pop()
```



### BFS 广度优先搜索

#### 简述

1. 访问指定的起始顶点。

2. 若当前顶点的邻居有未被访问的，则挨个全部访问。
3. 访问上一步所有邻居分别的全部邻居，仅限未被访问的。

4. 重复步骤2，直到全部顶点都被访问为止。

#### 性质

- **广度优先生成树：**如果将每次“前进”（纵深）路过的（将被访问的）结点和边都记录下来，就得到一个子图，该子图为以出发点为根的树，称为广度优先生成树。
- **最短路径：**从出发点广度优先遍历图，将得到出发点到它的各个可达到的路径。广度优先遍历得到的到各点的路径是最短路径（未考虑边权）。

#### 实现

```python
# Iteration
def BFS_i(start):
    visited = []
    if start:
    	queue = [start]
        while queue:
            node = queue.pop(-1)
            print(node.val)
            visited.append(node)
            for next_node in node.neighbors:
                if next_node not in visited:
                    queue.append(next_node)
                    
# Recursion
# BFS Recursion 不方便，思路是利用DFS的递归函数，递归时传参以记录当前深度，结束后按深度拼接
```



## 参考资料

- [图的遍历（搜索）算法（深度优先算法DFS和广度优先算法BFS）](https://www.cnblogs.com/kubixuesheng/p/4399705.html)
- [图的遍历之 深度优先搜索和广度优先搜索](https://www.cnblogs.com/skywang12345/p/3711483.html)
- [Python实现图的DFS（递归和非递归）和BFS](https://blog.csdn.net/weixin_40314737/article/details/80893507)
- [百度百科 广度优先遍历](https://baike.baidu.com/item/%E5%B9%BF%E5%BA%A6%E4%BC%98%E5%85%88%E9%81%8D%E5%8E%86)
- [二叉树的层序遍历详细讲解（附完整C++程序）](https://blog.csdn.net/FX677588/article/details/74276513)

