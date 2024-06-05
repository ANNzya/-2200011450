# Assignment #9: 图论：遍历，及 树算

Updated 1739 GMT+8 Apr 14, 2024

2024 spring, Complied by ==赵昱安 物理学院==



## 1. 题目

### 04081: 树的转换

http://cs101.openjudge.cn/dsapre/04081/

**思路：**这道题看似是树的题目，实际上并不用建树，只需要找出如何通过输入字符串中的'u'和'd'来判断树的高度即可。

**代码**

```python
# 赵昱安 2200011450
def tree_height(s):
    old_height=0
    max_old=0
    new_height=0
    max_new=0
    stack=[]

    for i in s:
        if i=='d':
            old_height+=1
            max_old=max(max_old,old_height)
            new_height+=1
            stack.append(new_height)
            max_new=max(max_new,new_height)
        else:
            old_height-=1
            new_height=stack[-1]
            stack.pop()
    return f"{max_old} => {max_new}"

s=input().strip()
print(tree_height(s))
```

![树的转换](C:\Users\赵昱安\Desktop\树的转换.png)



### 08581: 扩展二叉树

http://cs101.openjudge.cn/dsapre/08581/

**思路：**这也是一道树的题目，但同样也不需要严格定义TreeNode类然后按照模版建树，可以直接用元组(root,left,right)来代表一棵树，并用两个返回值实现函数的递归。

**代码**

```python
# 赵昱安 2200011450
def build_tree(preorder):
    if not preorder or preorder[0]=='.':
        return None,preorder[1:]
    root=preorder[0]
    left,preorder=build_tree(preorder[1:])
    right,preorder=build_tree(preorder)
    return (root,left,right),preorder

def inorder(tree):
    if tree is None:
        return ''
    root,left,right=tree
    return inorder(left)+root+inorder(right)

def postorder(tree):
    if tree is None:
        return ''
    root,left,right=tree
    return postorder(left)+postorder(right)+root

preorder=input().strip()
tree,_=build_tree(preorder)
print(inorder(tree))
print(postorder(tree))
```

![扩展](C:\Users\赵昱安\Desktop\扩展.png)



### 22067: 快速堆猪

http://cs101.openjudge.cn/practice/22067/

**思路：**主要是依靠栈这个数据结构来实现，但是关于栈中体重最轻的猪的比较需要注意，直接使用min()函数会超时，需要单独再建立一个列表来帮助我们快速找到体重最小的猪。

**代码**

```python
# 赵昱安 2200011450
pigs=[]
mini=[]

while True:
    try:
        s=input().split()
        if s[0]=='pop':
            if pigs:
                pigs.pop()
                if mini:
                    mini.pop()
        elif s[0]=='min':
            if pigs:
                print(mini[-1])
        else:
            n=int(s[1])
            pigs.append(n)
            if not mini:
                mini.append(n)
            else:
                mini.append(min(mini[-1],n))
    except EOFError:
        break
```

![快速堆猪](C:\Users\赵昱安\Desktop\快速堆猪.jpg)



### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123

**思路：**感觉是比较典型的dfs题目，每一步都进行递归并且打上标记，一旦成功了一次就把总可能性+1。

**代码**

```python
# 赵昱安 2200011450
steps=[[1,2],[1,-2],[-1,2],[-1,-2],[2,1],[2,-1],[-2,1],[-2,-1]]
answer=0

def Dfs(count,x,y):
    if count==n*m:
        global answer
        answer+=1
        return

    for step in steps:
        dx,dy=step[0],step[1]
        newx=x+dx
        newy=y+dy
        if 0<=newx<n and 0<=newy<m and field[newx][newy]:
            field[newx][newy]=0
            Dfs(count+1,newx,newy)
            field[newx][newy]=1

T=int(input())
for _ in range(T):
    n,m,x,y=map(int,input().split())
    field=[[1]*10 for __ in range(10)]
    answer=0
    field[x][y]=0
    Dfs(1,x,y)
    print(answer)
```

![马走日](C:\Users\赵昱安\Desktop\马走日.png)



### 28046: 词梯

bfs, http://cs101.openjudge.cn/practice/28046/

**思路：**这道题用到了dict、deque的数据结构，bfs的算法以及桶的思想，有点困难，学习了题解和大佬的代码。

**代码**

```python
# 赵昱安 2200011450
from collections import deque
bucket,dic={},{}
for j in range(int(input())):
    word=input()
    dic[word]=[]
    for i in range(4):
        val=word[:i]+"_"+word[i+1:]
        if val not in bucket:
            bucket[val]=[word]
        else:
            for value in bucket[val]:
                dic[value].append(word)
                dic[word].append(value)
            bucket[val].append(word)
a,b=[str(x) for x in input().split()]
stack,output,judge=deque([a]),[],{}
judge[a],k=-1,0
while stack:
    check=stack.popleft()
    if check!=b:
        for j in dic[check]:
            if j not in judge:
                judge[j]=check
                stack.append(j)
    else:
        while judge[check]!=-1:
            output.append(check)
            check=judge[check]
        k=1
        output.append(a)
        break
print(" ".join(map(str,output[::-1])) if k==1 else "NO")
```

![词梯](C:\Users\赵昱安\Desktop\词梯.png)



### 28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/

**思路：**在“马走日”的基础上再使用Warnsdorff规则提高搜索效率。

**代码**

```python
# 赵昱安 2200011450
def is_valid_move(board,n,x,y):
    return 0<=x<n and 0<=y<n and board[x][y]==-1

def find_next_move(board,n,x,y):
    dx=[2,1,-1,-2,-2,-1,1,2]
    dy=[1,2,2,1,-1,-2,-2,-1]
    valid_moves=[]
    
    for i in range(8):
        newx=x+dx[i]
        newy=y+dy[i]
        if is_valid_move(board,n,newx,newy):
            count = 0
            for j in range(8):
                neighbor_x=newx+dx[j]
                neighbor_y=newy+dy[j]
                if is_valid_move(board,n,neighbor_x,neighbor_y):
                    count+=1
            valid_moves.append((newx, newy, count))
    valid_moves.sort(key=lambda x: x[2])
    return valid_moves

def knight_tour(board,n,x,y,move_count):
    if move_count==n*n:
        return True 
    next_moves=find_next_move(board,n,x,y) 
    for move in next_moves:
        newx,newy, _=move
        board[newx][newy]=move_count
        if knight_tour(board,n,newx,newy,move_count+1):
            return True
        board[newx][newy]=-1  
    return False

n=int(input().strip())
sr,sc=map(int,input().strip().split())
board=[[-1 for _ in range(n)] for _ in range(n)]
board[sr][sc]=0
if knight_tour(board,n,sr,sc,1):
    print("success")
else:
    print("fail")
```

![骑士周游](C:\Users\赵昱安\Desktop\骑士周游.png)



## 2. 学习总结和收获

本次作业主要聚焦于树和图。

“树的转换”和“扩展二叉树”两道题目，让我明白解决树的问题并不一定需要上来就定义TreeNode类，树是一种思维模式，而并不是一个死板的形式，所以树的题目也需要灵活变通。

“马走日”和“骑士周游”运用了图搜索中的“深度优先搜索”，“词梯”运用了图搜索中的“广度优先探索”，这是非常重要的两个有关图的算法，需要再多练一些题目加深理解。
