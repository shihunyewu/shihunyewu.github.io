---
layout:     post
title:      "Seven-Methods-Traverse-BinaryTree"
subtitle:   " \"遍历二叉树的七种方法\""
date:       2018-04-03 11:11:11
author:     "shihunyewu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: false
tags:
    - Data Structure
---

> "七种遍历二叉树的方法"

做 LeetCode 的时候经常会碰到二叉树这种数据结构，也经常碰到需要遍历二叉树，有时候需要使用非递归，有时需要使用递归，再有时需要层次遍历。本文总结了七种遍历方式。

### 构造一棵二叉树

先用简单的方法构造一棵二叉树

```
class Node:
    def __init__(self,val):
        self.val = val
        self.left = None
        self.right = None

# 构造一棵树
def construct_tree(treelist,i):
	"""
	:type treelist:List[Node] 用来创建二叉树的顺序表，比如[1,2,3,null,null,4,5]
	:type i:int 待处理结点在顺序表中的索引
	:rtype: Node
	"""
    if i <len(treelist) and tree_list[i] != None:
        node = Node(treelist[i])
    else:
        return
	# 使用顺序表存储的二叉树
	# 根节点索引为0时，顺序表中的第 i 个元素的左孩子的索引为 2*i+1，右孩子的索引为2*i+2
    if 2*i+1<len(treelist):
        node.left = construct_tree(treelist,2*i+1)
    if 2*i+2<len(treelist):
        node.right = construct_tree(treelist,2*i+2)
    return node
```

### 递归遍历

二叉树遍历递归写法非常简单，先序遍历，先将根的值输出，然后对左右孩子调用本函数。中序遍历，就先对左孩子调用本函数，再输出根值，再对右孩子调用本函数。而后序遍历，先对左右孩子调用本函数，再输出根的值。

#### 前序遍历

```python
def pre(node):
    if node:
        print(node.val)
    if node.left:
        pre(node.left)
    if node.right:
        pre(node.right)
```

#### 中序遍历

```python
def mid(node):
    if node:
        if node.left:
            mid(node.left)
        print(node.val)
        if node.right:
            mid(node.right)
```

#### 后序遍历

```python
def last(node):
    if node:
        if node.left:
            last(node.left)
        if node.right:
            last(node.right)
        print(node.val)
```

### 非递归遍历

非递归遍历需要借用栈这种数据结构，python 中的 list 提供了 append 方法（从末尾添加一个元素）和 pop 方法（参数为空的时候，默认从结尾删除一个元素并将其作为返回值），分别对应入栈和出栈这两个操作。
使用栈来实现递归，其本质就是回溯。一直向下寻找符合条件的元素，如果元素不符合条件，那么处理之后，回溯到上一个元素，再使用相同的策略，直到栈为空为止。

#### 无递归先序遍历

无递归先序遍历，边输出边往下寻找左结点。如果左结点为空，那么就开始考虑右结点。

```python
def pre_no_recursion(node):
    if node == None:
        return
    s = []
    while s or node:
        while node:
            print(node.val)
            s.append(node)
            node = node.left
        node = s.pop()
        node = node.right
    pass
```

#### 无递归中序遍历

和先序遍历相似，但是中序遍历是先往下寻找，找到之后，再输出，然后再考虑右结点。

```python
# 左 根 右
def mid_no_recursion(node):
    if node == None:
        return
    s = []
    while s or node:
        while node:
            s.append(node)
            node = node.left
        node = s.pop()
        print(node.val)
        node = node.right
    pass
```
	
#### 无递归后序遍历

后序无递归遍历比前面的要难一点，需要用两个栈来完成，其中一个栈负责遍历，另外一个栈负责按后序的倒序保存元素。

```python
def last_no_recursion(node):
    if node == None:
        return
    s = []
    rs = []
    s.append(node)
    while s:
        node = s.pop()
        if node.left:
            s.append(node.left)
        if node.right:
            s.append(node.right)
		# 宏观上看，具有相同父结点的左右结点，右孩子结点最早进入 rs 栈中
        rs.append(node)
    while rs:   # 倒序输出，原先先进栈的右孩子结点会在左孩子结点之后输出
        print(rs.pop().val)
    pass
```

#### 层次遍历

层次遍历主要是使用队列这个数据结构，依次将队列中结点不为空的子结点添加到队列中，同时将处理过的结点出队。

```python
def level_traverse(self, root):
        """利用队列实现树的层次遍历"""
        if root == None:
            return
        myQueue = []
        node = root
        myQueue.append(node)
        while myQueue:
            node = myQueue.pop(0)  # pop(0)表示删除索引为0的元素，相当于出队
            print node.elem,
            if node.lchild != None:
                myQueue.append(node.lchild)
            if node.rchild != None:
                myQueue.append(node.rchild)
```

[资料](https://blog.csdn.net/bone_ace/article/details/46718683)

> 后记：自己经历了上一个周的混沌时期，这个周稳住了自己的心态，重新开始保持自己以前的生活规律。