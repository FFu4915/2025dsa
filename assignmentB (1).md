# Assignment #B: 图为主

Updated 2223 GMT+8 Apr 29, 2025

2025 spring, Complied by <mark>同学的姓名、院系</mark>



> **说明：**
>
> 1. **解题与记录：**
>
>    对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
> 2. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>
> 3. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
>
> 请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### E07218:献给阿尔吉侬的花束

bfs, http://cs101.openjudge.cn/practice/07218/

思路：



代码：

```python
import sys
from collections import deque

input = sys.stdin.readline

t = int(input())
for _ in range(t):
    r, c = map(int, input().split())
    grid = [list(input().rstrip('\n')) for _ in range(r)]
    dist = [[-1] * c for _ in range(r)]
    dq = deque()
    for i in range(r):
        for j in range(c):
            if grid[i][j] == 'S':
                dq.append((i, j))
                dist[i][j] = 0
            elif grid[i][j] == 'E':
                ex, ey = i, j
    ans = "oop!"
    while dq:
        x, y = dq.popleft()
        if (x, y) == (ex, ey):
            ans = str(dist[x][y])
            break
        for dx, dy in ((1,0),(-1,0),(0,1),(0,-1)):
            nx, ny = x + dx, y + dy
            if 0 <= nx < r and 0 <= ny < c and dist[nx][ny] == -1 and grid[nx][ny] != '#':
                dist[nx][ny] = dist[x][y] + 1
                dq.append((nx, ny))
    print(ans)
 	
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250504165916141](d:\Users\m1885\Downloads\image-20250504165916141.png)



### M3532.针对图的路径存在性查询I

disjoint set, https://leetcode.cn/problems/path-existence-queries-in-a-graph-i/

思路：最喜欢的并查集



代码：

```python
class Solution:
    def pathExistenceQueries(self, n: int, nums: List[int], maxDiff: int, queries: List[List[int]]) -> List[bool]:
        class bcj:
            def __init__(self,n):
                self.parent=list(range(n))
            def find(self,x):
                if x!=self.parent[x]:
                    self.parent[x]=self.find(self.parent[x])
                return self.parent[x]
            def merge(self,x,y):
                rx=self.find(x)
                ry=self.find(y)
                if rx!=ry:
                    self.parent[rx]=ry
        a=bcj(n)
        i=0
        j=0
        while j<n:
            if nums[j]-nums[i]<=maxDiff:
                a.merge(i,j)
            i=j
            j+=1
        ans=[]
        for query in queries:
            ans.append(bool(a.find(query[0])==a.find(query[1])))
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250504172604462](d:\Users\m1885\Downloads\image-20250504172604462.png)



### M22528:厚道的调分方法

binary search, http://cs101.openjudge.cn/practice/22528/

思路：



代码：

```python
scores=list(map(float,input().split()))
scores.sort()
n=len(scores)
m=int(n*0.4)
score=scores[m]
def check(b,s):
    if b*s/10**9+1.1**(b*s/10**9)>=85:
        return True
    return False
l=0
ans=0
r=10**9
while l<=r:
    mid=(l+r)//2
    if check(mid,score):
        ans=mid
        r=mid-1
    else:
        l=mid+1
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250504174349335](d:\Users\m1885\Downloads\image-20250504174349335.png)



### Msy382: 有向图判环 

dfs, https://sunnywhy.com/sfbj/10/3/382

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### M05443:兔子与樱花

Dijkstra, http://cs101.openjudge.cn/practice/05443/

思路：自己做一遍dijkstra才发现之前的掌握只是在思路上理解，没有深入到具体实现细节，唉唉菜就多练



代码：

```python
import heapq
from collections import defaultdict
class vertex:
    def __init__(self, name):
        self.name = name
        self.neighbors ={}
