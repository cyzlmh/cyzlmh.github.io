---
layout: post
title:  "[读书笔记]Data Structures and Algorithms"
date:   2018-01-16 00:00:00 +0800
categories: post
tag: Python
---

* TOC
{:toc}

# Motive

**Abstraction**

- procedural abstraction
- data abstraction => ADT


**Abstract Data Type (ADT)**

数据抽象后可以被视为黑匣子，使用者通常可以对它进行下面4种操作

- Constructor
- Accessor
- Mutator
- Iterator




# Algorithm Analysis

**一次计算**

CPU的一次计算：32位/64位整数的一次加减

*长整数的加减是线性复杂度而不是常数复杂度*

**Big-O Notation--O(n)**

表明算法需要时间根据数据量的规模变化而变化的情况

**常见的O(n)的函数**

| f(.)     | Common Name |
| -------- | ----------- |
| 1        | constant    |
| log(n)   | logarithmic |
| n        | linear      |
| n*log(n) | log linear  |
| n**2     | quadratic   |
| n**3     | cubic       |
| a**n     | exponential |

![big-O_functions](/assets/Data Structures and Algorithms/big-O_functions.png)

**Python中List操作的O(n)**

| List Operation | Worst Case |
| -------------- | ---------- |
| v = list()     | O(1)       |
| v = [0] * n    | O(n)       |
| v[i] = x       | O(1)       |
| v.append(x)    | O(n)       |
| v.extend(w)    | O(n)       |
| v.insert(x)    | O(n)       |
| v.pop()        | O(n)       |
| traversal      | O(n)       |



# ADTs

**数据在内存中的表现**

- 内存中连续储存的一块区域
  - 例子：**Array**，Set，List，Map
- 内存中非连续储存的区域，每个数据下储存关联数据的位置
  - 例子：**Linked List**，Stack，Deque，Tree，Graph

## Array

固定长度的数组

**Methods**

- *Array(size)*：新建一个固定长度的Array
- *length()*：返回它的长度值
- *getitem(index)*：返回指定位置的值
- *setitem(index, value)*：修改指定位置的值
- *clearing(value)*：重置整个Array为指定值
- *iterator()*：创建并返回一个用于遍历所有元素的迭代器

## Python List

长度可以变化的Array，长度为创建时包含元素数的两倍，当空间用完时会生成一个新的长度为当前元素数两倍的List，并将元素复制过去。

**create**

![python_list_create](/assets/Data Structures and Algorithms/python_list_create.png)

**append**

![python_list_append](/assets/Data Structures and Algorithms/python_list_append.png)

**extend**

![python_list_extend](/assets/Data Structures and Algorithms/python_list_extend.png)

**insert**

![python_list_insert](/assets/Data Structures and Algorithms/python_list_insert.png)

**pop**

![python_list_pop](/assets/Data Structures and Algorithms/python_list_pop.png)

**slice**

![python_list_slice](/assets/Data Structures and Algorithms/python_list_slice.png)

## Two-Dimensional Array

一般有两种实现方法：

1. 使用一维Array存储，在读取时对下标进行换算通过二维下标找到在一位Array中的位置
2. 每行都用一个一维Array存储，在用一个一维Array储存每一行的指针

**Methods**

- *Array2D(n_row, n_col)*：创建新的实例
- *num_row()*：返回行数
- *num_col()*：返回列数
- *clear(value)*：重置数组
- *get_item(i, j)*：返回指定位置的值
- *set_item(i, j, value)*：修改指定位置的值

## Set

没有重复值的集合

**Methods**

- *Set()*：创建实例
- *length()*：返回长度
- *contain(element)*：是否包含元素
- *add(element)*：添加新元素，如果已存在则不改变集合
- *remove(element)*：删除元素，如果不存在则报错
- *equal(setB)*：是否与另一集合相等
- *is_subset_of(setB)*：是否是另一集合的子集
- *union(setB)*：返回并集
- *intersect(setB)*：返回交集
- *difference(setB)*：返回差集
- *iterator()*：返回迭代器

**Efficiency**

- implementation using an unordered list

| Operation      | Worst Case |
| -------------- | ---------- |
| s = Set()      | O(1)       |
| len(s)         | O(1)       |
| x in s         | O(n)       |
| s.add(x)       | O(n)       |
| s.is_subset(t) | O(n^2)     |
| s == t         | O(n^2)     |
| s.union(t)     | O(n^2)     |
| traversal      | O(n)       |

