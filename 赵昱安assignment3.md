# Assignment #3: March月考

Updated 1537 GMT+8 March 6, 2024

2024 spring, Complied by 赵昱安 物理学院



## 1. 题目

**02945: 拦截导弹**

http://cs101.openjudge.cn/practice/02945/

**思路：**感觉这是一个比较典型的动态规划问题，只要列出以每一个导弹作为结尾时的最大拦截数，再找到列表里的最大数即可。

##### 代码

```python
#赵昱安 2200011450 
k=int(input())
heights=list(map(int,input().split()))
dp=[1]*k
for i in range(1,k):
    for j in range(i):
        if heights[j]>=heights[i]:
            dp[i]=max(dp[i],dp[j]+1)
print(max(dp))
```

![拦截导弹](C:\Users\赵昱安\Desktop\拦截导弹.png)



**04147:汉诺塔问题(Tower of Hanoi)**

http://cs101.openjudge.cn/practice/04147

**思路：**这是一个函数递归问题。关键是找到最小的循环单元然后不断进行嵌套，注意每一次调用函数所对应的参数是如何变化的。

##### 代码

```python
#赵昱安 2200011450
def hanoi(n,start,target,road):
    if n==1:
        print(f"{n}:{start}->{target}")
    else:
        hanoi(n-1,start,road,target)
        print(f"{n}:{start}->{target}")
        hanoi(n-1,road,target,start)
n,start,road,target=map(str,input().split())
n=int(n)
hanoi(n,start,target,road)
```

![汉诺塔问题](C:\Users\赵昱安\Desktop\汉诺塔问题.png)



**03253: 约瑟夫问题No.2**

http://cs101.openjudge.cn/practice/03253

**思路：**虽然形式看上去复杂了一些，但本质与猴子选大王那道约瑟夫问题没有区别，只不过这道题要注意并不是从一号小朋友开始报数，而且最后要求输出每一个小朋友出局的顺序。整体思路没有问题，只要注意其中的一些小细节就好啦。

##### 代码

```python
# 赵昱安 2200011450
while True:
    n,p,m=map(int,input().split())
    if n==p==m==0:
        break

    if p==1:
        kids=[_ for _ in range(1,n+1)]
    else:
        kids=[]  
        for x in range(p,n+1):
            kids.append(x)
        for y in range(1,p):
            kids.append(y)

    answer=[]
    i=1
    while len(kids)>0:
        a=kids.pop(0)
        if i%m==0:
            answer.append(str(a))
        else:
            kids.append(a)
        i+=1
    print(','.join(answer))
```

![拦截导弹](C:\Users\赵昱安\Desktop\拦截导弹.png)



**21554:排队做实验 (greedy)v0.2**

http://cs101.openjudge.cn/practice/21554

**思路：**感觉算是一个基础的排序问题（也可以说是贪心？），逻辑上没有什么难的地方，只要注意时间相等的情况如何处理即可。

##### 代码

```python
# 赵昱安 2200011450
n=int(input())
t=list(map(int,input().split()))
T=sorted(t)
answer=[]
time=0
for i in range(n):
    for j in range(n):
        if t[j]==T[i] and str(j+1) not in answer:
            answer.append(str(j+1))
    time+=(n-1-i)*T[i]
print(' '.join(answer))
result="%.2f"%(time/n)
print(result)
```

![排队做试验](C:\Users\赵昱安\Desktop\排队做试验.png)



**19963:买学区房**

http://cs101.openjudge.cn/practice/19963

**思路：**用到了re库，不难，但是有点麻烦，需要预处理好数据之后遍历来找到满足条件的学区房数量。

##### 代码

```python
# 赵昱安 2200011450
import re
pattern=r'\d+'
n=int(input())
d=re.findall(pattern,input())
distances=[(int(d[i])+int(d[i+1])) for i in range(0,2*n,2)]
prices=list(map(int,input().split()))
values=[]
for j in range(n):
    values.append(distances[j]/prices[j])

Prices=sorted(prices)
Values=sorted(values)
if n%2==1:
    mp=Prices[(n-1)//2]
    mv=Values[(n-1)//2]
else:
    mp=(Prices[n//2]+Prices[n//2-1])/2
    mv=(Values[n//2]+Values[n//2-1])/2

count=0
for k in range(n):
    if prices[k]<mp and values[k]>mv:
        count+=1
print(count)
```

![买学区房](C:\Users\赵昱安\Desktop\买学区房.png)



**27300: 模型整理**

http://cs101.openjudge.cn/practice/27300

**思路：**感觉这道题在逻辑上没有什么难点，但是代码的实现比较繁琐，自己提交了很多次都是WA，最后还是用GPT提交成功的。它使用了一个我没见过的defaultdict()函数，把列表变成了字典，并且通过定义函数实现了数据大小的比较。

##### 代码

```python
# 赵昱安 2200011450
from collections import defaultdict

def convert_to_numeric(param_str):
    if param_str[-1] == 'B':
        return float(param_str[:-1])*1000
    else:
        return float(param_str[:-1])

n=int(input())
models=defaultdict(list)
for _ in range(n):
    model=input().strip()
    name,param=model.split('-')
    models[name].append(param)
for name,params in sorted(models.items()):
    formatted_params=sorted(params,key=convert_to_numeric)
    print(f"{name}: {', '.join(formatted_params)}")
```

![模型整理](C:\Users\赵昱安\Desktop\模型整理.png)



## 2. 学习总结和收获

本次月考在规定时间内AC5，除了最后一题其他都通过了（其中有两道以前做过原题），自己觉得比较满意。这次月考主要还是应用上学期计概学习的知识，所以还能够应对。但是后面的树和图对于我来说就非常陌生了，之后应该会把大部分精力投入到树和图等全新的数据结构的学习中。





