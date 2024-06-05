# Assignment #F: All-Killed 满分

Updated 1844 GMT+8 May 20, 2024

2024 spring, Complied by ==赵昱安 物理学院==



## 1. 题目

### 22485: 升空的焰火，从侧面看

http://cs101.openjudge.cn/practice/22485/

**思路**：用双端队列来实现二叉树的结构，从而实现输出从右边看到的序列的功能。

**代码**

```python
#赵昱安 2200011450 
from collections import deque

def right_view(n,tree):
    queue=deque([(1,tree[1])])
    result=[]
    
    while queue:
        for i in range(len(queue)):
            node,children=queue.popleft()
            if children[0]!=-1:
                queue.append((children[0],tree[children[0]]))
            if children[1]!=-1:
                queue.append((children[1],tree[children[1]]))
        result.append(node)
    return result

n=int(input())
tree={1:[-1,-1] for _ in range(n+1)}
for i in range(1,n+1):
    left,right=map(int,input().split())
    tree[i]=[left,right]

result=right_view(n,tree)
print(' '.join(map(str,result)))
```

![烟花](C:\Users\赵昱安\Desktop\烟花.jpg)



### 28203:【模板】单调栈

http://cs101.openjudge.cn/practice/28203/

**思路**：单调栈的模版。才知道用'*'来调整输出格式可以减少内存。

**代码**

```python
# 赵昱安 2200011450
n=int(input())
a=list(map(int,input().split()))
s=[]
answer=[0]*n

for i in range(n):
    while s and a[s[-1]]<a[i]:
        answer[s.pop()]=i+1
    s.append(i)

print(*answer)
```

![单调栈](C:\Users\赵昱安\Desktop\单调栈.jpg)



### 09202: 舰队、海域出击！

http://cs101.openjudge.cn/practice/09202/

**思路**：本质上就是判断一个无向图中有没有环。

**代码**

```python
# 赵昱安 2200011450
from  collections import defaultdict

def has_circle(node,color):
    color[node]=1
    for neighbour in graph[node]:
        if color[neighbour]==1:
            return True
        if color[neighbour]==0 and has_circle(neighbour,color):
            return True
    color[node]=2
    return False

T=int(input())
for _ in range(T):
    N,M=map(int,input().split())
    graph=defaultdict(list)
    for _ in range(M):
        x,y=map(int,input().split())
        graph[x].append(y)
    color=[0]*(N+1)
    for node in range(1,N+1):
        if color[node]==0:
            if has_circle(node,color):
                print('Yes')
                break
    else:
        print('No')
```

![舰队](C:\Users\赵昱安\Desktop\舰队.jpg)



### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135/

**思路**：双指针。不断调整左右指针来找到最优解。

**代码**

```python
# 赵昱安 2200011450
def check(x):
    num,cut=1,0
    for i in range(n):
        if cut+L[i]>x:
            num+=1
            cut=L[i]
        else:
            cut+=L[i]
    if num>m:
        return False
    else:
        return True

n,m=map(int,input().split())
L=[]
for _ in range(n):
    L.append(int(input()))
maxmax=sum(L)
minmax=max(L)
while minmax<maxmax:
    middle=(minmax+maxmax)//2
    if check(middle):
        maxmax=middle
    else:
        minmax=middle+1
print(maxmax)
```

![月度开销](C:\Users\赵昱安\Desktop\月度开销.jpg)



### 07735: 道路

http://cs101.openjudge.cn/practice/07735/

**思路**：用Dijkstra算法，参照了群里大佬的代码。

**代码**

```python
# 赵昱安 2200011450
import heapq
from collections import defaultdict

K=int(input())
N=int(input())
R=int(input())
graph=defaultdict(list)
for i in range(R):
    S,D,L,T=map(int,input().split())
    graph[S].append((D,L,T))

def Dijkstra(graph):
    global K,N,R
    q,answer=[],[]
    heapq.heappush(q,(0,0,1,0))
    while q:
        l,cost,cur,step=heapq.heappop(q)
        if cur==N:
            return l 
        for next,nl,nc in graph[cur]:
            if cost+nc<=K and graph[cur]:
                heapq.heappush(q,(l+nl,cost+nc,next,step+1))
    return -1
print(Dijkstra(graph))
```

![道路](C:\Users\赵昱安\Desktop\道路.png)



### 01182: 食物链

http://cs101.openjudge.cn/practice/01182/

**思路**：用并查集来解决，主要是要搞清楚每个物种之间的捕食关系。

**代码**

```python
# 赵昱安 2200011450
def find(x):
    if p[x]==x:
        return x
    else:
        p[x]=find(p[x])
        return p[x]

N,K=map(int,input().split())
p=[i for i in range(3*N+1)]

answer=0
for _ in range(K):
    D,X,Y=map(int,input().split())
    if X>N or Y>N:
        answer+=1
    elif D==1:
        if find(X+N)==find(Y) or find(Y+N)==find(X):
            answer+=1
        else:
            p[find(X)]=find(Y)
            p[find(X+N)]=find(Y+N)
            p[find(X+2*N)]=find(Y+2*N)
    else:
        if find(X)==find(Y) or find(Y+N)==find(X):
            answer+=1
        else:
            p[find(X+N)]=find(Y)
            p[find(Y+2*N)]=find(X)
            p[find(X+2*N)]=find(Y+N)
print(answer)
```

![食物链](C:\Users\赵昱安\Desktop\食物链.png)



## 2. 学习总结和收获

已经全面开始上机考试的复习了，这周做了一些晴问上的题目（主要是树部分），感觉晴问上的题目按知识点和难度分类做得很好，复习一个知识点就可以在上面找对应的题目练习，对我建立起知识体系比较有帮助。





