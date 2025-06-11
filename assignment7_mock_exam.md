# Assignment #7: 20250402 Mock Exam

Updated 1624 GMT+8 Apr 2, 2025

2025 spring, Complied by <mark>付麟瑞 数院</mark>



> **说明：**
>
> 1. **⽉考**：AC?<mark>A!K!</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
>
> 2. **解题与记录：**
>
>    对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
> 3. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>
> 4. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
>
> 请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### E05344:最后的最后

http://cs101.openjudge.cn/practice/05344/



思路：



代码：

```python
n,k=map(int,input().split())
a=list(range(1,n+1))
b=[]
m=0
while a:
    m=(m+k-1)%len(a)
    b.append(a.pop(m))
b.pop()
print(*b)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250402171441427](D:\Users\m1885\Documents\WeChat Files\wxid_7tjob98aylie22\FileStorage\File\2025-04\image-20250402171441427.png)



### M02774: 木材加工

binary search, http://cs101.openjudge.cn/practice/02774/



思路：

典，但是因为右端点考试时取了一次min(a)没查出来把自己心态搞炸了，最后反复确认后面循环没写错，往前面查出来了

这个题交上去错了七八次，除0一次，还有mid，l,r，挨个试怎么不死循环（

代码：

```python
def check(l,k,a):
    c=0
    if l==0:
        return True 
    for i in a:
        c+=i//l
    if c>=k:
        return True
    return False
n,k=map(int,input().split())
a=[]
for i in range(n):
    a.append(int(input()))
l=0
ans=0
r=max(a)
while l<=r:
    mid=(l+r)//2
    if check(mid,k,a):
        ans=mid
        l=mid+1
    else:
        r=mid-1
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250402171619149](D:\Users\m1885\Documents\WeChat Files\wxid_7tjob98aylie22\FileStorage\File\2025-04\image-20250402171619149.png)



### M07161:森林的带度数层次序列存储

tree, http://cs101.openjudge.cn/practice/07161/



思路：学树的时候光顾着怎么遍历了，建树学的少

很明显能感觉到 层序遍历 和 先/后序遍历两类的区别，建树的时候也是根据这两种方法建，倒着来一遍就行



代码：

```python
from collections import deque
def show(node):
    if not node:
        return []
    else:
        w=[]
        for i in node.child:
            w+=show(i)
        return w+[node.val]
class Node:
    def __init__(self, val=None,n=0,child=None):
        self.val = val
        self.n = n
        self.child=[]
dum=root = Node()
n=int(input())
for i in range(n):
    a=deque()
    b=deque()
    msg=list(input().split())
    j=0
    while j<len(msg):
        a.append(msg[j])
        j+=1
        b.append(int(msg[j]))
        j+=1
    r=Node(a.popleft(),b.popleft())
    root.child.append(r)
    cur=[r]
    while cur:
        m=0
        nxt=[]
        t=0
        for j in cur:
            m+=j.n
        for _ in range(m):
            nxt.append(Node(a.popleft(),b.popleft()))
        for k in cur:
            for c in range(k.n):
                k.child.append(nxt[t])
                t+=1
        cur=nxt
ans=show(root)
ans.pop()
print(*ans)


```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250402172053533](D:\Users\m1885\Documents\WeChat Files\wxid_7tjob98aylie22\FileStorage\File\2025-04\image-20250402172053533.png)



### M18156:寻找离目标数最近的两数之和

two pointers, http://cs101.openjudge.cn/practice/18156/



思路：写的非常麻烦，因为当时第二题debug失败，第三题照先序遍历序列的思路建树失败，导致心态非常爆炸，刚好题也容易就东拼西凑出来了。

你懂的，狗急跳墙



代码：

```python
n=int(input())
a=list(map(int,input().split()))
a.sort()
ans=float('inf')
p=0
if a[0]+a[1]>=n:
    print(a[0]+a[1])
    exit()
elif a[-1]+a[-2]<=n:
    print(a[-1]+a[-2])
    exit()
i=0
j=len(a)-1
while i<j:
    if a[i]+a[j]==n:
        print(a[i]+a[j])
        exit()
    if a[i]+a[j]>n:
        c=ans
        ans=min(ans,a[i]+a[j]-n+0.1)
        if c>ans:
            p=1
        j-=1
        continue
    elif a[i]+a[j]<n:
        c = ans
        ans=min(ans,n-a[i]-a[j])
        if c>ans:
            p=0
        i+=1
        continue
if p:
    print(int(ans)+n)
else:
    print(n-int(ans))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250402172700484](D:\Users\m1885\Documents\WeChat Files\wxid_7tjob98aylie22\FileStorage\File\2025-04\image-20250402172700484.png)



### M18159:个位为 1 的质数个数

sieve, http://cs101.openjudge.cn/practice/18159/



思路：刚好看到10000就直接丢掉大脑了（笑）

如果1000000，我会想办法写欧拉筛，如果query很多，想用dp or一些其他办法避免每次query都遍历

但是AC了:(



代码：

```python
dp=[0]*10002
primes=[2,3]
p=list()
for i in range(5,10002):
    for j in primes:
        if j*j>i:
            primes.append(i)
            break
        if i%j==0:
            break
for i in primes:
    if i%10==1:
        p.append(i)
cases=int(input())
for _ in range(cases):
    n=int(input())
    a=[]
    print(f'Case{_+1}:')
    for j in p:
        if j>=n:
            break
        a.append(j)
    if not a:
        print('NULL')
    else:
        print(*a)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250402172938949](D:\Users\m1885\Documents\WeChat Files\wxid_7tjob98aylie22\FileStorage\File\2025-04\image-20250402172938949.png)



### M28127:北大夺冠

hash table, http://cs101.openjudge.cn/practice/28127/



思路：意义不明



代码：

```python
m=int(input())
teams=[]
t=dict()
tt=dict()
for _ in range(m):
    sc,pr,an=input().split(',')
    if sc not in t:
        t[sc]=set()
        tt[sc]=0
    tt[sc]+=1
    if an=='yes':
        t[sc].add(pr)
for team in t:
    teams.append([-len(t[team]),tt[team],team])
teams.sort()
for index in range(min(12,len(teams))):
    print(index+1,teams[index][2],-teams[index][0],teams[index][1])	
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20250402173746884](D:\Users\m1885\Documents\WeChat Files\wxid_7tjob98aylie22\FileStorage\File\2025-04\image-20250402173746884.png)



## 2. 学习总结和收获

<mark>下周高代期中考，好好复习一下，回来再好好过一遍树</mark>

做了做力扣周赛，见得多了以后，很多题出思路并不难，一些难题看对眼了基本能把该怎么优化也都摸得差不多

问题是：模板是不背的（）

经常会从之前写的题里面找代码块，或者交给gpt解决。

感觉下一步该把一些模板性的东西再熟练一些了。











