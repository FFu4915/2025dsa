# Assignment #C: 202505114 Mock Exam

Updated 1518 GMT+8 May 14, 2025

2025 spring, Complied by <mark>付麟瑞 数院</mark>



> **说明：**
>
> 1. **⽉考**：AC?<mark>（请改为同学的通过数）</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
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

### E06364: 牛的选举

http://cs101.openjudge.cn/practice/06364/

思路：



代码：

```python
n,k=map(int,input().split())
a=[]
for i in range(n):
    x,y=map(int,input().split())
    a.append([x,y,i+1])
a.sort(reverse=True)
b=a[:k]
b.sort(key=lambda x:x[1])
print(b[-1][-1])

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250518125650394](d:\Users\m1885\Downloads\image-20250518125650394.png)



### M04077: 出栈序列统计

http://cs101.openjudge.cn/practice/04077/

思路：



代码：

```python
ans=set()
def dfs(wl,stack,res):
    if not wl:
        stack.reverse()
        ans.add(tuple(res+stack))
        stack.reverse()
        return
    if stack:
        q=stack.pop(-1)
        dfs(wl,stack,res+[q])
        stack.append(q)
    p=wl.pop(-1)
    dfs(wl,stack+[p],res)
    wl.append(p)
n=int(input())
wl=list(range(n))
dfs(wl,[],[])
print(len(ans))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250518131208103](d:\Users\m1885\Downloads\image-20250518131208103.png)



### M05343:用队列对扑克牌排序

http://cs101.openjudge.cn/practice/05343/

思路：



代码：

```python
n=int(input())
a=list(input().split())
queue1=[[] for i in range(9)]
queue2=[[] for i in range(4)]
dic={'A':0,'B':1,'C':2,'D':3}
for p in a:
    i=int(p[1])
    queue1[i-1].append(p)
for i in queue1:
    for p in i:
        j=dic[p[0]]
        queue2[j].append(p)
for i in range(9):
    print(f'Queue{i+1}:',end='')
    print(*queue1[i])
for j in dic.keys():
    print(f'Queue{j}:',end='')
    print(*queue2[dic[j]])
b=[]
for j in queue2:
    for i in j:
        b.append(i)
print(*b)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250518132630262](d:\Users\m1885\Downloads\image-20250518132630262.png)



### M04084: 拓扑排序

http://cs101.openjudge.cn/practice/04084/

思路：

对dfs比较熟，不知道这个应该用kahn。失败

代码：

```python
n,e=map(int,input().split())
import heapq
class point:
    def __init__(self,key):
        self.key=key
        self.edges=[]
        self.innum=0
class graph:
    def __init__(self):
        self.points={}
        self.edges=[]
    def addpoint(self,key):
        if key not in self.points:
            self.points[key]=point(key)
    def addedge(self,a,b):
        self.addpoint(a)
        self.addpoint(b)
        self.points[a].edges.append(b)
        self.points[b].innum+=1
g=graph()
res=[]
for i in range(n):
    g.addpoint(i+1)
for i in range(e):
    a,b=map(int,input().split())
    g.addedge(a,b)
ans=[]
l=list(g.points.keys())
wl=[]
for i in l:
    if g.points[i].innum == 0:
        heapq.heappush(wl, i)
while wl:
    p=heapq.heappop(wl)
    q = g.points[p]
    for i in q.edges:
        g.points[i].innum -= 1
        if g.points[i].innum == 0:
            heapq.heappush(wl, i)
    res.append(p)
for i in res:
    ans.append('v'+str(i))
print(*ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250518145550275](d:\Users\m1885\Downloads\image-20250518145550275.png)



### M07735:道路

Dijkstra, http://cs101.openjudge.cn/practice/07735/

思路：



代码：

```python
import heapq


class point:
    def __init__(self, key):
        self.key = key
        self.edges = {}


class graph:
    def __init__(self):
        self.points = {}

    def addpoint(self, key):
        if key not in self.points:
            self.points[key] = point(key)

    def addedge(self,a, b, c, d):
        self.addpoint(a)
        self.addpoint(b)
        if b not in self.points[a].edges:
            self.points[a].edges[b] = [[c, d]]
        else:
            self.points[a].edges[b].append([c, d])


def bfs(s, e, fee):
    q = []
    q.append([0, 0, s])
    while q:
        dist, f, poz = heapq.heappop(q)
        if f > fee:
            continue
        if poz==e:
            return dist
        if poz in visited and visited[poz] <= f:
            continue
        visited[poz] = f
        for np, roads in g.points[poz].edges.items():
            for o in roads:
                heapq.heappush(q, [dist + o[0], f + o[1], np])
    return -1


fee = int(input())
n = int(input())
r = int(input())
g = graph()
visited = {}
for i in range(r):
    a, b, c, d = map(int, input().split())
    g.addedge(a, b, c, d)
print(bfs(1, n, fee))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250518141615066](d:\Users\m1885\Downloads\image-20250518141615066.png)



### T24637:宝藏二叉树

dp, http://cs101.openjudge.cn/practice/24637/

思路：



代码：

```python
class node:
    def __init__(self,val=0):
        self.val=val
        self.left=None
        self.right=None
def value(root):
    if not root:
        return 0
    return max(value(root.left)+value(root.right),nv(root.left)+nv(root.right)+root.val)
def nv(root):
    if not root:
        return 0
    return value(root.left)+value(root.right)
n=int(input())
values=list(map(int,input().split()))
root=node()
cur=[root]
k=0
while cur:
    nxt=[]
    for i in cur:
    	if k<n:
            i.val=values[k]
            k+=1
            i.left=node()
            i.right=node()
            nxt.append(i.left)
            nxt.append(i.right)
    cur=nxt
print(value(root))
    
        
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250518143051083](d:\Users\m1885\Downloads\image-20250518143051083.png)



## 2. 学习总结和收获

<mark>前面摆的厉害，现在已经把这些算法学过一遍了，剩下只需要反复刷熟练度+提高debug能力</mark>











