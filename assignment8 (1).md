# Assignment #8: 树为主

Updated 1704 GMT+8 Apr 8, 2025

2025 spring, Complied by <mark>数院 付麟瑞</mark>



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

### LC108.将有序数组转换为二叉树

dfs, https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/

思路：

感觉树很神奇的一点就是用一个非常简单的逻辑+递归即可实现想要的结果。每次写都要感叹

代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return
        mid=len(nums)//2
        root=TreeNode(nums[mid])
        root.left=self.sortedArrayToBST(nums[:mid])
        root.right=self.sortedArrayToBST(nums[mid+1:])
        return root
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250412103314916](d:\Users\m1885\Downloads\image-20250412103314916.png)



### M27928:遍历树

 adjacency list, dfs, http://cs101.openjudge.cn/practice/27928/

思路：

有时候感觉细节处理棘手，会想一些等价的方法来实现（鬼点子，启动

代码：

```python
n=int(input())
dic=dict()
dic1=dict()
class Node:
    def __init__(self,val=None):
        self.val=val
        self.children=[]
for _ in range(n):
    a=list(map(int,input().split()))
    t=a[0]
    if t in dic:
        node=dic[t]
    else:
        node=Node(t)
        dic[t]=node
        dic1[t]=0
    for i in range(1,len(a)):
        if a[i] in dic:
            node.children.append(dic[a[i]])
            dic1[a[i]]=1
        else:
            m=Node(a[i])
            dic[a[i]]=m
            node.children.append(m)
            dic1[a[i]]=1
for key,val in dic1.items():
    if val==0:
        root=dic[key]
        break

def show(root):
    if not root:
        return []
    if not root.children:
        return [root.val]
    root.children.append(Node(root.val))
    root.children.sort(key=lambda x: x.val)
    ans=[]
    for child in root.children:
        ans.extend(show(child))
    return ans
ans=show(root)
print(*ans, sep='\n')	
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250412111706822](d:\Users\m1885\Downloads\image-20250412111706822.png)



### LC129.求根节点到叶节点数字之和

dfs, https://leetcode.cn/problems/sum-root-to-leaf-numbers/

思路：



代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        def dfs(root,path,s):
            if not root:
                s+=int(path)
                return s
            else:
                path+=str(root.val)
                g=0
                if root.left:
                    s=dfs(root.left,path,s)
                    g+=1
                if root.right:
                    s=dfs(root.right,path,s)
                    g+=1
                if g==0:
                    s=dfs(root.left,path,s)
            return s
        return dfs(root,'',0)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250412113514524](d:\Users\m1885\Downloads\image-20250412113514524.png)



### M22158:根据二叉树前中序序列建树

tree, http://cs101.openjudge.cn/practice/22158/

思路：



代码：

```python
class Node:
    def __init__(self,val=None):
        self.val = val
        self.left = None
        self.right = None
def build(pre,inorder):
    if not pre:
        return
    root=Node(pre[0])
    ind=inorder.index(pre[0])
    root.left=build(pre[1:ind+1],inorder[:ind])
    root.right=build(pre[ind+1:],inorder[ind+1:])
    return root
def hou(root):
    if not root:
        return []
    return hou(root.left) + hou(root.right)+[root.val]
ans=[]
while 1:
    try:
        pre=input()
        inorder=input()
        root=build(pre,inorder)
        ans.append(''.join(map(str,hou(root))))
    except EOFError:
        break
for i in ans:
    print(i)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250412114908357](d:\Users\m1885\Downloads\image-20250412114908357.png)



### M24729:括号嵌套树

dfs, stack, http://cs101.openjudge.cn/practice/24729/

思路：



代码：

