# Assignment #B: 图论和树算

Updated 1709 GMT+8 Apr 28, 2024

2024 spring, Complied by   赵昱安 物理学院



## 1. 题目

### 28170: 算鹰

dfs, http://cs101.openjudge.cn/practice/28170/

**思路**：这道题感觉是一个很标准的使用dfs算法的模型。当找到一个未被访问过的'.'时，调用dfs函数把与它上下左右相连的所有'.'都打上已经访问过的标记，这样就能保证每一个”鹰“都只被计数一次了。

**代码**

```python
# 赵昱安 2200011450
directions=[(1,0),(0,1),(0,-1),(-1,0)]
unvisited=[[True]*10 for _ in range(10)]

def dfs(x,y):
    unvisited[x][y]=False
    for dx,dy in directions:
        newx,newy=x+dx,y+dy
        if 0<=newx<10 and 0<=newy<10 and unvisited[newx][newy]:
            if board[newx][newy]=='.':
                dfs(newx,newy)

board=[]
count=0
for _ in range(10):
    board.append(input())
for i in range(10):
    for j in range(10):
        if unvisited[i][j] and board[i][j]=='.':
            count+=1
            dfs(i,j)
print(count)
```

![算鹰](C:\Users\赵昱安\Desktop\算鹰.jpg)



### 02754: 八皇后

dfs, http://cs101.openjudge.cn/practice/02754/

**思路**：用dfs算法，判断两个皇后不在同一斜线的方法是保证两个皇后所在坐标的行差不等于列差。

**代码**

```python
# 赵昱安 2200011450
answer=[]
def Queen(l,s):
    for i in '12345678':
        for j in range(l):
            if i==s[j] or abs(int(i)-int(s[j]))==abs(l-j):
                break
        else:
            if l==7:
                answer.append(s+i)
            else:
                Queen(l+1,s+i)
Queen(0,'')
n=int(input())
for _ in range(n):
    a=int(input())
    print(answer[a-1])
```

![八皇后](C:\Users\赵昱安\Desktop\八皇后.jpg)



### 03151: Pots

bfs, http://cs101.openjudge.cn/practice/03151/

**思路**：bfs在于每走一步都要把所有下一步的可能遍历一遍，从而保证搜索的全面性。

**代码**

```python
# 赵昱安 2200011450
def bfs(A,B,C):
    start=(0,0)
    visited=set()
    visited.add(start)
    queue=[(start,[])]

    while queue:
        (a,b),actions=queue.pop(0)
        if a==C or b==C:
            return actions

        next_states=[(A,b),(a,B),(0,b),(a,0),(min(a+b,A),max(0,a+b-A)),(max(0,a+b-B),min(a+b,B))]

        for i in next_states:
            if i not in visited:
                visited.add(i)
                new_actions=actions+[get_action(a,b,i)]
                queue.append((i,new_actions))
    return ["impossible"]

def get_action(a,b,next_state):
    if next_state==(A,b):
        return "FILL(1)"
    elif next_state==(a,B):
        return "FILL(2)"
    elif next_state==(0,b):
        return "DROP(1)"
    elif next_state==(a,0):
        return "DROP(2)"
    elif next_state==(min(a+b,A),max(0,a+b-A)):
        return "POUR(2,1)"
    else:
        return "POUR(1,2)"

A,B,C=map(int,input().split())
solution=bfs(A,B,C)

if solution==["impossible"]:
    print(solution[0])
else:
    print(len(solution))
    for i in solution:
        print(i)
```

![pots](C:\Users\赵昱安\Desktop\pots.png)



### 05907: 二叉树的操作

http://cs101.openjudge.cn/practice/05907/

**思路**：这是一道二叉树的题目，要按照题目的要求实现交换节点和找出最左子节点的操作。总共分成三部分：1.定义TreeNode类并建树；2.定义实现交换节点功能的函数；3.定义实现找到最左子节点的函数。

**代码**

