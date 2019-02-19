# Traversal 遍历

本文讨论二叉树的先中后序遍历，和图的BFS和DFS遍历。

## 二叉树的遍历

###简述

遍历的前中后序其实指的是根节点所在的先后次序中的位置，而左一定先于右。

- 前序遍历：**根节点**->左子树->右子树

- 中序遍历：左子树->**根节点**->右子树

- 后序遍历：左子树->右子树->**根节点**

### 递归实现

```python
def preorder(root):
    print(root.val)
    preorder(root.left)
    preorder(root.right)

def midorder(root):
    midorder(root.left)
    print(root.val)
    midorder(root.right)
    
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
        cur = stack.pop(0)
        print(cur.val)
        if cur.right: stack.push(cur.right)
        if cur.left: stack.push(cur.left)
        
def midorder(root):
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
    stack1 = [root]
    postorderReversed = []
    while stack1:
        cur = stack1.pop(-1)
        if cur.left: stack1.append(cur.left)
        if cur.right: stack1.append(cur.right)
        stack2.append(cur.val)
    for val in postorderReversed[::-1]: print(val)
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