## Map

字典，通常的实现方式包括
1. 定义一个包含键-值对的类，用列表/链表储存它们
2. 使用Hash Table

**Methods**

- *Map()*：创建实例
- *length()*：返回长度
- *contain(key)*：是否包含键
- *add(key, value)*：添加一组新的键/值并返回*True*，如果键已存在则替换为新值并返回*False*
- *remove(key)*：删除键/值，如果不存在则报错
- *value_of(key)*：返回键对应的值
- *iterator()*：返回迭代器
- *_find_position(key)*：helper mothed，查找键的位置

## Linked List

每个元素储存下一个元素的位置

**双向链表**：每个元素储存下一个和上一个元素的位置

**有序链表**：按元素大小排序的链表

**循环链表**：最后一个元素链接第一个元素

## Stack

后进先出(LIFO)的序列

可以使用序列或链表实现

**Methods**
- *Stack()*：创建实例
- *is_empty()*：是否为空
- *length()*：返回长度
- *pop()*：删除并返回最上面的元素
- *peek()*：返回最上层元素但是不修改Stack
- *push(item)*：添加一个元素至顶部

## Queue

先进先出(FIFO)的序列

可以使用序列或链表实现

**Methods**
- *Queue()*：创建实例
- *is_empty()*：是否为空
- *length()*：返回长度
- *enqueue(item)*：添加一个元素至队尾
- *dequeue()*：删除并返回队列最前的元素

**Priority Queue**：有优先级的队列，优先级高的元素先出队

## Tree

一些概念：
- **node**：结点
- **root**：根结点
- **parent**：父结点
- **children**：子结点
- **interior node**：内结点，有子结点的结点
- **leaf**：叶结点，没有子结点的结点
- **path**：从一个结点到另一个结点的路径
- **subtree**：子树
- **relative**：亲戚
  - **ancestor**：祖先
  - **decendant**：后代
- **size**：结点的个数
- **level**：唯一同一层的所有结点
- **depth**：距离根结点的距离
- **width**：结点数最多的一层的结点个数

**Full Binary Tree**

所有的interior node都有两个子结点的树

![full_binary_tree](/assets/Data Structures and Algorithms/full_binary_tree.png)

**Perfect Binary Tree**

所有leaf都在最下一层

![perfect_binary_tree](/assets/Data Structures and Algorithms/perfect_binary_tree.jpg)

**Complete Binary Tree**

从根部到倒数第二层为perfect binary tree，最下一层由左至右排

![perfect_binary_tree](/assets/Data Structures and Algorithms/complete_binary_tree.png)

### Implementation

类似链表的实现，每个结点储存数据和左右子结点的指针

```python
class _BinTreeNode:
	def __init__(self, data):
		self.data = data
        self.left = None
        self.right = None
```



### Traversal

**Preorder Traversal**

```python
def preorder_trav(subtree):
    if subtree is not None:
        print(subtree.data)
        preorderTrav(subtree.left)
        preorderTrav(subtree.right)
```

![preorder_traversal](/assets/Data Structures and Algorithms/preorder_traversal.png)

**Inorder Traversal**

```python
def preorder_trav(subtree):
    if subtree is not None:
        preorderTrav(subtree.left)
        print(subtree.data)
        preorderTrav(subtree.right)
```

![inorder_traversal](/assets/Data Structures and Algorithms/inorder_traversal.png)

**Postorder Traversal**

```python
def preorder_trav(subtree):
    if subtree is not None:
        preorderTrav(subtree.left)
        preorderTrav(subtree.right)
        print(subtree.data)
```

![postorder_traversal](/assets/Data Structures and Algorithms/postorder_traversal.png)

**Breath-First Traversal**

可以使用队列实现

![breath_first_traversal](/assets/Data Structures and Algorithms/breath_first_traversal.png)

### Heap

按照结点储存的值的大小来排列的complete binary tree

例如一个max-heap，每个父结点的值都大于其子结点

![heap](/assets/Data Structures and Algorithms/heap.png)

**Insertion**

时间复杂度为 $O(log n)$

![heap_insert](/assets/Data Structures and Algorithms/heap_insert.png)

**extract**

![heap_extract](/assets/Data Structures and Algorithms/heap_extract.png)

### Binary Search Tree

