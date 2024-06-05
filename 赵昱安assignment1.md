# Assignment #1: 拉齐大家Python水平

Updated 0940 GMT+8 Feb 19, 2024

2023 fall, Complied by 赵昱安 物理学院 2200011450



## 1. 题目

### 20742: 泰波拿契數

http://cs101.openjudge.cn/practice/20742/

**思路：**很简单的动态规划。题目中已经给出数列的前三项，只需根据数列的递推关系式确定数列后面的每一项即可。

##### 代码

```python
# 赵昱安 2200011450
n=int(input())
T=[0 for _ in range(31)]
T[1]=T[2]=1
for i in range(3,31):
    T[i]=T[i-1]+T[i-2]+T[i-3]
print(T[n])
```

![数列](C:\Users\赵昱安\Desktop\数列.png)



### 58A. Chat room

greedy/strings, 1000, http://codeforces.com/problemset/problem/58/A

**思路：**这道题应该可以用双指针做，但是为了避免出错，我使用了暴力穷举的方法，五重循环直接从输入的字符串中找出按顺序的排列的'h','e','l','l','o'，只要找到一个就是YES，找不到就是NO。

##### 代码

```python
# 赵昱安 2200011450
s=input()
l=len(s)
for a in range(l-4):
    if s[a]=='h':
        for b in range(a+1,l-3):
            if s[b]=='e':
                for c in range(b+1,l-2):
                    if s[c]=='l':
                        for d in range(c+1,l-1):
                            if s[d]=='l':
                                for e in range(d+1,l):
                                    if s[e]=='o':
                                        print('YES')
                                        exit(0)
else:
    print('NO')
```

![chat room](C:\Users\赵昱安\Desktop\chat room.jpg)



### 118A. String Task

implementation/strings, 1000, http://codeforces.com/problemset/problem/118/A

**思路：**先把所有的vowels字母存储在一个列表里。把输入的字符串全变成小写，找出不在列表里的字母，每个字母前面加上'.',最后合成一个字符串。

##### 代码

```python
# 赵昱安 2200011450
vowels=['a','o','y','e','u','i']
s=input().lower()
answer=[]
for i in range(len(s)):
    if s[i] not in vowels:
        answer.append('.'+s[i])
print(''.join(answer))
```

![string task](C:\Users\赵昱安\Desktop\string task.jpg)



### 22359: Goldbach Conjecture

http://cs101.openjudge.cn/practice/22359/

**思路：**用埃氏筛找到小于输入数字的所有质数，再寻找质数表中相加之和为输入数字的两个质数即可。

##### 代码

```python
#赵昱安 2200011450 
s=int(input())
primes=[]
is_prime=[True]*s
is_prime[0]=is_prime[1]=False
for i in range(1,s):
    if is_prime[i]:
        primes.append(i)
        for k in range(2*i,s,i):
            is_prime[k]=False
for a in primes:
    if s-a in primes:
        print(a,(s-a))
        exit(0)
```

![质数](C:\Users\赵昱安\Desktop\质数.jpg)



### 23563: 多项式时间复杂度

http://cs101.openjudge.cn/practice/23563/

**思路：**第一步，把多项式的每一项拆开；第二步，找出所有非0的项，并记录其次数；第三步，找到最高的次数；第四步，按要求格式输出。

##### 代码

```python
# 赵昱安 2200011450
s=input().split('+')
lst=[]
for i in s:
    I=i.split('^')
    if I[0]!='0n':
        lst.append(int(I[1]))
a=str(max(lst))
print('n^'+a)
```

![多项式时间复杂度](C:\Users\赵昱安\Desktop\多项式时间复杂度.jpg)



### 24684: 直播计票

http://cs101.openjudge.cn/practice/24684/

**思路：**利用字典这一数据结构进行计票，并通过逐个比对找出拥有最高票数的选项。

##### 代码

```python
# 赵昱安 2200011450
def find_max_votes(nums):
    votes={}
    max_votes=0
    result=[]

    for num in nums:
        if num in votes:
            votes[num]+=1
        else:
            votes[num]=1
        
        if votes[num]>max_votes:
            max_votes=votes[num]
    
    for num in nums:
        if votes[num]==max_votes:
            if int(num) not in result:
                result.append(int(num))
    return result

input_nums = input().split()
result=find_max_votes(input_nums)
result.sort()
for k in range(len(result)):
    result[k]=str(result[k])
print(' '.join(result))
```

![直播计票](C:\Users\赵昱安\Desktop\直播计票.jpg)



## 2. 学习总结和收获

​       本次作业的题目都相对基础，主要考察python的一些基础语法和操作，能够帮助我很好地回忆上学期学过的计算概论B。但通过这次作业我也发现自己对于python的一些基本数据结构的操作有些生疏，还需要再进一步复习。
