# Assignment #5: "树"算：概念、表示、解析、遍历

Updated 2124 GMT+8 March 17, 2024

2024 spring, Complied by  赵昱安 物理学院



## 1. 题目

### 27638: 求二叉树的高度和叶子数目

http://cs101.openjudge.cn/practice/27638/

**思路：**这道题主要的思想还是用递归的方法定义树，来求出二叉树的高度和叶子数目。关键点在于如何找到根节点以及如何确定叶子节点。学到了题解里面用列表来标记节点是否有父节点，从而找出根节点的方法。

**代码**

```python
# 赵昱安 2200011450
class TreeNode:
    def __init__(self):
        self.left=None
        self.right=None

def tree_height(node):
    if node is None:
        return -1
    return max(tree_height(node.left),tree_height(node.right))+1

def count_leaves(node):
    if node is None:
        return 0
    if node.left is None and node.right is None:
        return 1
    return count_leaves(node.left)+count_leaves(node.right)

n=int(input())
nodes=[TreeNode() for _ in range(n)]
has_parent=[False]*n

for i in range(n):
    left_index,right_index=map(int,input().split())
    if left_index != -1:
        nodes[i].left=nodes[left_index]
        has_parent[left_index]=True
    if right_index != -1:
        nodes[i].right=nodes[right_index]
        has_parent[right_index]=True

root_index=has_parent.index(False)
root=nodes[root_index]

height=tree_height(root)
number=count_leaves(root)
print(f"{height} {number}")
```

![二叉树的高度和叶子数目](C:\Users\赵昱安\Desktop\二叉树的高度和叶子数目.png)



### 24729: 括号嵌套树

http://cs101.openjudge.cn/practice/24729/

**思路：**这道题展示了可以用括号嵌套法来表示一棵树，并且考察了树的前、中、后序遍历。

**代码**

```python
# 赵昱安 2200011450
class TreeNode:
    def __init__(self,value):
        self.value=value
        self.children=[]

def parse_tree(s):
    stack=[]
    node=None
    for char in s:
        if char.isalpha():
            node=TreeNode(char)
            if stack:
                stack[-1].children.append(node)
        elif char=='(':
            if node:
                stack.append(node)
                node=None
        elif char==')':
            if stack:
                node=stack.pop()
    return node

def preorder(node):
    output=[node.value]
    for child in node.children:
        output.extend(preorder(child))
    return ''.join(output)

def postorder(node):
    output=[]
    for child in node.children:
        output.extend(postorder(child))
    output.append(node.value)
    return ''.join(output)
   
s=input().strip()
s=''.join(s.split())
root=parse_tree(s)
if root:
    print(preorder(root))
    print(postorder(root))
```

![括号树](C:\Users\赵昱安\Desktop\括号树.png)



### 02775: 文件结构“图”

http://cs101.openjudge.cn/practice/02775/

**思路：**这道题表面上是整理“目录”和“文件”，实际上本质还是运用树的结构。感觉这道题的题干好像是定义了一种新的树的遍历方式（类似于前中后序遍历），然后再按照它要求的格式输出。

**代码**

```python
# 赵昱安 2200011450
class Node:
    def __init__(self,name):
        self.name=name
        self.dirs=[]
        self.files=[]

def print_structure(node,indent=0):
    prefix='|     ' * indent
    print(prefix+node.name)
    for dir in node.dirs:
        print_structure(dir,indent+1)
    for file in sorted(node.files):
        print(prefix+file)

dataset=1
datas=[]
temp=[]

while True:
    line=input()
    if line=='#':
        break
    if line=='*':
        datas.append(temp)
        temp=[]
    else:
        temp.append(line)

for data in datas:
    print(f'DATA SET {dataset}:')
    root=Node('ROOT')
    stack=[root]
    for line in data:
        if line[0]=='d':
            dir=Node(line)
            stack[-1].dirs.append(dir)
            stack.append(dir)
        elif line[0]=='f':
            stack[-1].files.append(line)
        elif line==']':
            stack.pop()
    print_structure(root)
    if dataset<len(datas):
        print()
    dataset+=1
```

![文件结构](C:\Users\赵昱安\Desktop\文件结构.jpg)



### 25140: 根据后序表达式建立队列表达式

http://cs101.openjudge.cn/practice/25140/

**思路：**建立起表达式树，按层次遍历表达式树的结果前后颠倒就得到队列表达式。

**代码**

```python
# 赵昱安 2200011450
class TreeNode:
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None

def build_tree(postfix):
    stack=[]
    for char in postfix:
        node=TreeNode(char)
        if char.isupper():
            node.right=stack.pop()
            node.left=stack.pop()
        stack.append(node)
    return stack[0]

def level_order_traversal(root):
    queue=[root]
    traversal=[]
    while queue:
        node=queue.pop(0)
        traversal.append(node.value)
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
    return traversal

n=int(input().strip())
for _ in range(n):
    postfix=input().strip()
    root=build_tree(postfix)
    queue_expression=level_order_traversal(root)[::-1]
    print(''.join(queue_expression))
```

