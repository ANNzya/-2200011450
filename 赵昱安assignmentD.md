# Assignment #D: May月考

Updated 1654 GMT+8 May 8, 2024

2024 spring, Complied by ==赵昱安 物理学院==



## 1. 题目

### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/

**思路**：运用集合set这一数据结构，将需要砍下的树不重不漏地记录，最后输出剩下的树的数量。

**代码**

```python
# 赵昱安 2200011450
L,M=map(int,input().split())
A=set()
for _ in range(M):
    a,b=map(int,input().split())
    for i in range(a,b+1):
        A.add(i)
print(L-len(A)+1)
```

![校门外的树](C:\Users\赵昱安\Desktop\校门外的树.jpg)



### 20449: 是否被5整除

http://cs101.openjudge.cn/practice/20449/

**思路**：这道题主要是要掌握用 int(s,n)把n进制的数转化成十进制数字的方法。

**代码**

```python
# 赵昱安 2200011450
A=input()
ans=[]
for i in range(len(A)):
    number=int(A[:i+1],2)
    if number%5==0:
        ans.append('1')
    else:
        ans.append('0')
print(''.join(ans))
```

![是否被5整除](C:\Users\赵昱安\Desktop\是否被5整除.jpg)



### 01258: Agri-Net

http://cs101.openjudge.cn/practice/01258/

**思路**：这个问题可以通过最小生成树（MST）算法来解决，我选择使用Kruskal算法来解决。

Kruskal算法的步骤如下：

①将所有边按照权值从小到大排序。

②从小到大遍历每条边，如果加入这条边不形成环，则将其加入最小生成树。

③最终得到的最小生成树就是连接所有农场的最小光纤长度。

**代码**

```python
# 赵昱安 2200011450
class UnionFind:
    def __init__(self,n):
        self.parent=[i for i in range(n)]
    def find(self,x):
        if self.parent[x]!=x:
            self.parent[x]=self.find(self.parent[x])
        return self.parent[x]
    def union(self,x,y):
        root_x,root_y=self.find(x),self.find(y)
        if root_x!=root_y:
            self.parent[root_x]=root_y

def kruskal(edges,n):
    edges.sort(key=lambda x: x[2])
    uf=UnionFind(n)
    min_fiber=0
    for edge in edges:
        u,v,weight=edge
        if uf.find(u)!=uf.find(v):
            uf.union(u,v)
            min_fiber+=weight
    return min_fiber

while True:
    try:
        n=int(input())
        connectivity,edges=[],[]
        for _ in range(n):
            connectivity.append(list(map(int,input().split())))    
        for i in range(n):
            for j in range(i+1,n):
                edges.append((i,j,connectivity[i][j]))       
        result=kruskal(edges,n)
        print(result)
    except EOFError:
        break
```

![01258](C:\Users\赵昱安\Desktop\01258.jpg)



### 27635: 判断无向图是否连通有无回路(同23163)

http://cs101.openjudge.cn/practice/27635/

**思路**：给出一个无向图，要求实现两个功能：查找是否联通以及是否有回路。首先用列表分别记录每个节点与哪些节点之间直接联通。要判断是否连通，只需要从其中一个节点出发，看是否能走完无向图中的所有节点，如果能走完说明连通；要判断是否有回路，可以用bfs算法，检查图中是否存在一个节点，使得从它出发不通过原路返回而能够又回到它自己，如果存在这样的节点就说明有回路。

**代码**

```python
# 赵昱安 2200011450
def is_connected(graph,n):
    visited=[True]+[False]*(n-1)
    stack=[0]
    while stack:
        node=stack.pop()
        for neighbor in graph[node]:
            if not visited[neighbor]:
                stack.append(neighbor)
                visited[neighbor]=True
    return visited==[True]*n

def has_loop(graph,n):
    visited=[False]*n
    def dfs(node,parent):
        visited[node]=True
        for neighbor in graph[node]:
            if not visited[neighbor]:
                if dfs(neighbor,node):
                    return True
            elif neighbor!=parent:
                return True
        return False
    for i in range(n):
        if not visited[i]:
            if dfs(i,-1):
                return True
    return False

n,m=map(int,input().split())
graph=[[] for _ in range(n)]
for _ in range(m):
    u,v=map(int,input().split())
    graph[u].append(v)
    graph[v].append(u)
print('connected:yes') if is_connected(graph,n) else print('connected:no')
print('loop:yes') if has_loop(graph,n) else print('loop:no')
```

