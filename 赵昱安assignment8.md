# Assignment #8: 图论：概念、遍历，及 树算

Updated 1919 GMT+8 Apr 8, 2024

2024 spring, Complied by  赵昱安 物理学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 19943: 图的拉普拉斯矩阵

matrices, http://cs101.openjudge.cn/practice/19943/

请定义Vertex类，Graph类，然后实现

**思路：**这道题直接用矩阵写很简单，但是要求定义类来实现，代码行数很多，正好借此题学习了Vertex类和Graph类的写法。（AC截图我放用矩阵的简洁版了，类的方法那个代码太长截图截不清楚）

**代码**

```python
# 赵昱安 2200011450
class Vertex:
    def __init__(self,key):
        self.id=key
        self.connectedTo={}
    def addNeighbor(self,nbr,weight=0):
        self.connectedTo[nbr]=weight
    def __str__(self):
        return str(self.id)+'connectedTo:'+str([x.id for x in self.connectedTo])
    def getConnections(self):
        return self.connectedTo.keys()
    def getId(self):
        return self.id
    def getWeight(self,nbr):
        return self.connectedTo[nbr]
    
class Graph:
    def __init__(self):
        self.vertList={}
        self.numVertices=0
    def addVertex(self,key):
        self.numVertices+=1
        newVertex=Vertex(key)
        self.vertList[key]=newVertex
        return newVertex
    def getVertex(self,n):
        if n in self.vertList:
            return self.vertList[n]
        else:
            return None
    def __contains__(self,n):
        return n in self.vertList
    def addEdge(self,f,t,weight=0):
        if f not in self.vertList:
            nv=self.addVertex(f)
        if t not in self.vertList:
            nv=self.addVertex(t)
        (self.vertList[f]).addNeighbor(self.vertList[t],weight)
    def getVertices(self):
        return self.vertList.keys()
    def __iter__(self):
        return iter(self.vertList.values())
    
def construct_Laplacian_Matrix(n,edges):
    graph=Graph()
    for i in range(n):
        graph.addVertex(i)
    for edge in edges:
        a,b=edge
        graph.addEdge(a,b)
        graph.addEdge(b,a)
    laplacianMatrix=[]
    for vertex in graph:
        row=[0]*n
        row[vertex.getId()]=len(vertex.getConnections())
        for neighbor in vertex.getConnections():
            row[neighbor.getId()]=-1
        laplacianMatrix.append(row)
    return laplacianMatrix

n,m=map(int,input().split())
edges=[]
for i in range(m):
    a,b=map(int,input().split())
    edges.append((a,b))

laplacianMatrix=construct_Laplacian_Matrix(n,edges)
for row in laplacianMatrix:
    print(' '.join(map(str,row)))
```

![图的拉普拉斯矩阵](C:\Users\赵昱安\Desktop\图的拉普拉斯矩阵.png)



### 18160: 最大连通域面积

matrix/dfs similar, http://cs101.openjudge.cn/practice/18160

**思路：**利用函数的递归来实现深度优先搜索，对每一个点找其连通域，并从中找最大者。

**代码**

```python
# 赵昱安 2200011450
dire=[[-1,-1],[-1,0],[-1,1],[0,-1],[0,1],[1,-1],[1,0],[1,1]]
area=0
def dfs(x,y):
    global area
    if matrix[x][y]=='.':
        return
    matrix[x][y]='.'
    area+=1
    for i in range(len(dire)):
        dfs(x+dire[i][0],y+dire[i][1])

t=int(input())
for _ in range(t):
    n,m=map(int,input().split())
    matrix=[['.' for _ in range(m+2)]]
    for i in range(n):
        matrix.append(['.']+list(input())+['.'])
    matrix.append(['.' for _ in range(m+2)])

    answer=0
    for i in range(1,n+1):
        for j in range(1,m+1):
            if matrix[i][j]=='W':
                area=0
                dfs(i,j)
                answer=max(answer,area)
    print(answer)
```

![最大连通域面积](C:\Users\赵昱安\Desktop\最大连通域面积.png)



### sy383: 最大权值连通块

https://sunnywhy.com/sfbj/10/3/383

**思路：**这是一道图的相关题目，用并查集来做代码比较简洁。

**代码**