搜索树指每个结点包含一个键-值对，结点根据键的值来组织

二叉搜索树指所有键值小于当前结点的都位于它的左子树上，所有键值大于当前结点的都位于它的右子树上

![binary_search_tree](/assets/Data Structures and Algorithms/binary_search_tree.png)

**Search**

反复对比查找值与当前结点键值的大小，若较小则往左子结点走，若较大则往右子结点走，若相等则查找成功

![binary_search_tree_search](/assets/Data Structures and Algorithms/binary_search_tree_search.png)

**Min and Max**

一直向左走直到最左的leaf或一直向右走直到最右的leaf

![binary_search_tree_min](/assets/Data Structures and Algorithms/binary_search_tree_min.png)

**Insert**

先按照搜索的方式找到其位置，再将结点插入

![binary_search_tree_insert](/assets/Data Structures and Algorithms/binary_search_tree_insert.png)

**Remove**

1. 若要移除的是leaf，则找到它的父结点然后移除

![binary_search_tree_remove_1](/assets/Data Structures and Algorithms/binary_search_tree_remove_1.png)

2. 若要移除的结点有一个子结点，则将它移除后链接它的父结点和它的子结点

![binary_search_tree_remove_2](/assets/Data Structures and Algorithms/binary_search_tree_remove_2.png)

3. 若要移除的结点有两个子结点，先找到它的继承者（predecessor-它的左子树的最右结点，successor--它的右子树的最左结点），将它和继承者交换位置，再将其移除

![binary_search_tree_successor](/assets/Data Structures and Algorithms/binary_search_tree_successor.png)

![binary_search_tree_remove_3](/assets/Data Structures and Algorithms/binary_search_tree_remove_3.png)

**Efficiency**

![binary_search_tree_efficiency](/assets/Data Structures and Algorithms/binary_search_tree_efficiency.png)

### AVL Tree

平衡的搜索二叉树。不平衡的二叉搜索树导致在最差的情况下会成为类似链表导致搜索效率下降，平衡二叉树在每次插入或删除时对树进行再平衡，保证搜索效率。

平衡是指每个结点的左子树与右子树的高度相差不超过1

![AVL_tree](/assets/Data Structures and Algorithms/AVL_tree.png)

**Rotation**

当通过类似heap的方式插入结点时，可能会导致树的不平衡，这是某个结点的左右子树高度相差2，这时可以通过rotation解决

![AVL_tree_rotation](/assets/Data Structures and Algorithms/AVL_tree_rotation.png)

新插入结点后的不平衡有四种情况：

1. 插入后P变为>>，而C变为>
  ![AVL_tree_rotation_1](/assets/Data Structures and Algorithms/AVL_tree_rotation_1.png)

2. 插入后P变为>>，而C变为<
  ![AVL_tree_rotation_2](/assets/Data Structures and Algorithms/AVL_tree_rotation_2.png)

3. 另外两种情况是前面两种的镜像

![AVL_tree_rotation_3](/assets/Data Structures and Algorithms/AVL_tree_rotation_3.png)

**Insert**

先按heap的方式插入，在对树进行rotation从而达到rebanlance

![AVL_tree_insert](/assets/Data Structures and Algorithms/AVL_tree_insert.png)

**Remove**

删除结点后，用rotation的方式对树进行再平衡

![AVL_tree_remove](/assets/Data Structures and Algorithms/AVL_tree_remove.png)

### 2-3 Tree

![2-3_tree](/assets/Data Structures and Algorithms/2-3_tree.png)

## Graph

<!--待续-->

# Search & Sorting

## Search

### Linear search

遍历数据中各元素只到找到目标

- 时间复杂度为O(n)

### Binary search

适用于有序序列

- 时间复杂度为O(log n)

### Hash table

将一个序列用一个函数或表映射至一个长度有限的数集中[1, 2, ..., n]，在查找时直接读取array[k]的元素，若存在则查找成功，否则则为失败

- *例子*：将序列[103, 116, 133, 107, 101, 155, 105, 118, 134]通过函数$h(key)=key\ mod\ M$映射至长度为M的array中
- 当load factor在1/3到2/3之间时，hash操作平均时间为$O(1)$

#### Probing