![无向图](C:\Users\赵昱安\Desktop\无向图.jpg)



### 27947: 动态中位数

http://cs101.openjudge.cn/practice/27947/

**思路**：这个问题需要通过堆（heap）来解决。首先创建两个堆，一个用来储存较小的一半数字，一个用来储存较大的一半数字；为了保证对半分，每当两个堆的大小差异超过1时，就要从多的堆里弹出一个元素放到少的堆里；每次维护好堆之后，此时的中位数就是元素较多的那一个堆的堆顶的元素。

**代码**

```python
# 赵昱安 2200011450
import heapq

def dynamic_median(numbers):
    max_heap,min_heap,medians=[],[],[]
    for i, num in enumerate(numbers):
        if not max_heap or num<=-max_heap[0]:
            heapq.heappush(max_heap,-num)
        else:
            heapq.heappush(min_heap,num)

        if len(max_heap)>len(min_heap)+1:
            heapq.heappush(min_heap,-heapq.heappop(max_heap))
        elif len(min_heap)>len(max_heap):
            heapq.heappush(max_heap,-heapq.heappop(min_heap))
        
        if (i + 1)%2==1:
            median=-max_heap[0]
            medians.append(median)
    return medians

T=int(input())
for _ in range(T):
    numbers=list(map(int,input().split()))
    medians=dynamic_median(numbers)
    print(len(medians))
    print(' '.join(map(str,medians)))
```

![动态中位数](C:\Users\赵昱安\Desktop\动态中位数.jpg)



### 28190: 奶牛排队

http://cs101.openjudge.cn/practice/28190/

**思路**：我们设左端点为 A ,右端点为 B。
因为A是区间内最矮的，所以[A.B]中，都比A高。所以只要A右侧第一个≤A的奶牛位于B的右侧；
同理，因为B是区间内最高的，所以[A.B]中，都比B矮。所以只要B左侧第一个≥B的奶牛位于A的左侧；
对于左/右侧第一个≥/≤不符合条件的我们可以使用单调栈维护。用单调栈预处理出left_bound数组表示左，right_bound数组表示右。然后枚举右端点B寻找A，更新ans即可。

**代码**

```python
# 赵昱安 2200011450
N=int(input())
heights=[int(input()) for _ in range(N)]
left_bound=[-1]*N
right_bound=[N]*N

stack=[]
for i in range(N):
    while stack and heights[stack[-1]]<heights[i]:
        stack.pop()
    if stack:
        left_bound[i]=stack[-1]
    stack.append(i)

stack=[]
for i in range(N-1,-1,-1):
    while stack and heights[stack[-1]]>heights[i]:
        stack.pop()
    if stack:
        right_bound[i]=stack[-1]
    stack.append(i)

ans=0
for i in range(N):
    for j in range(left_bound[i]+1,i):
        if right_bound[j]>i:
            ans = max(ans,i-j+1)
            break
print(ans)
```

![奶牛](C:\Users\赵昱安\Desktop\奶牛.jpg)



## 2. 学习总结和收获

第1、2题比较简单，第1题主要用集合，第2题主要考察字符串的转化；

通过第3题学习了最小生成树的Kruskal算法；

第4题是一道无向图的题，中间还用到了bfs算法，直接用模版就好。虽然代码比较长，但是思维量相对较小；

第5题需要用堆来解决。考察对heapq库中函数的运用；

通过第6题第一次接触双单调栈。

通过这次月考，我认为自己对于比较经典的简单的数据结构和算法掌握的还可以，但是对于最小生成树、单调栈等没有那么基本的知识就比较陌生。还是需要进一步扩展自己的知识面。
