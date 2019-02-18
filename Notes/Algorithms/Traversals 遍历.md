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