- Linear probing：当发生碰撞（collision）时，元素按顺序向后插入。查找时顺序向后查找，直到遇到空值，说明查找失败。

  $$slot=(home+i)\ mod\ M$$

  $home$是元素本来应该在的位置，$i$是需要往后调整的次数

  ![linear_probing](/assets/Data Structures and Algorithms/linear_probing.png)

  - 会引起clustering现象，即表中连续的元素会越排越长

- Modified linear probing

  $$slot=(home+c*i)\ mod\ M$$

  即间隔着c的步长向后插入，$c$应与$M$互质

  ![linear_probing](/assets/Data Structures and Algorithms/modified_linear_probing.png)

- Quadratic probing

  $$slot=(home+i^2)\ mod\ M$$

  ![quadratic_probing](/assets/Data Structures and Algorithms/quadratic_probing.png)

  - 会减少primary clustering，但是会引起secondary clustering，即若两个元素对应同一个$home$，他们会遵循一样的路径向后排

- Double hashing

  后顺的步长通过对$key$的第二次hash确认

  $$slot=(home+i*hp(hey))\ mod\ M$$

  $$hp(key)=1+(key\ mod\ P)$$

  - 可以同时减少primary clustering和secondary clustering，在解决碰撞时最为常用
  - 表的长度应为质数

#### Rehashing

当表的元素增加、可用空间越来越少时，需要重新生成新的、更长的表，并对所有元素重新hash，这个过程叫作rehash。

![rehashing](/assets/Data Structures and Algorithms/rehashing.png)

- 当前元素个数与哈希表的长度之比称为load factor
- load factor在2/3以下效率较高，超过该值一般需要rehash
- 新生成的表的长度一般为$2M+1$，$M$为原表长度

#### Seperate Chaining
当不同元素在表中发生碰撞时，让这些元素共用这一个位置，并用链表将他们连接起来
![seperate_chaining](/assets/Data Structures and Algorithms/seperate_chaining.png)

#### Hash Functions
Hash functions的意思是将数据的搜索匙的集合映射至长度一定的整数下标中，常用的hash function有：

- Division/取余：$h(key)=key\ \%\ M$
- Trunction：对于一个大的整数，取其中某些位数上的数字，例如4873152去其中的8、1、2组成812
- Folding：将key分成多个部分再加/乘起来，例如将4873152分成48、731、52再加起来得831
- Hashing Strings：将各个字符转化为ASCII码，再相加，或用多项式相加

## Sorting

###  Bubble Sort

依次对比相邻两个元素，将较大者交换至后面，这样一轮之后可以保证最后一个元素为最大值，对前面n-1个元素进行相同操作，直至排序完成。

```python
def bubble_sort(ls):
    n = len(ls)
    for i in range(n-1):
        for j in range(n-i-1):
            # swap adjacent value if the former is larger
            ls[j], ls[j+1] = min(ls[j], ls[j+1]), max(ls[j], ls[j+1])
    return ls
```

### Selection Sort

每轮挑选未排序序列中的最小值排至队首，重复操作直至排序结束。

```python
def selection_sort(ls):
    n = len(ls)
    for i in range(n-1):
        small_idx = i
        # find the smallest value in the rest of list
        for j in range(i+1, n):
            if ls[j] < ls[small_idx]:
                small_idx = j
        # sway the current value with the smallest
        ls[i], ls[small_idx] = ls[small_idx], ls[i]
    return ls
```

### Insertion Sort

选取未排序序列中的第一个元素，将其与排好序的序列中逐个元素对比以确定取所对应的位置，将其插入位置，重复操作直至排序结束。

```python
def insertion_sort(ls):
    n = len(ls)
    for i in range(1, n):
        for j in range(i):
            # find the position of current value
            if ls[j] > ls[i]:
                # insert the value in the postion
                tmp = ls[i]
                ls.pop(i)
                ls.insert(j, tmp)
                break
    return ls
```

```python
def insertion_sort_2(ls):
    n = len(ls)
    for i in range(1, n):
        pos = i
        value = ls[i]
        while pos > 0 and value < ls[pos-1]:
            ls[pos] = ls[pos-1]
            pos -= 1
        ls[pos] = value
    return ls
```

*测试*

```python
import numpy as np
ls = list(np.random.randint(0, 100, 10))
print(ls)
#ls = bubble_sort(ls)
#ls = selection_sort(ls)
#ls = insertion_sort(ls)
ls = insertion_sort_2(ls)
print(ls)
```



### Merge Sort

