# Assignment #A: Graph starts

Updated 1830 GMT+8 Apr 22, 2025

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

### M19943:图的拉普拉斯矩阵

OOP, implementation, http://cs101.openjudge.cn/practice/19943/

要求创建Graph, Vertex两个类，建图实现。

思路：



代码：

```python
class Vertex:
    def __init__(self,key):
        self.key = key
        self.neighbors ={}
    def setNeighbor(self,other,weight=0):
        self.neighbors[other] = weight
    def getNeighbor(self,other):
        return self.neighbors.get(other,None)
class Graph:
    def __init__(self):
        self.vertices = {}
    def addvertex(self,key):
        self.vertices[key] = Vertex(key)
    def addedge(self,v1,v2,weight=0):
        if v1 not in self.vertices:
            self.vertices[v1] = Vertex(v1)
        if v2 not in self.vertices:
            self.vertices[v2] = Vertex(v2)
        self.vertices[v1].setNeighbor(self.vertices[v2],weight)
        self.vertices[v2].setNeighbor(self.vertices[v1],weight)
    def build_matrix(self):
        n=len(self.vertices)
        matrix=[[0]*n for i in range(n)]
        for i in self.vertices.keys():
            for j in self.vertices[i].neighbors.keys():
                matrix[i][j.key]-=1
            matrix[i][i]=len(self.vertices[i].neighbors)
        return matrix
n,m=map(int,input().split())
g=Graph()
for i in range(n):
    g.addvertex(i)
for i in range(m):
    a,b=map(int,input().split())
    g.addedge(a,b)
matrix=g.build_matrix()
for i in matrix:
    print(*i)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250429214639536](d:\Users\m1885\Downloads\image-20250429214639536.png)

### LC78.子集

backtracking, https://leetcode.cn/problems/subsets/

思路：



代码：

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        def pw(a,b):
            if not a:
                return [[]+b]
            else:
                t=[]
                for i in pw(a[1:],b):
                    t.append(i)
                    t.append(i+[a[0]])
                return t
        return pw(nums,[])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250429214711557](d:\Users\m1885\Downloads\image-20250429214711557.png)



### LC17.电话号码的字母组合

hash table, backtracking, https://leetcode.cn/problems/letter-combinations-of-a-phone-number/

思路：



代码：

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return []
        phone_map = {'2': ['a', 'b', 'c'],'3': ['d', 'e', 'f'],'4': ['g', 'h', 'i'],'5': ['j', 'k', 'l'],'6': ['m', 'n', 'o'],'7': ['p', 'q', 'r', 's'],'8': ['t', 'u', 'v'],'9': ['w', 'x', 'y', 'z']}
        def func(k,n,string):
            if k==n:
                ans.append(string)
                return
            else:
                for i in phone_map[digits[k]]:
                    func(k+1,n,string+i)
        ans=[]
        func(0,len(digits),'')
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250429221657684](d:\Users\m1885\Downloads\image-20250429221657684.png)



### M04089:电话号码

trie, http://cs101.openjudge.cn/practice/04089/

思路：

WA了问gpt，他说我算法不对，我就和他争（

吵了半天互相没能说服，上群里一看，好家伙，是我s==1的时候直接break导致后续输入错乱

代码：

```python
cases=int(input())
class TrieNode:
    def __init__(self):
        self.children = {}
class Trie:
    def __init__(self):
        self.root = TrieNode()
        self.root.children = {'*':1}
    def insert(self, word):
        node = self.root
        for x in word:
            if x not in node.children:
                node.children[x] = TrieNode()
            node = node.children[x]
    def search(self, word):
        node = self.root
        for x in word:
            if x not in node.children:
                if not node.children:
                    return True
                return False
            node = node.children[x]
        return True
for _ in range(cases):
    n=int(input())
    a=Trie()
    s=0
    for i in range(n):
        num=input()
        s+=a.search(num)
        a.insert(num)
    if s==0:
        print('YES')
        continue
    print('NO')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250429225428151](d:\Users\m1885\Downloads\image-20250429225428151.png)



### T28046:词梯

bfs, http://cs101.openjudge.cn/practice/28046/

思路：

被大小写埋伏了，直接让AI帮忙把大小写文体改了（检查了一下确实只有这里有出入）

代码：

```python
from collections import deque
import string

n = int(input())
words = set()
# ------------------ 修改这里 ------------------
letters = list(string.ascii_letters)   # 'a'–'z' + 'A'–'Z'
# --------------------------------------------

class Vertex:
    def __init__(self, key):
        self.key = key
        self.neighbors = {}
    def setNeighbor(self, other, weight=0):
        self.neighbors[other] = weight

class Graph:
    def __init__(self):
        self.vertices = {}
    def addvertex(self, key):
        if key not in self.vertices:
            self.vertices[key] = Vertex(key)
    def addedge(self, v1, v2, weight=0):
        if v1 not in self.vertices:
            self.vertices[v1] = Vertex(v1)
        if v2 not in self.vertices:
            self.vertices[v2] = Vertex(v2)
        self.vertices[v1].setNeighbor(self.vertices[v2], weight)
        self.vertices[v2].setNeighbor(self.vertices[v1], weight)

g = Graph()
for _ in range(n):
    word = input().strip()
    g.addvertex(word)
    for j in range(4):
        for k in letters:
            cand = word[:j] + k + word[j+1:]
            if cand in words:
                g.addedge(cand, word)
    words.add(word)

s, e = input().split()

def bfs(s, e, g):
    q = deque([[s, [s]]])
    seen = {s}
    while q:
        now, path = q.popleft()
        if now == e:
            return path
        for v in g.vertices[now].neighbors:
            nk = v.key
            if nk not in seen:
                seen.add(nk)
                q.append([nk, path + [nk]])
    return None

path = bfs(s, e, g)
print(*path) if path else print("NO")

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250429233353697](d:\Users\m1885\Downloads\image-20250429233353697.png)



### T51.N皇后

backtracking, https://leetcode.cn/problems/n-queens/

思路：

经典老题懒得再来一遍了

代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>学图爽</mark>