![树转队列](C:\Users\赵昱安\Desktop\树转队列.png)



### 24750: 根据二叉树中后序序列建树

http://cs101.openjudge.cn/practice/24750/

**思路：**先根据输入的后序遍历序列和中序遍历序列建树，再输出树的前序遍历序列。

**代码**

```python
# 赵昱安 2200011450
class TreeNode:
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None

def build_tree(inorder,postorder):
    if not inorder or not postorder:
        return None
    root_val=postorder.pop()
    root=TreeNode(root_val)
    root_index=inorder.index(root_val)
    root.right=build_tree(inorder[root_index+1:],postorder)
    root.left=build_tree(inorder[:root_index],postorder)
    return root

def preorder_traversal(root):
    result=[]
    if root:
        result.append(root.value)
        result.extend(preorder_traversal(root.left))
        result.extend(preorder_traversal(root.right))
    return result

inorder=input().strip()
postorder=input().strip()
root=build_tree(list(inorder),list(postorder))
print(''.join(preorder_traversal(root)))
```

![中后建树](C:\Users\赵昱安\Desktop\中后建树.png)



### 22158: 根据二叉树前中序序列建树

http://cs101.openjudge.cn/practice/22158/

**思路：**先根据输入的前序遍历序列和中序遍历序列建树，再输出树的后序遍历序列。

**代码**

```python
# 赵昱安 2200011450
class TreeNode:
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None

def b_t(preo,ino):
    if not preo or not ino:
        return None
    root_value=preo[0]
    root=TreeNode(root_value)
    root_index_ino=ino.index(root_value)
    root.left=b_t(preo[1:1+root_index_ino],ino[:root_index_ino])
    root.right=b_t(preo[1+root_index_ino:],ino[1+root_index_ino:])
    return root

def posto_tra(root):
    if root is None:
        return ''
    return posto_tra(root.left)+posto_tra(root.right)+root.value

while True:
    try:
        preo=input().strip()
        ino=input().strip()
        root=b_t(preo,ino)
        print(posto_tra(root))
    except EOFError:
        break
```

![前中建树](C:\Users\赵昱安\Desktop\前中建树.png)



## 2. 学习总结和收获

##### 1.树的属性

（1）层次性，即树是按层次构建的；

（2）一个节点的所有子节点都与另一个节点的所有子节点无关；

（3）叶子节点都是独一无二的；

（4）可以将树的某个部分（子树）整体移到另一个位置，而不影响下面的层。

##### 2.有关树的术语定义

**（1）节点（Node）：**节点是树的基础部分。它可以有自己的名字，我们称作“键”。节点也可以带有附加信息，我们称作“有效载荷”。

**（2）边（Edge）：**边是树的另一个基础部分。两个节点通过一条边相连，表示它们之间存在关系。除了根节点以外，其他每个节点都仅有一条入边，出边则可能有多条。

**（3）根节点（Root）：**根节点是树中唯一没有入边的节点。

**（4）路径（Path）：**路径是由边连接的有序节点列表。

**（5）子节点（Children）：**一个节点通过出边与子节点相连。

**（6）父节点（Parent）：**一个节点是其所有子节点的父节点。

**（7）兄弟节点（Sibling）：**具有同一父节点的节点互称为兄弟节点。

**（8）子树（Subtree）：**一个父节点及其所有后代的节点和边构成一棵子树。

**（9）叶子节点（Leaf Node）：**叶子节点没有子节点。

**（10）层数（Level）：**节点n的层数是从根节点到n的唯一路径长度。

**（11）高度（Height）：**树的高度是其中节点层数的最大值。

##### 3.树的定义

**（1）定义一：**树由节点及连接节点的边构成。树有以下属性：

①有一个根节点；

②除根节点外，其他每个节点都与其唯一的父节点相连；

③从根节点到其他每个节点都有且仅有一条路径；

④如果每个节点最多有两个子节点，我们就称这样的树为**二叉树**。

**（2）定义二（树的递归定义）：**一棵树要么为空，要么由一个根节点和零棵或多棵子树构成，子树本身也是一棵树。每棵子树的根节点通过一条边连到父树的根节点。

##### 4.树的遍历

①前序遍历：先访问根节点，然后递归地前序遍历左子树，最后递归地前序遍历右子树。

②中序遍历：先递归地中序遍历左子树，然后访问根节点，最后递归地中序遍历右子树。

③后序遍历：先递归地后序遍历右子树，然后递归地后序遍历左子树，最后访问根节点。



​       感觉对树的掌握程度加深了一些，但还是没有办法独立写出完整的实现树的功能的代码，最后必须都得参照题解，希望能尽快达到树的题目不看题解就能AC的层次。