```python
# 赵昱安 2200011450
class TreeNode:
    def __init__(self,value=0):
        self.value=value
        self.left=None
        self.right=None

def build_tree(nodes_infor):
    nodes=[TreeNode(i) for i in range(n)]
    for value,left,right in nodes_infor:
        if left!=-1:
            nodes[value].left=nodes[left]
        if right!=-1:
            nodes[value].right=nodes[right]
    return nodes

def swap_nodes(nodes,x,y):
    for node in nodes:
        if node.left and node.left.value in [x,y]:           
            node.left=nodes[y] if node.left.value==x else nodes[x]
        if node.right and node.right.value in [x,y]:           
            node.right=nodes[y] if node.right.value==x else nodes[x]
            
def find_mostleft(node:TreeNode):
    while node and node.left:
        node=node.left
    return node.value if node else -1

t=int(input())
for _ in range(t):
    n,m=map(int,input().split())
    nodes_infor=[tuple(map(int,input().split())) for _ in range(n)]
    nodes=build_tree(nodes_infor)
    ops=[tuple(map(int,input().split()))for _ in range(m)]
    for op in ops:
        if op[0]==1:
            swap_nodes(nodes,op[1],op[2])
        elif op[0]==2:
            print(find_mostleft(nodes[op[1]]))
```

![二叉树的操作](C:\Users\赵昱安\Desktop\二叉树的操作.png)



### 18250: 冰阔落 I

Disjoint set, http://cs101.openjudge.cn/practice/18250/

**思路**：这是一道考察并查集的题目。只要按照标准模版定义find()函数和union()函数即可，不过要注意，由于题目中的描述是把y杯子中的阔落倒进x杯子中，故union()函数中x和y的位置要调换一下。

**代码**

```python
# 赵昱安 2200011450
def find(x):
    if parent[x]!=x:
        parent[x]=find(parent[x])
    return parent[x]

def union(x,y):
    parent[find(y)]=find(x)

while True:
    try:
        n,m=map(int,input().split())
        parent=[i for i in range(n+1)]
        for _ in range(m):
            x,y=map(int,input().split())
            if find(x)==find(y):
                print('Yes')
            else:
                print('No')
                union(x,y)

        glasses=set(find(x) for x in range(1,n+1))
        answer=sorted(glasses)
        print(len(answer))
        for j in range(len(answer)):
            answer[j]=str(answer[j])
        print(' '.join(answer))
    except EOFError:
        break
```

![冰阔落](C:\Users\赵昱安\Desktop\冰阔落.png)



### 05443: 兔子与樱花

http://cs101.openjudge.cn/practice/05443/

**思路**：使用dijkstra算法。

**代码**

```python
# 赵昱安 2200011450
import heapq
def dijkstra(graph,start):
    distances={vertex:float('infinity') for vertex in graph}
    pre={vertex:None for vertex in graph}
    distances[start]=0
    pq=[(0,start)]
    while pq:
        current_dis,current_vertex=heapq.heappop(pq)
        if current_dis>distances[current_vertex]:
            continue
        for neighbor,weight in graph[current_vertex].items():
            distance=current_dis+weight
            if distance<distances[neighbor]:
                distances[neighbor]=distance
                pre[neighbor]=current_vertex
                heapq.heappush(pq,(distance,neighbor))
    return distances,pre

def shortest_path(graph,start,end):
    distances,pre=dijkstra(graph,start)
    path,current=[],end
    while pre[current] is not None:
        path.insert(0,current)
        current=pre[current]
    path.insert(0,start)
    return path,distances[end]

P=int(input())
places=[]
for _ in range(P):
    places.append(input())

Q=int(input())
graph={place:{} for place in places}
for _ in range(Q):
    place1,place2,distance=input().split()
    distance=int(distance)
    graph[place1][place2]=graph[place2][place1]=distance

R=int(input())
for _ in range(R):
    start,end=input().split()
    if start==end:
        print(start)
        continue
    output=''
    path,total_distance=shortest_path(graph,start,end)
    for i in range(len(path)-1):
        output+=f"{path[i]}->({graph[path[i]][path[i+1]]})->"
    output+=f"{end}"
    print(output)
```

![image-20240502111301550](C:\Users\赵昱安\AppData\Roaming\Typora\typora-user-images\image-20240502111301550.png)



## 2. 学习总结和收获

通过这次作业，感觉自己对于dfs算法和disjoint set掌握的比较好了。对于bfs算法和dijkstra算法，虽然能够明白其基本思想，但是很难在不参考讲义和题解的情况下自己写出完整的代码，还需要进行进一步的练习。
