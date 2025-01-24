# Assignment #10: dp & bfs

Updated 2 GMT+8 Nov 25, 2024

2024 fall, Complied by <mark>李欣妤、地空学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### LuoguP1255 数楼梯（10min）

dp, bfs, https://www.luogu.com.cn/problem/P1255

思路：非常简单dp题，稍微注意一下n = 1的情况



代码：

```python
n = int(input())
dp = [0]*n
dp[0] = 1
if n > 1:
    dp[1] = 2
    for i in range(2, n):
        dp[i] = dp[i-1] + dp[i-2]
print(dp[-1])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241127110431597](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241127110431597.png)



### 27528: 跳台阶（10min）

dp, http://cs101.openjudge.cn/practice/27528/

思路：

数楼梯的变式，用了一个last存储之前所有项的和并且加上一步跳上去的一种

代码：

```python
n = int(input())
dp = [0]*n
dp[0] = 1
last = 1
if n > 1:
    for i in range(1,n):
        dp[i] = last + 1
        last += dp[i]
print(dp[-1])
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241127111942740](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241127111942740.png)



### 474D. Flowers(20min)

dp, https://codeforces.com/problemset/problem/474/D

思路：每次选择吃一朵红花或者吃一组白花



代码：

```python
MOD = 1000000007
t, k = map(int, input().split())
max_n = 100000
dp = [0] * (max_n + 1)
prefix_sum = [0] * (max_n + 1)
dp[0] = 1
for i in range(1, max_n + 1):
    dp[i] = dp[i - 1]
    if i >= k:
        dp[i] += dp[i - k]
    dp[i] %= MOD
for i in range(1, max_n + 1):
    prefix_sum[i] = (prefix_sum[i - 1] + dp[i]) % MOD
 
for i in range(t):
    a, b = map(int, input().split())
    result = (prefix_sum[b] - prefix_sum[a - 1] + MOD) % MOD
    print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241127122649241](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241127122649241.png)



### LeetCode5.最长回文子串(40min)

dp, two pointers, string, https://leetcode.cn/problems/longest-palindromic-substring/

思路：gpt说这样有比二维dp更小的空间复杂度，这个算暴力？



代码：

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        def expand_around_center(left: int, right: int) -> int:
            while left >= 0 and right < len(s) and s[left] == s[right]:
                left -= 1
                right += 1
            return right - left - 1  # 回文长度

        n = len(s)
        if n <= 1:
            return s

        start, max_length = 0, 0
        for i in range(n):
            # 奇数长度的回文
            len1 = expand_around_center(i, i)
            # 偶数长度的回文
            len2 = expand_around_center(i, i + 1)
            # 取最大长度
            max_len = max(len1, len2)
            if max_len > max_length:
                max_length = max_len
                start = i - (max_len - 1) // 2

        return s[start:start + max_length]
            
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241127125507417](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241127125507417.png)





### 12029: 水淹七军（50min）

bfs, dfs, http://cs101.openjudge.cn/practice/12029/

思路：一开始想用visited，后来发现判断水能不能过去是<=,但是判断淹掉基地是<

被输入搞了好久



代码：

```python
import sys
from collections import deque


def is_headquarter_flooded(k, cases):
    results = []

    for case in cases:
        m, n, terrain, (i, j), p, water_points = case
        visited = [[False] * n for _ in range(m)]
        queue = deque()

        for x, y in water_points:
            queue.append((x - 1, y - 1))  # 转换为 0 索引
            visited[x - 1][y - 1] = True  # 标记为已访问

        # 四个方向的移动向量（上、下、左、右）
        directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]

        while queue:
            x, y = queue.popleft()  # 从队列中取出当前点
            for dx, dy in directions:
                nx, ny = x + dx, y + dy
                if 0 <= nx < m and 0 <= ny < n and not visited[nx][ny]:
                    # 水流向低处或相同高度
                    if terrain[nx][ny] <= terrain[x][y]:
                        visited[nx][ny] = True
                        queue.append((nx, ny))

        # 判断司令部是否被淹
        if visited[i - 1][j - 1]:  # 转换为 0 索引
            results.append("Yes")
        else:
            results.append("No")

    return results


# 一次性读取所有输入
data = sys.stdin.read().split()
idx = 0

# 读取测试用例数量
k = int(data[idx])
idx += 1
cases = []

for _ in range(k):
    # 读取矩阵大小
    m, n = map(int, data[idx:idx + 2])
    idx += 2

    # 读取地形矩阵
    terrain = []
    for _ in range(m):
        terrain.append(list(map(int, data[idx:idx + n])))
        idx += n

    # 读取司令部位置
    i, j = map(int, data[idx:idx + 2])
    idx += 2

    # 读取放水点数量
    p = int(data[idx])
    idx += 1

    # 读取所有放水点
    water_points = []
    for _ in range(p):
        x, y = map(int, data[idx:idx + 2])
        idx += 2
        water_points.append((x, y))

    # 保存当前测试用例
    cases.append((m, n, terrain, (i, j), p, water_points))

# 处理所有测试用例并输出结果
results = is_headquarter_flooded(k, cases)
sys.stdout.write("\n".join(results) + "\n")

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241127230109297](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241127230109297.png)



### 02802: 小游戏（40min)

bfs, http://cs101.openjudge.cn/practice/02802/

思路：算是bfs模板题，感觉自己还不够熟练

代码量好大容易出小错误

第一次遇到PE了，希望考试别卡格式



代码：

```python
from collections import deque

def bfs(board, start, end):
    w, h = len(board[0]), len(board)
    visited = set()
    queue = deque([(start[0], start[1], -1, 0)])
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    while queue:
        x, y, prev_dir, segments = queue.popleft()

        if (x, y) == end:
            return segments + 1

        for d, (dx, dy) in enumerate(directions):
            nx, ny = x + dx, y + dy
            while 0 <= ny < w and 0 <= nx < h and board[nx][ny] == ' ':
                if (nx, ny, d) not in visited:
                    visited.add((nx, ny, d))
                    queue.append((nx, ny, d, segments + (1 if prev_dir != d and prev_dir != -1 else 0)))
                nx += dx
                ny += dy

    return None

board_count = 0
while True:
    w, h = map(int, input().split())
    if w == 0 and h == 0:
        break
    board_count += 1
    board = [[' '] * (w + 2)]
    for _ in range(h):
        row = [' '] + list(input()) + [' ']
        board.append(row)
    board.append([' '] * (w + 2))
    print(f"Board #{board_count}:")
    pair_count = 0
    while True:
        x1, y1, x2, y2 = map(int, input().split())
        if x1 == 0 and y1 == 0 and x2 == 0 and y2 == 0:
            break
        pair_count += 1

        board[y1][x1], board[y2][x2] = ' ', ' '
        result = bfs(board, (y1, x1), (y2, x2))
        board[y1][x1], board[y2][x2] = 'X', 'X'

        if result is not None:
            print(f"Pair {pair_count}: {result} segments.")
        else:
            print(f"Pair {pair_count}: impossible.")
    print()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241127141619724](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241127141619724.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

有在练习盲打了

这次最近多练习了一些bfs的变式，bfs和dfs模板都放到cheatsheet。感觉除了代码量太大比较繁琐之外没有很难，每次都copy上一个bfs代码改改拿着用哈哈哈

这次最难的题应该是回文字符串那个，看群里大佬用了manacher算法。不知道老师能不能上课讲一下这种东西

三个dp题都挺简单，还是代码量小的题目写起来舒服点，可以稍微来点难度（求老师轻点



