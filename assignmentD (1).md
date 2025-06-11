# Assignment #D: 图 & 散列表

Updated 2042 GMT+8 May 20, 2025

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

### M17975: 用二次探查法建立散列表

http://cs101.openjudge.cn/practice/17975/

<mark>需要用这样接收数据。因为输入数据可能分行了，不是题面描述的形式。OJ上面有的题目是给C++设计的，细节考虑不周全。</mark>

```python
import sys
input = sys.stdin.read
data = input().split()
index = 0
n = int(data[index])
index += 1
m = int(data[index])
index += 1
num_list = [int(i) for i in data[index:index+n]]
```



思路：真阴啊

看了群讨论才发现问题



代码：

```python
import  sys
input = sys.stdin.read
data = input().split()
index = 0
n = int(data[index])
index += 1
m = int(data[index])
index += 1
num_list = [int(i) for i in data[index:index+n]]
num=1
used=set()
ans=[]
inlist=dict()
def f(x):
    return (-1)**(x-1)*((x+1)//2)**2
for i in num_list:
    if i in inlist:
        ans.append(inlist[i])
        continue
    t=i%m
    while t in used:
        t+=f(num)
        t%=m
        num+=1
    used.add(t)
    ans.append(t)
    inlist[i]=t
print(*ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250527185900612](d:\Users\m1885\Downloads\image-20250527185900612.png)



### M01258: Agri-Net

MST, http://cs101.openjudge.cn/practice/01258/

思路：



代码：

```python
import sys
import threading
def main():
    data = sys.stdin.read().split()
    idx = 0
    out = []
    while idx < len(data):
        N = int(data[idx]); idx += 1
        # 读入 N×N 矩阵
        dist = [[0]*N for _ in range(N)]
        for i in range(N):
            for j in range(N):
                dist[i][j] = int(data[idx]); idx += 1
        # Prim 算法 O(N^2)
        in_mst = [False]*N
        min_e = [10**9]*N
        min_e[0] = 0
        total = 0
        for _ in range(N):
            u = -1
            for v in range(N):
                if not in_mst[v] and (u == -1 or min_e[v] < min_e[u]):
                    u = v
            in_mst[u] = True
            total += min_e[u]
            for v in range(N):
                if not in_mst[v] and dist[u][v] < min_e[v]:
                    min_e[v] = dist[u][v]
        out.append(str(total))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### M3552.网络传送门旅游

bfs, https://leetcode.cn/problems/grid-teleportation-traversal/

思路：



代码：

```python
from collections import deque
from typing import List

class Solution:
    def minMoves(self, matrix: List[str]) -> int:
        m = len(matrix)
        n = len(matrix[0])
        grid = [list(row) for row in matrix]
        portals = {}
        for i in range(m):
            for j in range(n):
                c = grid[i][j]
                if 'A' <= c <= 'Z':
                    portals.setdefault(c, []).append((i, j))
        dist = [[-1] * n for _ in range(m)]
        dq = deque()
        dist[0][0] = 0
        dq.append((0, 0))
        used = {c: False for c in portals}
        while dq:
            x, y = dq.popleft()
            d = dist[x][y]
            if x == m - 1 and y == n - 1:
                return d
            for dx, dy in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                nx, ny = x + dx, y + dy
                if 0 <= nx < m and 0 <= ny < n and grid[nx][ny] != '#' and dist[nx][ny] == -1:
                    dist[nx][ny] = d + 1
                    dq.append((nx, ny))
            c = grid[x][y]
            if 'A' <= c <= 'Z' and not used[c]:
                for nx, ny in portals[c]:
                    if dist[nx][ny] ==-1 or dist[nx][ny]>d:
                        dist[nx][ny] = d
                        dq.appendleft((nx, ny))
                used[c] = True
        return -1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250527141715506](d:\Users\m1885\Downloads\image-20250527141715506.png)



### M787.K站中转内最便宜的航班

Bellman Ford, https://leetcode.cn/problems/cheapest-flights-within-k-stops/

思路：



代码：

```python

class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        INF = 10**9
        dp = [INF] * n
        dp[src] = 0
        for _ in range(k + 1):
            tmp = dp.copy()
            for u, v, w in flights:
                if dp[u] + w < tmp[v]:
                    tmp[v] = dp[u] + w
            dp = tmp
        return dp[dst] if dp[dst] < INF else -1

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250527151547707](d:\Users\m1885\Downloads\image-20250527151547707.png)



### M03424: Candies

Dijkstra, http://cs101.openjudge.cn/practice/03424/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### M22508:最小奖金方案

topological order, http://cs101.openjudge.cn/practice/22508/

思路：



代码：

```python
from collections import deque
import sys

def minimalBonuses(n, matches):
    rev = [[] for _ in range(n)]
    indeg = [0] * n
    for a, b in matches:
        rev[b].append(a)
        indeg[a] += 1
    dq = deque(i for i in range(n) if indeg[i] == 0)
    dp = [0] * n
    while dq:
        u = dq.popleft()
        for v in rev[u]:
            if dp[v] < dp[u] + 1:
                dp[v] = dp[u] + 1
            indeg[v] -= 1
            if indeg[v] == 0:
                dq.append(v)
    return sum(dp) + 100 * n

def main():
    data = sys.stdin.read().split()
    n, m = map(int, data[:2])
    matches = []
    idx = 2
    for _ in range(m):
        a = int(data[idx]); b = int(data[idx+1]); idx += 2
        matches.append([a, b])
    print(minimalBonuses(n, matches))

if __name__ == '__main__':
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250527151609846](d:\Users\m1885\Downloads\image-20250527151609846.png)



## 2. 学习总结和收获

感觉还得练











