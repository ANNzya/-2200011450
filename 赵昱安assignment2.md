# Assignment #2: 编程练习

Updated 0953 GMT+8 Feb 24, 2024

2024 spring, Complied by 赵昱安 物理学院



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/2024sp_routine/27653/

思路：考察类的定义。

##### 代码

```python
# 赵昱安 2200011450
def gcd(m,n):    #找最大公因数
    while m%n!=0:
        oldm=m
        oldn=n
        m=oldn
        n=oldm%oldn
    return n

class Fraction:
    def __init__(self,top,bottom):
        self.num=top
        self.den=bottom
    
    def __str__(self):
        return str(self.num)+'/'+str(self.den)

    def __add__(self,otherfraction):
        newnum=self.num*otherfraction.den+self.den*otherfraction.num
        newden=self.den*otherfraction.den
        common=gcd(newnum,newden)
        return Fraction(newnum//common,newden//common)

a,b,c,d=map(int,input().split())
f1=Fraction(a,b)
f2=Fraction(c,d)
f3=f1+f2
print(f3)
```

![类](C:\Users\赵昱安\Desktop\类.jpg)



### 04110: 圣诞老人的礼物-Santa Clau’s Gifts

greedy/dp, http://cs101.openjudge.cn/practice/04110

思路：贪心法，先拿价值最大的糖果。

##### 代码

```python
# 赵昱安 2200011450
n,w=map(int,input().split())
V,W=[],[]
answer=0
for _ in range(n):
    a,b=map(int,input().split())
    V.append(a/b)
    W.append(b)

while w>0 and len(V)>0:
    m=max(V)
    i=V.index(m)
    answer+=m*min(w,W[i])
    w-=W[i]  
    V.pop(i)
    W.pop(i)         

print(format(answer,'.1f'))
```

![圣诞老人的礼物](C:\Users\赵昱安\Desktop\圣诞老人的礼物.jpg)



### 18182: 打怪兽

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/

思路：使用字典记录技能，然后按照时刻顺序依次使用技能。

##### 代码

```python
#赵昱安 2200011450
nCases=int(input())
for _ in range(nCases):
    situation='alive'
    n,m,b=map(int,input().split())
    skills={}
    for __ in range(n):
        ti,xi=map(int,input().split())
        if ti not in skills:
            skills[ti]=[xi]
        else:
            skills[ti].append(xi)

    S=sorted(skills)
    for i in S:
        if m>=len(skills[i]):
            b-=sum(skills[i])
        else:
            skills[i]=sorted(skills[i],reverse=True)
            b-=sum(skills[i][:m])
        if b<=0:
            situation=i
            break

    print(situation)
```

![打怪兽](C:\Users\赵昱安\Desktop\打怪兽.jpg)



### 230B. T-primes

binary search/implementation/math/number theory, 1300, http://codeforces.com/problemset/problem/230/B

思路：考察用欧拉筛/埃氏筛找出质数，并且判断一个数是否为完全平方数。

##### 代码

```python
# 赵昱安 2200011450
import math
def sushu(n):
    primes=[True]*(n+1)
    primes[0]=primes[1]=False
    x=2
    while x*x<=n:
        if primes[x]:
            for i in range(x*x,n+1,x):
                primes[i]=False
        x+=1
    return primes
 
n=int(input())
numbers=list(map(int,input().split()))
primes=sushu(int(math.sqrt(max(numbers))+1))
for number in numbers:
    a=math.sqrt(number)
    A=int(a)
    if A*A!=number:
        print('NO')
    else:
        if primes[A]:
            print('YES')
        else:
            print('NO')
```

![T-primes](C:\Users\赵昱安\Desktop\T-primes.jpg)



### 1364A. XXXXX

brute force/data structures/number theory/two pointers, 1200, https://codeforces.com/problemset/problem/1364/A

思路：使用双指针，一头一尾同时向中间遍历。

##### 代码

```python
#赵昱安 2200011450 
t=int(input())
for _ in range(t):
    n,x=map(int,input().split())
    array=list(map(int,input().split()))
    if sum(array)%x!=0:
        print(n)
    else:
        a,b=0,n-1
        while a<=b:
            if array[a]%x!=0 or array[b]%x!=0:
                print(n-1-a)
                break
            else:
                a+=1
                b-=1
        else:
            print(-1)

```

![XXXXX](C:\Users\赵昱安\Desktop\XXXXX.jpg)



### 18176: 2050年成绩计算

http://cs101.openjudge.cn/practice/18176/

思路：与“T-prime”题目结合，找出所有的T-prime的成绩后求平均值，并保留两位小数。

##### 代码

```python
# 赵昱安 2200011450
import math
prime=[False]*2+[True]*9999
i=2
while i<=100:
    if prime[i]:
        for j in range(i*i,10001,i):
            prime[j]=False
    i+=1

m,n=map(int,input().split())
answers=[]
for _ in range(m):
    scores=list(map(int,input().split()))
    answer=0
    count=0
    for k in range(len(scores)):
        if scores[k]==int(math.sqrt(scores[k]))**2:
            if prime[int(math.sqrt(scores[k]))]:
                answer+=scores[k]
                count+=1
    if count==0:
        answers.append('0')
    else:
        answers.append("{:.2f}".format(answer/len(scores)))

for answer in answers:
    print(answer)
```

![2050成绩计算](C:\Users\赵昱安\Desktop\2050成绩计算.jpg)



## 2. 学习总结和收获

​       坚持了几天做OJ“2024spring每日选做”，后来由于别的课程任务量上来了就没能坚持下去，希望后面至少每周能做几道每日选做，提升一下做题速度和做题感觉。





