# Assignment #7: April 月考

Updated 1557 GMT+8 Apr 3, 2024

2024 spring, Complied by  赵昱安 物理学院



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 27706: 逐词倒放

http://cs101.openjudge.cn/practice/27706/

**思路：**好久没见到这么友好的题了哈哈哈哈哈，一分钟读题＋AC。

**代码**

```python
# 赵昱安 2200011450
sentence=input().split()
answer=sentence[::-1]
print(' '.join(answer))
```

![逐词倒放](C:\Users\赵昱安\Desktop\逐词倒放.png)



### 27951: 机器翻译

http://cs101.openjudge.cn/practice/27951/

**思路：**这道题也比较基础，大概用了5分钟AC？只要创建一个队列作为内存有限的“字典”，然后每次遇到字典里没有的词就查找外存的字典，如果字典满了就弹出第一个进入的单词，最后得出一共查找的次数。

**代码**

```python
# 赵昱安 2200011450
M,N=map(int,input().split())
words=list(map(int,input().split()))
diction=[]
count=0
for word in words:
    if word not in diction:
        count+=1
        if len(diction)<M:
            diction.append(word)
        else:
            diction.pop(0)
            diction.append(word)
print(count)
```

![机器翻译](C:\Users\赵昱安\Desktop\机器翻译.png)



### 27932: Less or Equal

http://cs101.openjudge.cn/practice/27932/

**思路：**先把输入的数列进行从小到大的排序，然后分几种情况进行讨论：如果k与n相等，说明只要输出数列中最大的数字（即排序后列表中的最后一个数）即可；如果k=0，那么还要再分两种子情况讨论：如果数列的第一个数大于1，则只要输出最小数1，即可保证有0个数小于等于0；如果数列的第一个数等于1，则找不到满足条件的x，只能输出-1；最后一种情况，即0<k<n时，需要看索引为k的元素是否与索引为k-1的元素相等。

这道题难度不大，只需要略微思考一下其中的逻辑，并且注意不遗漏情况即可，大概用了十几分钟AC。

**代码**

```python
# 赵昱安 2200011450
n,k=map(int,input().split())
lst=list(map(int,input().split()))
lst.sort()

if k==n:
    print(lst[-1])
elif k==0:
    if lst[0]==1:
        print(-1)
    else:
        print(1)
else:
    a=lst[k-1]
    if lst[k]==a:
        print(-1)
    else:
        print(a)
```

![less or equal](C:\Users\赵昱安\Desktop\less or equal.png)



### 27948: FBI树

http://cs101.openjudge.cn/practice/27948/

**思路：**按照递归的模版定义节点类、定义建树函数、定义后序遍历函数，并且还要定义一个对字符串类型（F\B\I）的判断函数。

**代码**

```python
# 赵昱安 2200011450
class Node:
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None

def judge(s):
    if '0' in s and '1' not in s:
        return 'B'
    elif '0' not in s and '1' in s:
        return 'I'
    else:
        return 'F'
    
def build_tree(s):
    if len(s)==1:
        return Node(judge(s))
    mid=len(s)//2
    root=Node(judge(s))
    root.left=build_tree(s[:mid])
    root.right=build_tree(s[mid:])
    return root

def post_traversal(root:Node):
    if root is None:
        return[]
    lst=[]
    lst+=post_traversal(root.left)
    lst+=post_traversal(root.right)
    lst.append(root.value)
    return lst

N=int(input())
s=input()
root=build_tree(s)
result=post_traversal(root)
print(''.join(result))
```

![FBI树](C:\Users\赵昱安\Desktop\FBI树.png)



### 27925: 小组队列

http://cs101.openjudge.cn/practice/27925/

**思路：**学习大佬的写法。

**代码**

```python
# 赵昱安 2200011450
import queue
t=int(input())
out=-1
dic={}
now=set()
q=queue.Queue()
flag=[queue.Queue() for _ in range(t)]

for _ in range(t):
    group=list(input().split())
    for member in group:
        dic[member]=_
while True:
    command=input()
    if command=='STOP':
        break
    elif command=='DEQUEUE':
        if out!=-1 and not (flag[out].empty()):
            print((flag[out].get()))
        elif out==-1:
            out=q.get()
            print(flag[out].get())
        else:
            while (flag[out].empty()):
                now.remove(out)
                out=q.get()
            print(flag[out].get())
    else:
        a,b=command.split()
        g=dic[b]
        if g not in now:
            now.add(g)
            q.put(g)
            flag[g].put(b)
        else:
            flag[g].put(b)
```

![小组队列](C:\Users\赵昱安\Desktop\小组队列.png)



### 27928: 遍历树

http://cs101.openjudge.cn/practice/27928/

**思路：**从大佬那里学的不用建树直接递归的做法。

**代码**

```python
# 赵昱安 2200011450
def iterate(i):
    lst=sorted([i]+dict[i])
    for j in lst:
        if i==j:
            print(i)
        else:
            iterate(j)

dict={}
n=int(input())
for _ in range(n):
    lst=list(map(int,input().split()))
    dict[lst[0]]=lst[1:]
roots={i:True for i in dict}
for i in dict:
    for j in dict[i]:
        roots[j]=False
for k in roots:
    if roots[k]:
        root=k
        break
```

![遍历树](C:\Users\赵昱安\Desktop\遍历树.png)



## 2. 学习总结和收获

这次月考应该是AC5，因为我开始做的时间比较晚，导致第4题FBI树提交的时候比赛已经结束了两分钟。所以实际上只有最后一题没有AC。

前两道Easy题是真的很Easy，如果期末的时候也是相同的难度的话，应该争取十分钟内AC。

两道Medium题属于那种不太能一上来就有完整的思路，而且提交一次就能过的（对于我来说），但是多读几遍题，多尝试提交几次，修改一些逻辑和细节，最后应该还是可以AC的。主要是要尽量缩短AC的时间，这样还能有时间去多思考一下后面的两道Hard题目。

这次的两道Hard题目，一道队列一道树。树那道我是根本没来得及看（可能看了也过不了hh），小组队列最后倒是AC了，但是中间经历了无数次的失败，而且花费了很长很长的时间。果然对于这种代码长一些有难度的题目我就很难保证规定时间内AC，能力还是不够。

我对树掌握的还是不太熟练，以前作业都是照着示例写的，这次想尝试着自己写但是总有细节不对。其实树的题目挺模型化的，理论上不应该太难想出来，所以还是得把各类模版理解清楚并熟记。
