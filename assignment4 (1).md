# Assignment #4: 位操作、栈、链表、堆和NN

Updated 1203 GMT+8 Mar 10, 2025

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

### 136.只出现一次的数字

bit manipulation, https://leetcode.cn/problems/single-number/



<mark>请用位操作来实现，并且只使用常量额外空间。</mark>

完全不会！看了题解，提到异或满足交换律和结合律，再由a^a=0，0 ^a=a就可得到这个

代码：

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        x = 0
        for num in nums:  # 1. 遍历 nums 执行异或运算
            x ^= num      
        return x
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250313192355259](d:\Users\m1885\Downloads\image-20250313192355259.png)



### 20140:今日化学论文

stack, http://cs101.openjudge.cn/practice/20140/



思路：



代码：

```python
def unpack(a):
    res=''
    i=0
    while i < len(a) :
        if a[i]=='[':
            c=0
            for j in range(i+1,len(a)):
                if a[j]==']':
                    c+=1
                if a[j]=='[':
                    c-=1
                if c==1:
                    m=''
                    for k in range(i+1,j):
                        if a[k].isdigit():
                            m+=a[k]
                        else:
                            break
                    msg=unpack(a[k:j])
                    break

            res+=msg*int(m)
            i=j+1
            continue
        else:
            res+=a[i]
            i+=1
    return res
print(unpack(input()))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250313200752380](d:\Users\m1885\Downloads\image-20250313200752380.png)



### 160.相交链表

linked list, https://leetcode.cn/problems/intersection-of-two-linked-lists/



思路：



代码：

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        a=[]
        c=headA
        while c!=None:
            a.append(c)
            c=c.next
        b=[]
        d=headB
        while d!=None:
            b.append(d)
            d=d.next
        i=-1
        while 1:
            if i+len(a)<0 or i+len(b)<0:
                return a[i+1]
            if a[i]!=b[i]:
                if i==-1:
                    return
                else:
                    return a[i+1]
            i-=1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250313191007958](d:\Users\m1885\Downloads\image-20250313191007958.png)



### 206.反转链表

linked list, https://leetcode.cn/problems/reverse-linked-list/



思路：



代码：

```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        a=head
        c=None
        while a!=None:
            c=ListNode(a.val,c)
            a=a.next
        return c
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250313190932701](d:\Users\m1885\Downloads\image-20250313190932701.png)



### 3478.选出和最大的K个元素

heap, https://leetcode.cn/problems/choose-k-elements-with-maximum-sum/



思路：大！败！而！归！

呜呜



代码：

```python
import heapq
class Solution:
    def findMaxSum(self, nums1: List[int], nums2: List[int], k: int) -> List[int]:
        a=sorted([(nums1[i],i)for i in range(len(nums1))])
        b=sorted([(nums2[i],i)for i in range(len(nums2))])
        n=len(nums1)
        ans=[0]*n
        buf=set()
        sum=0
        c=0
        new=set()
        heap=[]
        for i in range(n):
            index=a[i][1]
            buf.add(index)
            for j in new:
                if c<k:
                    heapq.heappush(heap,nums2[j])
                    sum+=nums2[j]
                    c+=1
                elif nums2[j] >heap[0]:
                    sum-=heapq.heappop(heap)
                    sum+=nums2[j]
                    heapq.heappush(heap,nums2[j])
            ans[index]=sum
            new=set()
            if i<n-1 and a[i+1][0]>a[i][0]:
                new=buf
                buf=set()           
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250313205827909](d:\Users\m1885\Downloads\image-20250313205827909.png)



### Q6.交互可视化neural network

https://developers.google.com/machine-learning/crash-course/neural-networks/interactive-exercises

**Your task:** configure a neural network that can separate the orange dots from the blue dots in the diagram, achieving a loss of less than 0.2 on both the training and test data.

**Instructions:**

In the interactive widget:

1. Modify the neural network hyperparameters by experimenting with some of the following config settings:
   - Add or remove hidden layers by clicking the **+** and **-** buttons to the left of the **HIDDEN LAYERS** heading in the network diagram.
   - Add or remove neurons from a hidden layer by clicking the **+** and **-** buttons above a hidden-layer column.
   - Change the learning rate by choosing a new value from the **Learning rate** drop-down above the diagram.
   - Change the activation function by choosing a new value from the **Activation** drop-down above the diagram.
2. Click the Play button above the diagram to train the neural network model using the specified parameters.
3. Observe the visualization of the model fitting the data as training progresses, as well as the **Test loss** and **Training loss** values in the **Output** section.
4. If the model does not achieve loss below 0.2 on the test and training data, click reset, and repeat steps 1–3 with a different set of configuration settings. Repeat this process until you achieve the preferred results.

给出满足约束条件的<mark>截图</mark>，并说明学习到的概念和原理。

看不懂但大受震撼

![image-20250313211918392](d:\Users\m1885\Downloads\image-20250313211918392.png)

## 2. 学习总结和收获

感觉什么都懂了，但是什么都不会











