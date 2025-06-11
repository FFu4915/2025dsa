# Assignment #9: Huffman, BST & Heap

Updated 1834 GMT+8 Apr 15, 2025

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

### LC222.完全二叉树的节点个数

dfs, https://leetcode.cn/problems/count-complete-tree-nodes/

思路：

最坏可能on

边缘情况瞎糊弄过去了（

代码：

```python
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        def height(root):
            if not root:
                return 0
            return 1+height(root.left)
        h=height(root)
        def poz(root,h):
            if not root:
                return 0
            if not root.left:
                return 1
            if h==1+height(root.right):
                return poz(root.right,h-1)+2**(h-2)
            return poz(root.left,h-1)
        return 2**(h-1)-1+poz(root,h)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250422160927847](d:\Users\m1885\Downloads\image-20250422160927847.png)



### LC103.二叉树的锯齿形层序遍历

bfs, https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/

思路：



代码：

```python
class Solution:
    def zigzagLevelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        ans=[]
        cur=[root]
        g=1
        while cur:
            nxt=[]
            res=[]
            for i in cur:
                if i:
                    nxt.append(i.left)
                    nxt.append(i.right)
            if g==1:
                for i in cur:
                    if i:
                        res.append(i.val)
                g=0
            else:
                for i in range(len(cur)-1,-1,-1):
                    if cur[i]:
                        res.append(cur[i].val)
                g=1
            if res:
                ans.append(res)
            cur=nxt
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250422162626707](d:\Users\m1885\Downloads\image-20250422162626707.png)



### M04080:Huffman编码树

greedy, http://cs101.openjudge.cn/practice/04080/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### M05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/

思路：



代码：

```python
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
intree=set()
def place(a,root=None):
    if root is None:
        root = Node(a)
        intree.add(a)
    if a not in intree:
        if a<root.val:
            root.left=place(a,root.left)
        else:
            root.right=place(a,root.right)
    return root
r=None
arr=list(map(int,input().split()))
for i in arr:
    r=place(i,r)
def ceng(root):
    if not root:
        return []
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
        if res:
            ans.extend(res)
        cur=nxt
    return ans
print(*ceng(r))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![image-20250422164032343](d:\Users\m1885\Downloads\image-20250422164032343.png)

### M04078: 实现堆结构

手搓实现，http://cs101.openjudge.cn/practice/04078/

类似的题目是 晴问9.7: 向下调整构建大顶堆，https://sunnywhy.com/sfbj/9/7

思路：

原来二叉搜索树这么好用

代码：

```python
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
def place(a,root=None):
    if root is None:
        root = Node(a)
        return root
    if a<root.val:
        root.left=place(a,root.left)
    else:
        root.right=place(a,root.right)
    return root
def popmin(root):
    if not root.left:
        return root.right,root.val
    else:
        root.left,ans=popmin(root.left)
        return root,ans
n=int(input())
r=None
for _ in range(n):
    msg=input()
    if msg.isdigit():
        r,ans=popmin(r)
        print(ans)
    else:
        __,a=map(int,msg.split())
        r=place(a,r)


```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250422165544284](d:\Users\m1885\Downloads\image-20250422165544284.png)



### T22161: 哈夫曼编码树

greedy, http://cs101.openjudge.cn/practice/22161/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>最近状态较差，还有一些论文压力，在慢慢进行复健训练</mark>











