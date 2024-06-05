# Assignment #6: "树"算：Huffman,BinHeap,BST,AVL,DisjointSet

Updated 2214 GMT+8 March 24, 2024

2024 spring, Complied by 赵昱安 物理学院



**说明：**

1）这次作业内容不简单，耗时长的话直接参考题解。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 22275: 二叉搜索树的遍历

http://cs101.openjudge.cn/practice/22275/

**思路：**依旧是递归思想，只不过二叉搜索树比二叉树多了一条二叉搜索性：小于父节点的键都在左子树中，大于父节点的键都在右子树中。根据这一性质建树即可。

**代码**

```python
# 赵昱安 2200011450
def post_order(pre_order):
    if not pre_order:
        return []
    root=pre_order[0]
    left_tree=[x for x in pre_order if x<root]
    right_tree=[x for x in pre_order if x>root]
    return post_order(left_tree)+post_order(right_tree)+[root]

n=int(input())
pre_order=list(map(int,input().split()))
answer=map(str,post_order(pre_order))
print(' '.join(answer))
```

![二叉搜索树的遍历](C:\Users\赵昱安\Desktop\二叉搜索树的遍历.png)



### 05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/

**思路：**根据输入的数据建立二叉搜索树+按照层次输出搜索树每个节点的值。

**代码**

```python
# 赵昱安 2200011450
class TreeNode:
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None

def insert(node,value):
    if node is None:
        return TreeNode(value)
    if value<node.value:
        node.left=insert(node.left,value)
    elif value>node.value:
        node.right=insert(node.right,value)
    return node

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

numbers=list(map(int,input().strip().split()))
numbers=list(dict.fromkeys(numbers))
root=None
for number in numbers:
    root=insert(root,number)
traversal=level_order_traversal(root)
print(' '.join(map(str,traversal)))
```

![层次遍历](C:\Users\赵昱安\Desktop\层次遍历.png)



### 04078: 实现堆结构

http://cs101.openjudge.cn/practice/04078/

练习自己写个BinHeap。当然机考时候，如果遇到这样题目，直接import heapq。手搓栈、队列、堆、AVL等，考试前需要搓个遍。

**思路：**其实就是自己定义一个BinHeap类，按照题目的要求逐条实现即可（就是变量名真的很多，写的时候脑子要够清楚）。

**代码**

```python
# 赵昱安 2200011450
class BinHeap:
    def __init__(self):
        self.List=[0]
        self.size=0

    def percUp(self,i):
        while i//2>0:
            if self.List[i]<self.List[i//2]:
                tmp=self.List[i//2]
                self.List[i//2]=self.List[i]
                self.List[i]=tmp
            i=i//2

    def insert(self,k):
        self.List.append(k)
        self.size+=1
        self.percUp(self.size)

    def minChild(self,i):
        if i*2+1>self.size:
            return i*2
        else:
            if self.List[i*2]<self.List[i*2+1]:
                return i*2
            else:
                return i*2+1
            
    def percDown(self,i):
        while(i*2)<=self.size:
            mc=self.minChild(i)
            if self.List[i]>self.List[mc]:
                tmp=self.List[i]
                self.List[i]=self.List[mc]
                self.List[mc]=tmp
            i=mc

    def delMin(self):
        retval=self.List[1]
        self.List[1]=self.List[self.size]
        self.size-=1
        self.List.pop()
        self.percDown(1)
        return retval

    def buildheap(self,alist):
        i=len(alist)//2
        self.size=len(alist)
        self.List=[0]+alist[:]
        while i>0:
            self.percDown(i)
            i-=1
    
n=int(input().strip())
binheap=BinHeap()
for _ in range(n):
    inp=input().strip()
    if inp[0]=='1':
        binheap.insert(int(inp.split()[1]))
    else:
        print(binheap.delMin())
```

![实现堆结构](C:\Users\赵昱安\Desktop\实现堆结构.png)



### 22161: 哈夫曼编码树

http://cs101.openjudge.cn/practice/22161/

**思路：**关键点在于如何比较两个节点的大小，每次合并最小的两个节点，直到合并为一棵树为止。

**代码**

```python
# 赵昱安 2200011450
import heapq
class Node:
    def __init__(self,weight,char=None):
        self.weight=weight
        self.char=char
        self.left=None
        self.right=None

    def __lt__(self,other):
        if self.weight==other.weight:
            return self.char<other.char
        return self.weight<other.weight
    
def build_huffman_tree(characters):
    heap=[]
    for char,weight in characters.items():
        heapq.heappush(heap,Node(weight,char))
    while len(heap)>1:
        left=heapq.heappop(heap)
        right=heapq.heappop(heap)
        merged=Node(left.weight+right.weight)
        merged.left=left
        merged.right=right
        heapq.heappush(heap,merged)
    return heap[0]

def encode_huffman_tree(root):
    codes={}
    def traverse(node,code):
        if node.char:
            codes[node.char]=code
        else:
            traverse(node.left,code+'0')
            traverse(node.right,code+'1')
    traverse(root,'')
    return codes

def huffman_encoding(codes,string):
    encoded=''
    for char in string:
        encoded+=codes[char]
    return encoded

def huffman_decoding(root,encoded_string):
    decoded=''
    node=root
    for bit in encoded_string:
        if bit=='0':
            node=node.left
        else:
            node=node.right
        if node.char:
            decoded+=node.char
            node=root
    return decoded

n=int(input())
characters={}
for _ in range(n):
    char,weight=input().split()
    characters[char]=int(weight)
huffman_tree=build_huffman_tree(characters)
codes=encode_huffman_tree(huffman_tree)

strings=[]
while True:
    try:
        line=input()
        strings.append(line)
    except EOFError:
        break

results=[]
for string in strings:
    if string[0]  in ('0','1'):
        results.append(huffman_decoding(huffman_tree,string))
    else:
        results.append(huffman_encoding(codes,string))
for result in results:
    print(result)
```

