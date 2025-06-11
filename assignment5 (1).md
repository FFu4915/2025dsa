# Assignment #5: 链表、栈、队列和归并排序

Updated 1348 GMT+8 Mar 17, 2025

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

### LC21.合并两个有序链表

linked list, https://leetcode.cn/problems/merge-two-sorted-lists/

思路：力扣上链表的题做了一些，感觉基本掌握差不多了



代码：

```python
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dum=now=ListNode(0)
        while list1!=None and list2!=None:
            if list1.val<list2.val:
                now.next=list1
                list1=list1.next
                now=now.next
            else:
                now.next=list2
                list2=list2.next
                now=now.next
        now.next=list1 if list2==None else list2
        return dum.next
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250318234358703](d:\Users\m1885\Downloads\image-20250318234358703.png)



### LC234.回文链表

linked list, https://leetcode.cn/problems/palindrome-linked-list/

<mark>请用快慢指针实现。</mark>



代码：

```python
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        def findmid(head):
            fast=head
            slow=head
            while fast.next!=None and fast.next.next!=None:
                fast=fast.next.next
                slow=slow.next
            return slow 
            #若链表长度为2n-1或2n，slow为第n个
        def reverse(head):
            if head == None or head.next==None:
                return head
            ans=reverse(head.next)
            head.next.next=head
            head.next=None
            return ans
        mid=reverse(findmid(head).next)
        while mid!=None:
            if head.val!=mid.val:
                return False
            else:
                head=head.next
                mid=mid.next
        return True
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250318234544315](d:\Users\m1885\Downloads\image-20250318234544315.png)



### LC1472.设计浏览器历史记录

doubly-lined list, https://leetcode.cn/problems/design-browser-history/

<mark>请用双链表实现。</mark>

有趣。

代码：

```python
class Node:
    def __init__(self,val,next=None,prev=None):
        self.next=next
        self.prev=prev
        self.val=val
class BrowserHistory:

    def __init__(self, homepage: str):
        self.page=Node(homepage)

        

    def visit(self, url: str) -> None:
        self.page.next=Node(url,None,self.page)
        self.page=self.page.next

        

    def back(self, steps: int) -> str:
        for i in range(steps):
            if self.page.prev!=None:
                self.page=self.page.prev
        return self.page.val
        

    def forward(self, steps: int) -> str:
        for i in range(steps):
            if self.page.next!=None:
                self.page=self.page.next
        return self.page.val
        
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250318235707845](d:\Users\m1885\Downloads\image-20250318235707845.png)



### 24591: 中序表达式转后序表达式

stack, http://cs101.openjudge.cn/practice/24591/

思路：==给GPT跪辣！！==

我自己写出来的WA，RE，PE各一次，后面拆东墙补西墙，在gpt的指导下才终于凑出来一个丑陋的AC

chatgpt大人不用看题干，随手一写就能轻松AC，代码简洁优美，逻辑清晰，没有冗余

这比我强多了

言归正传，gpt说的 括号不参与优先级比较 和 把后序表达式作为一个列表，最后用join输出 这两点确实很有用，虽然我太困了懒得改（

代码：

```python
dic={'+':1,'-':1,'*':2,'/':2,'(':3,')':3}
cases=int(input())
for _ in range(cases):
    shi=input()
    stack=[]
    houshi=[]
    buf=''
    for i in shi:
        if i in dic:
            if buf:
                houshi.append(buf)
                buf=''
            if not stack:
                stack.append(i)
            elif dic[i]>dic[stack[-1]] or stack[-1]=='(':
                stack.append(i)
                if i==')':
                    if stack[-1]==')':
                        stack.pop()
                    while stack[-1]!='(':
                        houshi.append(stack.pop())
                    stack.pop()
            else:
                while stack and dic[stack[-1]]>=dic[i] and stack[-1]!='(':
                    houshi.append(stack.pop())
                stack.append(i)

        else:
            buf+=i
    if buf:
        houshi.append(buf)
    while stack:
        houshi.append(stack.pop())
    houshi=' '.join(houshi)
    print(houshi)
