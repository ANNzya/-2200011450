# Assignment #4: 排序、栈、队列和树

Updated 0005 GMT+8 March 11, 2024

2024 spring, Complied by  赵昱安 物理学院



## 1. 题目

### 05902: 双端队列

http://cs101.openjudge.cn/practice/05902/

**思路：**这道题主要考察用列表实现一个特殊的双端队列，它只能从尾部添加元素，但可以从头部也可以从尾部移除元素。只要掌握了列表的一些基本操作就没有什么困难。

**代码**

```python
# 赵昱安 2200011450
t=int(input())
for _ in range(t):
    j=0
    q=[]
    n=int(input())
    for __ in range(n):
        type,x=map(int,input().split())
        if type==1:
            q.append(str(x))
        else:
            if len(q)==0:
                j+=1
            else:
                if x==0:
                    q.pop(0)
                else:
                    q.pop(-1)
    if len(q)==0:
        j+=1

    if j==1:
        print('NULL')
    else: 
        print(' '.join(q))  
```

![双端队列](C:\Users\赵昱安\Desktop\双端队列.jpg)



### 02694: 波兰表达式

http://cs101.openjudge.cn/practice/02694/

**思路：**波兰表达式是一种前序表达式。这道题的一个关键是要会用eval()内置函数来实现字符串到可以进行运算的表达式之间的转换，另一个关键点是理解前序表达式转换成中序表达式的逻辑。

**代码**

```python
# 赵昱安 2200011450
s=input().split()
def poland():
    a=s.pop(0)
    if a in '+-*/':
        return str(eval(poland()+a+poland()))
    else:
        return a
print("%.6f"%float(poland()))
```

![波兰表达式](C:\Users\赵昱安\Desktop\波兰表达式.jpg)



### 24591: 中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/

**思路：**利用Shunting Yard算法将中序表达式转换为后序表达式。

**代码**

```python
# 赵昱安 2200011450
def infix_to_postfix(expression):
    precedence={'+':1,'-':1,'*':2,'/':2}
    stack=[]
    postfix=[]
    number=''
    for char in expression:
        if char.isnumeric() or char=='.':
            number+=char
        else:
            if number:
                num=float(number)
                postfix.append(int(num) if num.is_integer() else num)
                number=''
            if char in '+-*/':
                while stack and stack[-1] in '+-*/' and precedence[char] <= precedence[stack[-1]]:
                    postfix.append(stack.pop())
                stack.append(char)
            elif char=='(':
                stack.append(char)
            elif char==')':
                while stack and stack[-1] !='(':
                    postfix.append(stack.pop())
                stack.pop()    
    if number:
        num=float(number)
        postfix.append(int(num) if num.is_integer() else num)
    while stack:
        postfix.append(stack.pop())
    return ' '.join(str(x) for x in postfix)

n=int(input())
for _ in range(n):
    expression=input()
    print(infix_to_postfix(expression))
```

![中转后](C:\Users\赵昱安\Desktop\中转后.jpg)



### 22068: 合法出栈序列

http://cs101.openjudge.cn/practice/22068/

**思路：**这道题考察“栈”这个数据结构，但是题干表述的有点不明白，一开始没读懂它想让我干什么（当然也可能是我理解能力有问题）。理解了题目之后定义一个函数用来判断是否为合法出栈顺序即可，但是中间的逻辑也有一点点复杂RE了很多次。

代码

```python
# 赵昱安 2200011450
def is_valid_pop_sequence(origin,output):
    if len(origin)!=len(output):
        return False

    stack=[]
    bank=list(origin)

    for i in output:
        while (not stack or stack[-1]!=i) and bank:
            stack.append(bank.pop(0))
        if not stack or stack[-1]!=i:
            return False
        stack.pop()

    return True

origin=input()
while True:
    try:
        output=input()
        if is_valid_pop_sequence(origin,output):
            print('YES')
        else:
            print('NO')
    except EOFError:
        break
```

![合法的出栈顺序](C:\Users\赵昱安\Desktop\合法的出栈顺序.jpg)



### 06646: 二叉树的深度

http://cs101.openjudge.cn/practice/06646/

**思路：**这道题其实就是考察我们是否学会了构建“二叉树”这一数据结构的基本部分，并且知道如何定义一个计算路径最大长度的函数。构建二叉树的过程是照着书一点点写的，非常不熟练，希望尽快对“树”熟悉起来。

**代码**

```python
# 赵昱安 2200011450
class TreeNode:
    def __init__(self,x):
        self.val=x
        self.left=None
        self.right=None

def build_tree(node_list):
    nodes={i:TreeNode(i) for i in range(1,len(node_list)+1)}
    for i,(left,right) in enumerate(node_list,1):
        if left!=-1:
            nodes[i].left=nodes[left]
        if right!=-1:
            nodes[i].right=nodes[right]
    return nodes[1]

def max_depth(root):
    if root is None:
        return 0
    else:
        left_depth=max_depth(root.left)
        right_depth=max_depth(root.right)
        return max(left_depth,right_depth)+1

n=int(input())
node_list=[]
for _ in range(n):
    left,right=map(int,input().split())
    node_list.append((left,right))
root=build_tree(node_list)
depth=max_depth(root)
print(depth)
```

![二叉树的深度](C:\Users\赵昱安\Desktop\二叉树的深度.jpg)



### 02299: Ultra-QuickSort

http://cs101.openjudge.cn/practice/02299/

**思路：**这道题考察的是排序的算法。我一开始直接使用暴力穷举的办法，不管怎么优化代码都一直超时，后来用GPT教给我的归并排序的分治算法AC了。通过这道题我意识到我对于排序算法还并不熟悉，需要再多进行排序问题的练习。

**代码**

```python
# 赵昱安 2200011450
def merge_count(arr,left,mid,right,temp):
    i,j,count,k=left,mid+1,0,left
    while i<=mid and j<=right:
        if arr[i]<=arr[j]:
            temp[k]=arr[i]
            i+=1
        else:
            temp[k]=arr[j]
            j+=1
            count+=(mid-i+1)
        k+=1
    while i<=mid:
        temp[k]=arr[i]
        i+=1
        k+=1
    while j<=right:
        temp[k]=arr[j]
        j+=1
        k+=1
    for p in range(left,right+1):
        arr[p]= temp[p]
    return count

def merge_sort_count(arr,left,right,temp):
    count=0
    if left < right:
        mid=(left+right)//2
        count+=merge_sort_count(arr, left, mid, temp)
        count+=merge_sort_count(arr, mid + 1, right, temp)
        count+=merge_count(arr, left, mid, right, temp)
    return count

while True:
    n = int(input())
    if n==0:
        break
    numbers = []
    for _ in range(n):
        numbers.append(int(input()))
    temp=[0]*n
    result=merge_sort_count(numbers,0,n-1,temp)
    print(result)
```

![排序](C:\Users\赵昱安\Desktop\排序.jpg)



## 2. 学习总结和收获

（1）这次作业让我发现“排序”算法是我的一个漏洞，以前并没有系统地掌握这个算法，之后要多找一些关于排序的资料和题目；

（2）初步了解了一点点“树”这个数据结构，但是还是非常不熟悉，构建树的过程还是只能照着书一点一点写，课下还需要自己多去学习研究一下树；

（3）前序、中序、后序表达式的相互转换好像懂了又没完全懂。逻辑基本明白了，但是一写代码就会卡壳，感觉对于“栈”的运用还是不熟练。
