# Assignment #A: 图论：遍历，树算及栈

Updated 2018 GMT+8 Apr 21, 2024

2024 spring, Complied by ==赵昱安 物理学院==



## 1. 题目

### 20743: 整人的提词本

http://cs101.openjudge.cn/practice/20743/

**思路**：利用“栈”的数据结构按题目要求对字符串进行处理。

**代码**

```python
# 赵昱安 2200011450
s=input().strip()
stack=[]
for char in s:
    if char==')':
        s1=[]
        while stack and stack[-1]!='(':
            s1.append(stack.pop())
        if stack:
            stack.pop()
        stack.extend(s1)
    else:
        stack.append(char)
print(''.join(stack))
```

![提词本](C:\Users\赵昱安\Desktop\提词本.jpg)



### 02255: 重建二叉树

http://cs101.openjudge.cn/practice/02255/

**思路**：利用所给的前序和中序遍历结果建树，再输出后序遍历的结果。

**代码**

```python
# 赵昱安 2200011450
def build_postorder(preorder,inorder):
    if not preorder:
        return ''
    
    root=preorder[0]
    root_index=inorder.index(root)
    
    left_inorder=inorder[:root_index]
    right_inorder=inorder[root_index+1:]
    
    left_preorder=preorder[1:1+len(left_inorder)]
    right_preorder=preorder[1+len(left_inorder):]
    
    left_postorder=build_postorder(left_preorder,left_inorder)
    right_postorder=build_postorder(right_preorder,right_inorder)
    
    return left_postorder+right_postorder+root

while True:
    try:
        preorder,inorder=input().split()
        print(build_postorder(preorder,inorder))
    except EOFError:
        break
```

![重建二叉树](C:\Users\赵昱安\Desktop\重建二叉树.jpg)



### 01426: Find The Multiple

http://cs101.openjudge.cn/practice/01426/

要求用bfs实现

**思路**：利用双端队列这一数据结构更简单地解决问题。

**代码**

```python
# 赵昱安 2200011450
from collections import deque

while True:
    n=int(input())
    if n==0:
        break

    q=deque([(1,'1')])
    record={1}
    while q:
        remain,ans=q.popleft()
        if remain==0:
            print(ans)
            break
        for i in [0,1]:
            new_remain=(remain*10+i)%n
            if new_remain not in record:
                record.add(new_remain)
                q.append((new_remain,ans+str(i)))
```

![Find](C:\Users\赵昱安\Desktop\Find.jpg)



### 04115: 鸣人和佐助

bfs, http://cs101.openjudge.cn/practice/04115/

**思路**：用bfs进行遍历搜索。

**代码**

```python
# 赵昱安 2200011450
from collections import deque

class Node:
    def __init__(self,x,y,tools,steps):
        self.x=x
        self.y=y
        self.tools=tools
        self.steps=steps

M,N,T=map(int,input().split())
maze=[list(input()) for _ in range(M)]
visit=[[[0]*(T+1) for _ in range(N)] for _ in range(M)]
directions=[[-1,0],[1,0],[0,-1],[0,1]]
start=end=None
flag=0
for i in range(M):
    for j in range(N):
        if maze[i][j]=='@':
            start=Node(i,j,T,0)
            visit[i][j][T]=1
        if maze[i][j]=='+':
            end=(i,j)
            maze[i][j]='*'

queue=deque([start])
while queue:
    node=queue.popleft()
    if (node.x,node.y)==end:
        print(node.steps)
        flag=1
        break

    for direction in directions:
        nx,ny=node.x+direction[0],node.y+direction[1]
        if 0<=nx<M and 0<=ny<N:
            if maze[nx][ny]=='*':
                if not visit[nx][ny][node.tools]:
                    queue.append(Node(nx,ny,node.tools,node.steps+1))
                    visit[nx][ny][node.tools]=1
            elif maze[nx][ny]=='#':
                if node.tools>0 and not visit[nx][ny][node.tools-1]:
                    queue.append(Node(nx,ny,node.tools-1,node.steps+1))
                    visit[nx][ny][node.tools-1]=1
if not flag:
    print('-1')
```

![鸣人与佐助](C:\Users\赵昱安\Desktop\鸣人与佐助.jpg)



### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/

**思路**：也是bfs，只不过用到了heap的数据结构和dijkstra的算法。

**代码**

```python
# 赵昱安 2200011450
import heapq

m,n,p=map(int,input().split())
moun=[]
for _ in range(m):
    moun.append(list(input().split()))
directions=[(1,0),(-1,0),(0,1),(0,-1)]

def dijkstra(sx,sy,ex,ey):
    if moun[sx][sy]=='#':
        return 'NO'
    pos=[]
    re=[[float('inf')]*n for _ in range(m)]
    re[sx][sy]=0
    heapq.heappush(pos,(0,sx,sy))
    while pos:
        d,x,y=heapq.heappop(pos)
        if x==ex and y==ey:
            return d     
        h=int(moun[x][y])
        for dx,dy in directions:
            nx=x+dx
            ny=y+dy
            if 0<=nx<m and 0<=ny<n and moun[nx][ny]!='#':
                if re[nx][ny]>d+abs(int(moun[nx][ny])-h):
                    re[nx][ny]=d+abs(int(moun[nx][ny])-h)
                    heapq.heappush(pos,(re[nx][ny],nx,ny))
    return 'NO'

for _ in range(p):
    sx,sy,ex,ey=map(int,input().split())
    print(dijkstra(sx,sy,ex,ey))
```

![走山路](C:\Users\赵昱安\Desktop\走山路.jpg)



### 05442: 兔子与星空

Prim, http://cs101.openjudge.cn/practice/05442/

**思路**：

**代码**

```python
# 赵昱安 2200011450
import heapq

def prim(graph,start):
    mst=[]
    used=set([start])
    edges=[(cost,start,to) for to,cost in graph[start].items()]
    heapq.heapify(edges)
    while edges:
        cost,frm,to=heapq.heappop(edges)
        if to not in used:
            used.add(to)
            mst.append((frm,to,cost))
            for to_next,cost2 in graph[to].items():
                if to_next not in used:
                    heapq.heappush(edges,(cost2,to,to_next))
    return mst

n=int(input())
graph={chr(i+65):{} for i in range(n)}
for _ in range(n-1):
    data=input().split()
    star=data[0]
    m=int(data[1])
    for j in range(m):
        to_star=data[2+j*2]
        cost=int(data[3+j*2])
        graph[star][to_star]=graph[to_star][star]=cost
mst=prim(graph,'A')
print(sum(x[2] for x in mst))
```

![兔子与星空](C:\Users\赵昱安\Desktop\兔子与星空.jpg)



## 2. 学习总结和收获

通过本次作业，复习了栈等基本数据结构的操作，对bfs和dijkstra算法有了更进一步理解，并且新掌握了heapq和deque的很多有用的函数操作。