```python
# 赵昱安 2200011450
def find(x):
    if p[x]==x:
        return x
    else:
        y=find(p[x])
        p[x]=y
        return y

def union(x,y):
    u=find(x)
    v=find(y)
    if u!=v:
        p[u]=v
        s[v]+=s[u]


n,m=map(int,input().split())
keys=list(map(int,input().split()))
s=[keys[i] for i in range(n)]
p=[i for i in range(n)]
for _ in range(m):
    x,y=map(int,input().split())
    union(x,y)
print(max(s))
```

![最大权值联通块](C:\Users\赵昱安\Desktop\最大权值联通块.png)



### 03441: 4 Values whose Sum is 0

data structure/binary search, http://cs101.openjudge.cn/practice/03441

**思路：**同时考虑四个数的和比较复杂，我们把a+b+c+d拆分成两部分a+b和c+d。用一个字典记录（a+b）的所有和以及其出现的次数，再看每一个（c+d）的和的相反数是否在字典里。

**代码**

```python
# 赵昱安 2200011450
n=int(input())
a=[0]*n
b=[0]*n
c=[0]*n
d=[0]*n

for _ in range(n):
    a[_],b[_],c[_],d[_]=map(int,input().split())

dict={}
for i in range(n):
    for j in range(n):
        if a[i]+b[j] not in dict:
            dict[a[i]+b[j]]=1
        else:
            dict[a[i]+b[j]]+=1
count=0
for i in range(n):
    for j in range(n):
        if -(c[i]+d[j]) in dict:
            count+=dict[-(c[i]+d[j])]
print(count)
```

![sum=0](C:\Users\赵昱安\Desktop\sum=0.png)



### 04089: 电话号码

trie, http://cs101.openjudge.cn/practice/04089/

Trie 数据结构可能需要自学下。

**思路：**通过这道题学习了一下“字典树”这个数据结构。

**代码**

```python
# 赵昱安 2200011450
class TrieNode:
    def __init__(self):
        self.child={}

class Trie:
    def __init__(self):
        self.root=TrieNode()
    def insert(self,nums):
        curnode=self.root
        for x in nums:
            if x not in curnode.child:
                curnode.child[x]=TrieNode()
            curnode=curnode.child[x]
    def search(self,num):
        curnode=self.root
        for x in num:
            if x not in curnode.child:
                return 0
            curnode=curnode.child[x]
        return 1
    
t=int(input())
p=[]
for _ in range(t):
    n=int(input())
    nums=[]
    for _ in range(n):
        nums.append(str(input()))
    nums.sort(reverse=True)
    s=0
    trie=Trie()
    for num in nums:
        s+=trie.search(num)
        trie.insert(num)
    if s>0:
        print('NO')
    else:
        print('YES')
```

![电话号码](C:\Users\赵昱安\Desktop\电话号码.png)



### 04082: 树的镜面映射

http://cs101.openjudge.cn/practice/04082/

**思路：**用虚子节点构造“伪满二叉树”。

**代码**

```python
#赵昱安 2200011450 
from collections import deque
class TreeNode:
    def __init__(self,x):
        self.x=x
        self.children=[]

def create_node():
    return TreeNode('')

def build_tree(tempList,index):
    node=create_node()
    node.x=tempList[index][0]
    if tempList[index][1]=='0' and node.x!='$':
        index+=1
        child,index=build_tree(tempList,index)
        node.children.append(child)
        index+=1
        child,index=build_tree(tempList,index)
        node.children.append(child)
    return node,index

def print_tree(p:TreeNode):
    Q=deque()
    s=deque()
    while p is not None:
        if p.x!='$':
            s.append(p)
        p=p.children[1] if len(p.children)>1 else None
    while s:
        Q.append(s.pop())
    while Q:
        p=Q.popleft()
        print(p.x,end=' ')
        if p.children:
            p=p.children[0]
            while p is not None:
                if p.x!='$':
                    s.append(p)
                p=p.children[1] if len(p.children)>1 else None
            while s:
                Q.append(s.pop())

n=int(input())
tempList=input().split(' ')
root,_=build_tree(tempList,0)
print_tree(root)
```

![树的镜面映射](C:\Users\赵昱安\Desktop\树的镜面映射.png)



## 2. 学习总结和收获

这次作业模版性的东西比较多，比如Vertex类和Graph类的定义、字典树（Trie）这一数据结构以及其他树和图的相关内容，还是需要花一些功夫进行理解和记忆。





