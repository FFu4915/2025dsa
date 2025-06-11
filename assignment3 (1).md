# Assignment #3: 惊蛰 Mock Exam

Updated 1641 GMT+8 Mar 5, 2025

2025 spring, Complied by <mark>付麟瑞 数院</mark>



> **说明：**
>
> 1. **惊蛰⽉考**：sorry有事未能参加，下次一定<mark>（请改为同学的通过数）</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
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

### E04015: 邮箱验证

strings, http://cs101.openjudge.cn/practice/04015



思路：



代码：

```python
import sys

def is_valid_email(email):
    if email.count('@') != 1:
        return "NO"
    
    if email[0] in {'@', '.'} or email[-1] in {'@', '.'}:
        return "NO"
    
    at_index = email.index('@')
    dot_index = email.rfind('.')
    
    if dot_index < at_index + 2:
        return "NO"
    
    if "@." in email or ".@" in email:
        return "NO"
    
    return "YES"

if __name__ == "__main__":
    for line in sys.stdin:
        email = line.strip()
        print(is_valid_email(email))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

去年计概的时候各种漏条件，和chatgpt一起修改，WA16次大败而归（笑）

![image-20250309113759735](d:\Users\m1885\Downloads\image-20250309113759735.png)

### M02039: 反反复复

implementation, http://cs101.openjudge.cn/practice/02039/



思路：



代码：

```python
n=int(input())
msg=input()
m=len(msg)//n
mtx=[]
ori_msg=''
for i in range(m):
    if i%2==0:
        mtx.append(list(msg[i*n:i*n+n]))
    else:
        a=list(msg[i*n:i*n+n])
        a.reverse()
        mtx.append(a)
for j in range(n):
    for i in range(m):
        ori_msg+=mtx[i][j]
print(ori_msg)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![image-20250309114914912](d:\Users\m1885\Downloads\image-20250309114914912.png)

### M02092: Grandpa is Famous

implementation, http://cs101.openjudge.cn/practice/02092/



思路：



代码：

```python
while 1:
    dp=[0]*10001
    n,m=map(int,input().split())
    if m==0 and n==0:
        break
    for i in range(n):
        a=list(map(int,input().split()))
        for j in a:
            dp[j]+=1
    first=(dp[1],1)
    seconds=[]
    lz_tag=[]
    for i in range(2,10001):
        if dp[i]>first[0]:
            seconds = [first]
            if lz_tag :
                for j in lz_tag:
                    seconds.append((first[0],j))
                lz_tag = []
            first=(dp[i],i)
        elif dp[i]==first[0]:
            lz_tag.append(i)
        else:
            if not seconds:
                seconds.append((dp[i],i))
            else:
                if seconds[0][0]<dp[i]:
                    seconds=[(dp[i],i)]
                elif seconds[0][0]==dp[i]:
                    seconds.append((dp[i],i))
    a=[]
    for i in seconds:
        a.append(i[1])
    print(' '.join(map(str,a)))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250309121447202](d:\Users\m1885\Downloads\image-20250309121447202.png)



### M04133: 垃圾炸弹

matrices, http://cs101.openjudge.cn/practice/04133/



思路：陈年老题，当时没想到要对每个垃圾在他能被炸到的点count+=1



代码：

```python
d = int(input())  # 输入偏移量
matrix = [[0] * 1025 for _ in range(1025)]  # 初始化矩阵
n = int(input())  # 输入数据点个数

for _ in range(n):
    x, y, t = map(int, input().split())  # 输入每个数据点的坐标和加值
    
    
    # 更新矩阵区域，添加边界检查
    for i in range(max(0, x - d), min(1024, x + d) + 1):
        for j in range(max(0, y - d), min(1024, y + d) + 1):
            matrix[i][j] += t

# 查找最大值并计算其出现次数
maxcount = 0
count = 1
for i in range(1025):
    for j in range(1025):
        if matrix[i][j] > maxcount:
            maxcount = matrix[i][j]
            count = 1
        elif matrix[i][j] == maxcount:
            count += 1

print(count, maxcount)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250309114000148](d:\Users\m1885\Downloads\image-20250309114000148.png)



### T02488: A Knight's Journey

backtracking, http://cs101.openjudge.cn/practice/02488/



思路：经典dfs，但是限制字典序最小，所以moves要按顺序排列



代码：

```python
moves=[[-2,-1],[-2,1],[-1,-2],[-1,2],[1,-2],[1,2],[2,-1],[2,1]]
def dfs(m,n,visited,path,x,y,c):
    if c==m*n:
        return path
    for i in moves:
        dx,dy=i[0]+x,i[1]+y
        a=None
        if 0<= dx<m and 0<= dy<n and (dx,dy) not in visited:
            a=dfs(m,n,visited.union({(dx,dy)}),path+[(dx,dy)],dx,dy,c+1)
        if a!=None:
            return a
    return None
cases=int(input())
for _ in range(cases):
    n,m=map(int,input().split())
    ans = 'impossible'
    for i in range(m):
        for j in range(n):
            res=dfs(m,n,{(0,0)},[(0,0)],0,0,1)
            if res is not None:
                path=''
                for k in res:
                    path+=chr(k[0]+65)
                    path+=str(k[1]+1)
                ans=path
                break
    if _ :
        print('')
    print(f'Scenario #{_+1}:')
    print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250309130212378](d:\Users\m1885\Downloads\image-20250309130212378.png)



### T06648: Sequence

heap, http://cs101.openjudge.cn/practice/06648/



思路：看上去感觉要dp，每行加入之后取n个最小值，减小时间复杂度用heap，由于n比较大会爆内存，所以有n//(w+1)的优化，大概把b从n^2缩到nlogn了，然后就是想办法优化边缘条件找bug（其实有些地方不用改，但是由于不知道哪里出了问题，就放得很宽以避免边缘情况出事故）



代码：

```python
import heapq
t=int(input())
for _ in range(t):
    m,n=map(int,input().split())
    mins=[]
    for i in range(m):
        if mins==[]:
            mins=list(map(int,input().split()))
            mins.sort()
        else:
            a=list(map(int,input().split()))
            a.sort()
            b=[]
            heapq.heapify(b)
            for w in range(n):
                for e in range(min(n//(w+1)+1,n)):
                    heapq.heappush(b,a[w]+mins[e])
            mins=heapq.nsmallest(n,b)
    print(*mins)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20250309150747404](d:\Users\m1885\Downloads\image-20250309150747404.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

在学了在学了（