![huffman1](C:\Users\赵昱安\Desktop\huffman1.png)

![huffman2](C:\Users\赵昱安\Desktop\huffman2.png)



### 晴问9.5: 平衡二叉树的建立

https://sunnywhy.com/sfbj/9/5/359

**思路：**在二叉搜索树的基础上，平衡二叉树的特殊之处在于每添加一个节点，它都要进行一次自我调整，使得每一个节点的左子树和右子树层数都尽可能地平均。按照这一特点建立AVL树，最后输出AVL树的前序遍历数列。

**代码**

```python
# 赵昱安 2200011450
class Node:
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None
        self.height=1

class AVL:
    def __init__(self):
        self.root=None
    def insert(self,value):
        if not self.root:
            self.root=Node(value)
        else:
            self.root=self._insert(value,self.root)
    def _insert(self,value,node):
        if not node:
            return Node(value)
        elif value<node.value:
            node.left=self._insert(value,node.left)
        else:
            node.right=self._insert(value,node.right)
        node.height=1+max(self._get_height(node.left),self._get_height(node.right))
        balance=self._get_balance(node)
        if balance>1:
            if value<node.left.value:
                return self._rotate_right(node)
            else:
                node.left=self._rotate_left(node.left)
                return self._rotate_right(node)
        if balance<-1:
            if value>node.right.value:
                return self._rotate_left(node)
            else:
                node.right=self._rotate_right(node.right)
                return self._rotate_left(node)
        return node
    def _get_height(self,node):
        if not node:
            return 0
        return node.height
    def _get_balance(self,node):
        if not node:
            return 0
        return self._get_height(node.left)-self._get_height(node.right)
    def _rotate_left(self,z):
        y=z.right
        T2=y.left
        y.left=z
        z.right=T2
        z.height=1+max(self._get_height(z.left),self._get_height(z.right))
        y.height=1+max(self._get_height(y.left),self._get_height(y.right))
        return y
    def _rotate_right(self,y):
        x=y.left
        T2=x.right
        x.right=y
        y.left=T2
        y.height=1+max(self._get_height(y.left),self._get_height(y.right))
        x.height=1+max(self._get_height(x.left),self._get_height(x.right))
        return x
    def preorder(self):
        return self._preorder(self.root)
    def _preorder(self,node):
        if not node:
            return []
        return [node.value]+self._preorder(node.left)+self._preorder(node.right)
    
n=int(input().strip())
values=list(map(int,input().strip().split()))
avl=AVL()
for value in values:
    avl.insert(value)
print(' '.join(map(str,avl.preorder())))
```

![平衡二叉树](C:\Users\赵昱安\Desktop\平衡二叉树.png)



### 02524: 宗教信仰

http://cs101.openjudge.cn/practice/02524/

**思路：**这道题一上来没什么头绪，仔细想一想其实也是靠递归的思想，建立一个数字列表记录每个人的宗教信仰。每输入一组数字，就要看其中有没有跟前面数字重合即信仰同一宗教，使得所有信仰同一宗教的人对应的列表中的数字都等于最小的那个，这样就很好地用列表记录了信仰相同的人，从而能够找出宗教种类的最大值。

**代码**

```python
# 赵昱安 2200011450
def check_religion(x,religion):
    if religion[x]!=x:
        religion[x]=check_religion(religion[x],religion)
    return religion[x]

def check_same(i,j,religion):
    I=check_religion(i,religion)
    J=check_religion(j,religion)
    if I==J:
        return
    religion[J]=I

case=1
while True:
    n,m=map(int,input().split())
    if n==m==0:
        break
    religion=[x for x in range(n)]
    answer=0
    for _ in range(m):
        i,j=map(int,input().split())
        i-=1
        j-=1
        check_same(i,j,religion)
    for k in range(n):
        if religion[k]==k:
            answer+=1    
    print(f"Case {case}: {answer}")
    case+=1
```

![宗教信仰](C:\Users\赵昱安\Desktop\宗教信仰.png)



## 2. 学习总结和收获

啊啊啊我发现自己对于长代码真的很难驾驭，再加上这周其他课程的任务量比较大没有太多时间仔细思考……导致本次作业大部分是靠题解完成的。等其他科目期中考完试后，要抽出一段时间好好整理一下树这部分的内容。

（ps:只有宗教信仰那道题算是在舒适区hhh）
