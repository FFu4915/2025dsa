# Assignment #6: 回溯、树、双向链表和哈希表

Updated 1526 GMT+8 Mar 22, 2025

2025 spring, Complied by <mark>付麟瑞 数院</mark>



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

### LC46.全排列

backtracking, https://leetcode.cn/problems/permutations/

思路：唉偷懒小子permutation还不会拼



代码：

```python
def permute(self, nums: List[int]) -> List[List[int]]:
        import itertools
        return list(itertools.permutations(nums))
        
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250324160027164](d:\Users\m1885\Downloads\image-20250324160027164.png)



### LC79: 单词搜索

backtracking, https://leetcode.cn/problems/word-search/

思路：

现在已经很自然地写dfs递归backtracking都这样搞，完全混为一谈了XD

代码：

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        l=len(word)
        m=len(board)
        n=len(board[0])
        moves=[[0,1],[0,-1],[-1,0],[1,0]]
        def find(word,k,searched,now):
            if k==l:
                return True
            else:
                for v in moves:
                    x=now[0]+v[0]
                    y=now[1]+v[1]
                    if 0<=x<m and 0<=y<n and board[x][y]==word[k] and (x,y) not in searched:
                         if find(word,k+1,searched|{(x,y)},(x,y)):
                            return True
                return False
        for i in range(m):
            for j in range(n):
                if board[i][j]==word[0]:
                    if find(word,1,{(i,j)},(i,j)):
                        return True
        return False
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250324161704115](d:\Users\m1885\Downloads\image-20250324161704115.png)



### LC94.二叉树的中序遍历

dfs, https://leetcode.cn/problems/binary-tree-inorder-traversal/

思路：

还没学树，但是有空了就看了看新的每日选做

感觉来了，仿着之前看过的递归写了点，还挺有意思

代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        return self.inorderTraversal(root.left)+[root.val]+self.inorderTraversal(root.right)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250324222023284](d:\Users\m1885\Downloads\image-20250324222023284.png)



### LC102.二叉树的层序遍历

bfs, https://leetcode.cn/problems/binary-tree-level-order-traversal/

思路：

不妙，好像n^2，时间好长

代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        def merge(a,b):
            c=len(a)
            d=len(b)
            if c<d:
                return [a[i]+b[i] for i in range(c)]+b[c:]
            else:
                return [a[i]+b[i] for i in range(d)]+a[d:]
        if not root:
            return []
        return [[root.val]]+merge(self.levelOrder(root.left),self.levelOrder(root.right))
```

看了一眼题解，瞬间感觉执着于递归的自己好蠢

回头想来，层序遍历这样写非常自然。每个列表是一层，然后获取每个元素的left和right，这样逐层推进确实是最直接最方便的做法

相比之下强行递归显得过于丑陋了

```
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        ans=[]
        cur=[root]
        while cur:
            res=[]
            nxt=[]
            for i in cur:
                if i:
                    res.append(i.val)
                    nxt.append(i.left)
                    nxt.append(i.right)
            cur=nxt
            if res:
                ans.append(res)
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250324223744213](d:\Users\m1885\Downloads\image-20250324223744213.png)

### LC131.分割回文串

dp, backtracking, https://leetcode.cn/problems/palindrome-partitioning/

思路：前几天看到有个dp的类似用法遂直接仿写

dp真是神奇



代码：

```python
def partition(self, s: str) -> List[List[str]]:
        dp = [[] for i in range(len(s))]
        for i in range(len(s)):
            for j in range(i + 1):
                if s[j] == s[i]:
                    l = s[j:i + 1]
                    if l == l[::-1]:
                        if j==0:
                            dp[i].append([l])
                        else:
                            for t in dp[j - 1]:
                                dp[i].append(t + [l])
        return dp[-1]
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250324155428188](d:\Users\m1885\Downloads\image-20250324155428188.png)



### LC146.LRU缓存

hash table, doubly-linked list, https://leetcode.cn/problems/lru-cache/

思路：一开始没想到dic里面放node，只用node无法快速定位，只用dic顺序问题就很难搞。

曾经尝试 队列+次数字典，更新后次数+1并加入到队列，put进新元素时，队列popleft直到拿出出现次数==1的元素将其删去。

当天未能AC，复查发现漏了一行，成功AC

```python
from collections import deque
class LRUCache:

    def __init__(self, capacity: int):
        self.c=capacity
        self.n=0
        self.dic=dict()
        self.dic2=dict()
        self.cache=deque([])

    def get(self, key: int) -> int:
        if key in self.dic:
            self.cache.append(key)
            self.dic2[key]+=1
            return self.dic[key]
        else:
            return -1


    def put(self, key: int, value: int) -> None:
        if key not in self.dic:
            self.n+=1
            self.dic2[key]=0
        self.dic[key]=value
        self.cache.append(key)
        self.dic2[key]+=1
        if self.n>self.c:
            while 1:
                a=self.cache.popleft()
                self.dic2[a]-=1
                if self.dic2[a]==0:
                    del self.dic[a]
                    self.n-=1
                    break
```

![image-20250324152357976](d:\Users\m1885\Downloads\image-20250324152357976.png)

代码：

照着题解思路写的

```python
class Node:
    def __init__(self,key,val,next=None,prev=None):
        self.val=val
        self.key=key
        self.next=next
        self.prev=prev
class LRUCache:

    def __init__(self, capacity: int):
        self.c=capacity
        self.n=0
        self.dic=dict()
        self.head=None
        self.tail=None

    def get(self, key: int) -> int:
        if key in self.dic:
            self.update(key)
            return self.dic[key].val
        else:
            return -1
    def update(self,key):
        if self.dic[key].prev==None:
            return 
        if self.dic[key].next==None:
            self.dic[key].prev.next=None
            self.tail=self.dic[key].prev
        else:
            self.dic[key].next.prev=self.dic[key].prev
            self.dic[key].prev.next=self.dic[key].next
        self.dic[key].next=self.head
        self.dic[key].prev=None
        self.head.prev=self.dic[key]
        self.head=self.dic[key]


    def put(self, key: int, value: int) -> None:
        if key not in self.dic:
            self.n+=1
            if not self.head:
                self.head=self.tail=Node(key,value)
                self.dic[key]=self.head
            else:
                self.head.prev=Node(key,value,self.head)
                self.head=self.head.prev
                self.dic[key]=self.head
                if self.n>self.c:
                    k=self.tail.key
                    self.tail.prev.next=None
                    self.tail=self.tail.prev
                    del self.dic[k]  
        else:
            self.update(key)
            self.head.val=value
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250324151251571](d:\Users\m1885\Downloads\image-20250324151251571.png)



## 2. 学习总结和收获

<mark>一直在跟进每日选做和力扣的题，现在用到的基本都会了，正在拓展自己的知识面，向大佬学思路（chatgpt立大功！）</mark>