class graph:
    def __init__(self):
        self.vertices ={}
    def add_vertex(self, vertex):
        self.vertices[vertex.name] = vertex
    def add_edge(self, vertex, edge,distance):
        self.vertices[vertex.name].neighbors[edge.name] = distance
        self.vertices[edge.name].neighbors[vertex.name] = distance
n=int(input())
g=graph()
for i in range(n):
    name=input()
    g.add_vertex(vertex(name))
m=int(input())
for i in range(m):
    name1,name2,dist=input().split()
    dist=int(dist)
    g.add_edge(g.vertices[name1],g.vertices[name2],dist)
p=int(input())
def dijkstra(s,e):
    q=[]
    inq=defaultdict(lambda:float('inf'))
    heapq.heappush(q,(0,s,[s]))
    inq[s]=0
    visited=set()
    while q:
        dist,now,path=heapq.heappop(q)
        if now in visited:
            continue
        visited.add(now)
        if now==e:
            return path
        for neighbor in g.vertices[now].neighbors.keys():
            if dist+g.vertices[now].neighbors[neighbor]<inq[neighbor]:
                inq[neighbor]=dist+g.vertices[now].neighbors[neighbor]
                heapq.heappush(q,(dist+g.vertices[now].neighbors[neighbor],neighbor,path+[neighbor]))

for i in range(p):
    s,e=input().split()
    path=dijkstra(s,e)
    out=[s]
    for i in range(1,len(path)):
        d=g.vertices[path[i-1]].neighbors[path[i]]
        out.append(f"->({d})->{path[i]}")
    print("".join(out))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250504183508022](d:\Users\m1885\Downloads\image-20250504183508022.png)



### T28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/

思路：本来dps就能行，但是还是想尽办法让图有点参与感。。。

向AI学习了简单的MRV，本来想用heap来实现，但是依然超时，讨论过后发现heap是害人精，直接列表排序快多了🤣



代码：

```python
class vertex:
    def __init__(self, x, y):
        self.key=(x,y)
        self.neighbors={}
        self.ways=0
edges=[(1,2),(1,-2),(-1,2),(-1,-2),(2,1),(-2,-1),(2,-1),(-2,1)]
class Graph:
    def __init__(self,n):
        self.vertices={}
        self.n=n
    def add_vertex(self, vertex):
        self.vertices[vertex.key]=vertex
    def add_edge(self, vertex):
        for edge in edges:
            newkey=(vertex.key[0]+edge[0],vertex.key[1]+edge[1])
            if 0<=newkey[0]<self.n and 0<=newkey[1]<self.n:
                vertex.neighbors[newkey]=g.vertices[newkey]
                vertex.ways+=1



n=int(input())
g=Graph(n)
visited = [[False]*n for _ in range(n)]
for i in range(n):
    for j in range(n):
        g.add_vertex(vertex(i, j))
for i in range(n):
    for j in range(n):
        g.add_edge(g.vertices[(i,j)])
s=tuple(map(int,input().split()))
visited[s[0]][s[1]]=1
success=False
def dfs(now,visited,m,n):
    global success
    if success:
        return
    if m==n*n:
        success=True
    wl=[]
    for neighbor in now.neighbors.values():
        wl.append((neighbor.ways,neighbor))
    wl.sort(key=lambda x:x[0])
    for _,neighbor in wl:
        if not visited[neighbor.key[0]][neighbor.key[1]]:
            visited[neighbor.key[0]][neighbor.key[1]]=1
            for neighbr in now.neighbors.values():
                neighbr.ways-=1
            dfs(neighbor,visited,m+1,n)
            visited[neighbor.key[0]][neighbor.key[1]]=0
            for neighbr in now.neighbors.values():
                neighbr.ways+=1
dfs(g.vertices[s],visited,1,n)
if success:
    print('success')
else:
    print('fail')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250504124815699](d:\Users\m1885\Downloads\image-20250504124815699.png)



## 2. 学习总结和收获

<mark>过去摆烂的我，已经死了（）</mark>