```



第二天改了

```
dic={'+':1,'-':1,'*':2,'/':2}
cases=int(input())
for _ in range(cases):
    shi=input()
    stack=[]
    houshi=[]
    buf=''
    for i in shi:
        if i in dic:
            if buf:
                houshi.append(buf)
                buf=''
            if not stack:
                stack.append(i)
            elif stack[-1] =='(' or dic[i]>dic[stack[-1]]:
                stack.append(i)
            else:
                while stack  and stack[-1]!='(' and dic[stack[-1]]>=dic[i]:
                    houshi.append(stack.pop())
                stack.append(i)

        else:
            if i == '(':
                if buf:
                    houshi.append(buf)
                    buf = ''
                stack.append(i)
            elif i == ')':
                if buf:
                    houshi.append(buf)
                    buf = ''
                while stack[-1] != '(':
                    houshi.append(stack.pop())
                stack.pop()
            else:
                buf+=i
    if buf:
        houshi.append(buf)
    while stack:
        houshi.append(stack.pop())
    houshi=' '.join(houshi)
    print(houshi)
```

代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250319010513749](d:\Users\m1885\Downloads\image-20250319010513749.png)



### 03253: 约瑟夫问题No.2

queue, http://cs101.openjudge.cn/practice/03253/

<mark>请用队列实现。</mark>



代码：链表写上瘾了写个链表先（竟然没超时）

```python
class Node:
    def __init__(self,val,next=None):
        self.val = val
        self.next = next
while 1:
    head=a=Node(1)
    n,p,m=map(int,input().split())
    if (n,p,m)==(0,0,0):
        break
    for i in range(1,n):
        a.next=Node(i+1)
        a=a.next
    a.next=head
    ans=[]
    for i in range(p-1):
        a=a.next
    while a.next!=a:
        for i in range(1,m):
            a=a.next
        b=a.next
        a.next=b.next
        ans.append(b.val)
    ans.append(a.val)
    print(*ans,sep=',')
```



这是队列。。吧？

有pop，是队列（

```
while 1:
    n,p,m=map(int,input().split())
    if (n,p,m)==(0,0,0):
        break
    a=p-1
    b=list(range(1,n+1))
    ans=[]
    while b:
        a=(a+m-1)%len(b)
        ans.append(b.pop(a))
    print(*ans,sep=',')
```



拼尽全力，这应该是队列罢

不得不说思路往某个固定方法上凑真的好怪

题目限时挺松的

```
from collections import deque
while 1:
    n,p,m=map(int,input().split())
    if (n,p,m)==(0,0,0):
        break
    a=p
    b=deque(list(range(a,n+1))+list(range(1,a)))
    c=[]
    ans=[]
    while b:
        for i in range(m-1):
            t=b.popleft()
            b.append(t)
        ans.append(b.popleft())
    print(*ans,sep=',')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250319141921953](d:\Users\m1885\Downloads\image-20250319141921953.png)



### 20018: 蚂蚁王国的越野跑

merge sort, http://cs101.openjudge.cn/practice/20018/

思路：



代码：

本能反应了（

竟然没超时,都O（n^2)了

```python
import bisect
a=[]
n=int(input())
ans=0
for i in range(n):
    v=int(input())
    t=bisect.bisect_left(a,v)
    ans+=t
    a.insert(t,v)
print(ans)
```



写下来才发现，利用mergesort的过程，排的时候记下数就行	

```
a=[]
n=int(input())
c=0
for i in range(n):
    a.append(int(input()))
def merge(qian,hou):
    global c
    a=len(qian)
    b=len(hou)
    new=[]
    i=0
    j=0
    while i<a and j<b:
        if hou[j]>qian[i]:
            new.append(hou[j])
            c+=a-i
            j+=1
        else:
            new.append(qian[i])
            i+=1
    if i==a:
        new.extend(hou[j:])
    else:
        new.extend(qian[i:])
    return new
def mergesort(a):
    global c
    if len(a)<2:
        return a
    left=a[:len(a)//2]
    right=a[len(a)//2:]
    l=mergesort(left)
    r=mergesort(right)
    return merge(l, r)
mergesort(a)
print(c)
```

代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250319143245430](d:\Users\m1885\Downloads\image-20250319143245430.png)

![image-20250319145333035](d:\Users\m1885\Downloads\image-20250319145333035.png)

## 2. 学习总结和收获

<mark>在做每日选做和力扣上的题，现在每天都练感觉顺手多了</mark>