```python
class Node:
    def __init__(self, val,child=None):
        self.val = val
        self.child = []
a=input()
if len(a)<=1:
    print(a)
    print(a)
    exit()
def build(a,ind):
    if ind>len(a)-1:
        return None,ind
    node=Node(a[ind])
    ind+=1
    if a[ind]!='(':
        return node,ind
    while ind < len(a) and a[ind]!=')':
        ind+=1
        child,ind=build(a,ind)
        node.child.append(child)
    ind+=1
    return node,ind
def x(node):
    if not node:
        return []
    ans=[node.val]
    for i in range(len(node.child)):
        ans.extend(x(node.child[i]))
    return ans
def h(root):
    if not root:
        return []
    ans=[]
    for i in range(len(root.child)):
        ans.extend(h(root.child[i]))
    ans.append(root.val)
    return ans
def show(root):
    ans=[]
    cur=[root]
    while cur:
        res=[]
        nxt=[]
        for i in cur:
            if i!=None:
                res.append(i.val)
                for j in i.child:
                    nxt.append(j)
        cur=nxt
        ans.append(res)
    return ans
root,_=build(a,0)
print(''.join(x(root)))
print(''.join(h(root)))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250412114929566](d:\Users\m1885\Downloads\image-20250412114929566.png)



### LC3510.移除最小数对使数组有序II

doubly-linked list + heap, https://leetcode.cn/problems/minimum-pair-removal-to-sort-array-ii/

思路：

思路好乱，看到题之后几个想法是延迟删除字典，链表，heap，并查集，



gpt大人也无法战胜

就这样结束了吗



拼尽全力debug终于战胜，还是用pycharm把中间量print出来一点点检查好

pycharm得了mvp，gpt是躺赢狗（

本来以为链表很麻烦，当时脑子比较乱就没用链表

这并查集的写法也是很恶心啊

确实有些磨时间了



回想一下，感觉对并查集中的某些操作还不太熟悉，而且，对本题的逻辑还没有完全理顺。

我写的代码中有一部分完全是可以直接删去的

悲

代码：

```python
class Solution:
    def minimumPairRemoval(self, nums: List[int]) -> int:
        import heapq
        from collections import defaultdict
        class disjoint:
            def __init__(self,arr):
                self.parent=list(range(len(arr)))
                self.arr=arr
            def find(self,x):
                if x==self.parent[x]:
                    return x
                else:
                    self.parent[x]=self.find(self.parent[x])
                    return self.parent[x]
            def merge(self,x,y):
                rx=self.find(x)
                ry=self.find(y)
                if rx!=ry:
                    self.parent[ry]=rx
                    self.arr[rx]+=self.arr[ry]


        delay=defaultdict(int)
        deled=set()
        jian=set()
        opr=0
        a=[]
        b=disjoint(nums)
        for i in range(len(nums)-1):
            heapq.heappush(a,(nums[i]+nums[i+1],i,i+1))
            if nums[i+1]<nums[i]:
                jian.add(i)
        while jian:
            while 1:
                s,i,ne=heapq.heappop(a)
                if i in deled or ne in deled:
                    continue
                if not delay.get((s,i,ne),0):
                    break
                else:
                    delay[(s,i,ne)]-=1
            i=b.find(i)
            ne=b.find(ne)
            if ne in jian:
                jian.remove(ne)
            if i>0:
                p=b.find(i-1)
                heapq.heappush(a,(s+b.arr[p],p,i))
                delay[(b.arr[p]+b.arr[i],p,i)]+=1
                if b.arr[p]>s:
                    jian.add(p)
                else:
                    if p in jian:
                        jian.remove(p)
            l=i+1
            r=len(nums)-1
            new=r+1
            while l<=r:
                mid=(l+r)//2
                if b.find(mid)>ne:
                    new=b.find(mid)
                    r=mid-1
                else:
                    l=mid+1
            if new<len(nums):
                heapq.heappush(a,(s+b.arr[new],i,new))
                if s>b.arr[new]:
                    jian.add(i)
                else:
                    if i in jian:
                        jian.remove(i)
            else:
                if i in jian:
                    jian.remove(i)
            b.merge(i,ne)
            deled.add(ne)
            delay[(s,i,ne)]+=1
            opr+=1
        return opr
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250412133724341](d:\Users\m1885\Downloads\image-20250412133724341.png)



## 2. 学习总结和收获

<mark>万恶的期中QAQ</mark>