将序列从中间分为两半，对两部分调用递归排序，再将两个排好序的子序列合并。是典型的分治法

**有序数列的合并**

每个序列取第一个数，对比取较小者排到新序列的队尾，不断重复此过程至排序结束

```python
def merge(ls_1, ls_2):
    new_ls = []
    i, j = (0, 0)
    # compare and append the smaller to the new list
    while i < len(ls_1) and j < len(ls_2):
        if ls_1[i] < ls_2[j]:
            new_ls.append(ls_1[i])
            i += 1
        else:
            new_ls.append(ls_2[j])
            j += 1
    # append the rest
    while i < len(ls_1):
        new_ls.append(ls_1[i])
        i += 1
    while j < len(ls_2):
        new_ls.append(ls_2[j])
        j += 1
    return new_ls
```

```python
def merge_2(ls_1, ls_2):
    # a realization by recursion
    if len(ls_1) == 0:
        return ls_2
    if len(ls_2) == 0:
        return ls_1
    if ls_1[0] < ls_2[0]:
        return [ls_1[0]] + merge_2(ls_1[1:], ls_2)
    else:
        return [ls_2[0]] + merge_2(ls_1, ls_2[1:])
```

**merge sort**

```python
def merge_sort(ls):
    if len(ls) <= 1:
        return ls
    else:
        # partition of the list
        half = int(len(ls)/2)
        ls_1 = ls[:half]
        ls_2 = ls[half:]
        # sort each sub-list
        sorted_ls_1 = merge_sort(ls_1)
        sorted_ls_2 = merge_sort(ls_2)
        # merge and return
        return merge(sorted_ls_1, sorted_ls_2)
```

### Quick Sort

选择某个元素的值来分割序列，将较该值小的元素与较该值大的元素分成两个子序列，分别对其进行递归排序。由于两个子序列有大小之分，在合并时只需直接链接即可。

```python
def quick_sort(ls):
    if len(ls) <= 1:
        return ls
    ls_1 = []
    ls_2 = []
    # partition based on the first element of the list
    pivot = ls[0]
    for i in range(1, len(ls)):
        if ls[i] < pivot:
            ls_1.append(ls[i])
        else:
            ls_2.append(ls[i])
    # return by joining the two sub-lists in sequence
    return quick_sort(ls_1) + [pivot] + quick_sort(ls_2)
```

```python
def quick_sort_2(ls):
    # quick sort can be achieved in place without addtional space
    pass
```

### Heap Sort

新建一个max-heap，将序列一个一个插入到heap中，再将heap的根元素一个一个取出来并按顺序排列，即得到排序后序列

![heap_sort_1](/assets/Data Structures and Algorithms/heap_sort_1.png)

![heap_sort_2](/assets/Data Structures and Algorithms/heap_sort_2.png)

![heap_sort_3](/assets/Data Structures and Algorithms/heap_sort_3.png)

#### Radix Sort

先将序列分成各个bin，再在各个bin内排序，最后将各个bin链接起来。

![radix_sort](/assets/Data Structures and Algorithms/radix_sort.png)



# Algorithms

- 蛮力法/枚举法/Brute force
- 贪心法/Greedy algorithm
  - 将问题分解为多步完成，每一步都选择当下局部最优解而不考虑全局最优解
  - *例题*：《算法艺术与信息学竞赛》P13-钓鱼
- 递归法
  - *例题*：《算法艺术与信息学竞赛》P19-折纸痕，P20-聪明的学生，P29-Yanghee的数表
- 分治法/Divide-and-conquer
  - 对于一个规模为n的问题，可以分为a个规模为n/b的子问题，合并这些子问题需要O(n^d)时间，那么用分治法解决该问题的时间可以表示为：
    $T(n)=aT([n/b])+O(n^d)$

![master_theorem](/assets/Data Structures and Algorithms/master_theorem.png)

  - *代表*：merge sort/quick sort，将序列分解成两个子序列，分别排序后再合并，用递归实现
  - *例题*：《Algorithms》P60-Medians
- 动态规划/Dynamic programming
  - 可以分解为子问题，不同子问题间存在重叠子子问题，导致递归法效率不高
  - 需要记录已解决的子子问题的结果，从而不需要重复求解
  - 思路可以分为自上而下与自下而上
  - *例题*：《Introduction to Algorithms》P360-Rod cutting，P370-Matrix-chain multiplication
- 线性规划/Linear programming
