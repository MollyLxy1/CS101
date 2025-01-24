# Assignment #8: 田忌赛马来了

Updated 1021 GMT+8 Nov 12, 2024

2024 fall, Complied by <mark>李欣妤、地空学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 12558: 岛屿周⻓（15min）

matices, http://cs101.openjudge.cn/practice/12558/ 

思路：想到了老师上课讲的保护圈



代码：

```python
n,m = map(int,input().split())
map = []
length = 0
map.append([0] * (m + 2))
for i in range(n):
    row = [0] + [int(x) for x in input().split()] + [0]
    map.append(row)
map.append([0] * (m + 2))
for i in range(1,n+1):
    for j in range(1,m+1):
        if map[i][j] == 1:
            if map[i - 1][j] == 0:
                length += 1
            if map[i + 1][j] == 0:
                length += 1
            if map[i][j - 1] == 0:
                length += 1
            if map[i][j + 1] == 0:
                length += 1
print(length)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241114115359012](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241114115359012.png)



### LeetCode54.螺旋矩阵（30min）

matrice, https://leetcode.cn/problems/spiral-matrix/

与OJ这个题目一样的 18106: 螺旋矩阵，http://cs101.openjudge.cn/practice/18106

思路：



代码：

```python
n = int(input())
matrix = [[0] * n for _ in range(n)]
num = 1
l, r, u, d = 0, n - 1, 0, n - 1

while num <= n ** 2:
    # 向右
    for i in range(l, r + 1):
        matrix[u][i] = num
        num += 1
    u += 1

    # 向下
    for i in range(u, d + 1):
        matrix[i][r] = num
        num += 1
    r -= 1

    # 向左
    if u <= d:  # 防止向上移动时超出范围
        for i in range(r, l - 1, -1):
            matrix[d][i] = num
            num += 1
        d -= 1

    # 向上
    if l <= r:  # 防止向左移动时超出范围
        for i in range(d, u - 1, -1):
            matrix[i][l] = num
            num += 1
        l += 1
for row in matrix:
    print(' '.join(map(str, row)))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241114200234207](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241114200234207.png)



### 04133:垃圾炸弹（30min）

matrices, http://cs101.openjudge.cn/practice/04133/

思路：在每个垃圾可以被清理的点加上垃圾数量，为了避免遍历所有的点使用了字典



代码：

```python
d = int(input())
n = int(input())
trash = []

for i in range(n):
    trash.append(list(map(int, input().split())))
bomb_map = {}
for x, y, amount in trash:
    for i in range(max(0, x - d), min(1025, x + d + 1)):
        for j in range(max(0, y - d), min(1025, y + d + 1)):
            bomb_map.setdefault((i, j), 0)
            bomb_map[(i, j)] += amount
max_trash = 0
max_points = 0
for value in bomb_map.values():
    if value > max_trash:
        max_trash = value
        max_points = 1
    elif max_trash == value:
        max_points += 1
print(max_points,max_trash)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### LeetCode376.摆动序列（25min）

greedy, dp, https://leetcode.cn/problems/wiggle-subsequence/

与OJ这个题目一样的，26976:摆动序列, http://cs101.openjudge.cn/routine/26976/

思路：一开始把差值放到列表里看相乘是否<0，后来看了ai的答案发现自己的思路被完爆

一个dp表解决不了的那就两个！



代码：

```python
l = int(input())
if l < 2:
    print(l)
else:
    nums = list(map(int, input().split()))
    dp_up = [1] * l
    dp_down = [1] * l

    for i in range(1, l):
        if nums[i] > nums[i - 1]:
            dp_up[i] = dp_down[i - 1] + 1
            dp_down[i] = dp_down[i - 1]
        elif nums[i] < nums[i - 1]:
            dp_down[i] = dp_up[i - 1] + 1
            dp_up[i] = dp_up[i - 1]
        else:
            dp_up[i] = dp_up[i - 1]
            dp_down[i] = dp_down[i - 1]

    result = max(dp_up[-1], dp_down[-1])
    print(result)


```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241115113926919](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241115113926919.png)



### CF455A: Boredom（30min）

dp, 1500, https://codeforces.com/contest/455/problem/A

思路：dp

选i的话前后两个都不能选，所以状态从i+2转过来



代码：

```python
def maxpoints(nums):
    if not nums:
        return 0

    count = [0] * (max(nums) + 1)
    for num in nums:
        count[num] += 1

    dp = [0] * (len(count))
    dp[0] = 0
    dp[1] = count[1]
    for i in range(2, len(count)):
        dp[i] = max(dp[i-1], dp[i-2] + i * count[i])

    return dp[-1]

n = int(input())
nums = list(map(int, input().split()))
print(maxpoints(nums))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241115123135154](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241115123135154.png)



### 02287: Tian Ji -- The Horse Racing（1.5h）

greedy, dfs http://cs101.openjudge.cn/practice/02287

思路：改着改着就和题解一样了（



代码：

```python
def horse_racing(n,tj,qw):
    tj_l,tj_r,qw_l,qw_r = 0,n-1,0,n-1
    win = 0
    while tj_l<=tj_r:
        if tj[tj_l] > qw[qw_l]:
            win += 1
            tj_l += 1
            qw_l += 1
        elif tj[tj_r] > qw[qw_r]:
            win += 1
            tj_r -= 1
            qw_r -= 1
        else:
            if tj[tj_l] <qw[qw_r]:
                win -= 1
            tj_l += 1
            qw_r -= 1

    return win*200
while True:
    n = int(input())
    if n == 0:
        break
    tj = list(map(int, input().split()))
    qw = list(map(int, input().split()))
    tj.sort()
    qw.sort()
    print(horse_racing(n,tj,qw))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241115110649719](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241115110649719.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

像田忌赛马这种有时候不知道自己错在了哪里就会特别难受，另外总是出现index out of range。所以保护圈这种技巧很重要，需要听老师讲。贪心和dp这种算法有时候很难想，还要多练。

最近有在试图跟进每日选做，落下的有点多，慢慢来，体会自己一点一点进步。高数考炸了希望计概拿个好点的分数哈哈



