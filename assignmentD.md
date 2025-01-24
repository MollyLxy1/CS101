# Assignment #D: 十全十美 

Updated 1254 GMT+8 Dec 17, 2024

2024 fall, Complied by <mark>李欣妤、地空学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02692: 假币问题（1h）

brute force, http://cs101.openjudge.cn/practice/02692

思路：只要有even就不可能有假，集合处理可能轻的和可能中的，同时出现在light集合和heavy集合的一定是真的，最后剩下假币。在heavy就是heavy，在light就是light。

（可恶，想思路大概花了三分钟，一开始用的字典后来发现用集合更好。由于对字典的使用不熟悉加上debug，用时特别长。甚至之前都不知道集合的&和|用法，还好考前补上了）



代码：

```python
def find_fake():
    possible_fake = set('ABCDEFGHIJKL')
    light = set()
    heavy = set()

    for i in range(3):
        l, r, result = input().split()
        if result == "even":
            possible_fake -= set(l + r)
        elif result == "up":
            light |= set(r)
            heavy |= set(l)
        elif result == "down":
            light |= set(l)
            heavy |= set(r)
    possible_fake -= (light & heavy)  # 排除那些既可能轻又可能重的硬币
    for coin in possible_fake:
        if coin in heavy:
            print(f"{coin} is the counterfeit coin and it is heavy.")
            break
        elif coin in light:
            print(f"{coin} is the counterfeit coin and it is light.")
            break
n = int(input())
for _ in range(n):
    find_fake()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241218132624727](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241218132624727.png)



### 01088: 滑雪（30min）

dp, dfs similar, http://cs101.openjudge.cn/practice/01088

思路：写到后面才发现我根本没dp，就是一个dfs（正好是第二天c++班的期末考试天花板题啊）

代码：

```python
def ski_length(R, C, heights):
    # 定义四个方向，上下左右
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]

    # 用来存储每个点的最长路径
    dp = [[-1] * C for _ in range(R)]

    # 判断某个位置是否合法
    def is_valid(x, y):
        return 0 <= x < R and 0 <= y < C

    # DFS计算从 (i, j) 点开始的最长滑坡路径
    def dfs(i, j):
        if dp[i][j] != -1:
            return dp[i][j]

        max_length = 1  # 至少当前点是一个路径
        for dx, dy in directions:
            ni, nj = i + dx, j + dy
            if is_valid(ni, nj) and heights[ni][nj] < heights[i][j]:
                max_length = max(max_length, 1 + dfs(ni, nj))

        dp[i][j] = max_length
        return dp[i][j]

    # 计算所有点的最长滑坡路径
    max_ski_length = 0
    for i in range(R):
        for j in range(C):
            max_ski_length = max(max_ski_length, dfs(i, j))

    return max_ski_length


# 输入
R, C = map(int, input().split())
heights = [list(map(int, input().split())) for _ in range(R)]

# 输出最长的滑坡路径长度
print(ski_length(R, C, heights))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241218134942823](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241218134942823.png)



### 25572: 螃蟹采蘑菇（30min）

bfs, dfs, http://cs101.openjudge.cn/practice/25572/

思路：bfs。但有两格



代码：

```python
from collections import deque
DIRECTIONS = [(-1, 0), (1, 0), (0, -1), (0, 1)]
def bfs(maze, n, start):
    queue = deque([start])
    visited = set()
    # 初始状态：将两部分的位置入队
    visited.add(start)
    while queue:
        # 当前小呆的两个身体部分的位置
        (x1, y1), (x2, y2) = queue.popleft()
        # 如果小呆的任何一部分到达蘑菇的目标位置，返回yes
        if maze[x1][y1] == 9 or maze[x2][y2] == 9:
            return True
        for dx, dy in DIRECTIONS:
            nx1, ny1 = x1 + dx, y1 + dy
            nx2, ny2 = x2 + dx, y2 + dy
            # 检查两个部分是否都在有效范围内，并且可以移动到目标位置
            if 0 <= nx1 < n and 0 <= ny1 < n and 0 <= nx2 < n and 0 <= ny2 < n:
                if maze[nx1][ny1] != 1 and maze[nx2][ny2] != 1:
                    new_state = ((nx1, ny1), (nx2, ny2))
                    if new_state not in visited:
                        visited.add(new_state)
                        queue.append(new_state)

    return False


def can_reach_mushroom(n, maze):
    start = None
    for i in range(n):
        for j in range(n):
            if maze[i][j] == 5:
                if j + 1 < n and maze[i][j + 1] == 5:
                    start = ((i, j), (i, j + 1))
                elif i + 1 < n and maze[i + 1][j] == 5:
                    start = ((i, j), (i + 1, j))
                if start:
                    break
        if start:
            break

    if bfs(maze, n, start):
        return "yes"
    else:
        return "no"
n = int(input())
maze = [list(map(int, input().split())) for _ in range(n)]
print(can_reach_mushroom(n, maze))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241219100815828](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241219100815828.png)



### 27373: 最大整数

dp, http://cs101.openjudge.cn/practice/27373/

思路：好难想啊。想了挺久没什么思路，借鉴了题解的思路，用自定义排序稍微简化了一点？



代码：

```python
from functools import cmp_to_key
m = int(input())
n = int(input())
nums = input().split()
def compare(x, y):
    if x + y > y + x:
        return -1
    elif x + y < y + x:
        return 1
    else:
        return 0
nums.sort(key=cmp_to_key(compare),reverse=True)
dp = [[''] * (m + 1) for _ in range(n + 1)]
for k in range(m + 1):
    dp[0][k] = ''
length = [len(num) for num in nums]
for i in range(1, n + 1):
    for j in range(1, m + 1):
        if length[i - 1] > j:
            # 如果当前数字的位数超过剩余的位数，不能选该数字
            dp[i][j] = dp[i - 1][j]
        else:
            # 选择不选当前数字，或者选择拼接当前数字
            option1 = dp[i - 1][j]  # 不选择当前数字
            option2 = nums[i - 1] + dp[i - 1][j - length[i - 1]]  # 选择当前数字并拼接
            dp[i][j] = max(option1, option2, key=lambda x: (len(x), x))  # 选择两者中较大的结果
result = dp[n][m].lstrip('0')
print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241219112330425](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241219112330425.png)



### 02811: 熄灯问题

brute force, http://cs101.openjudge.cn/practice/02811

思路：差点一个个枚举了，还好有提示，也是用上深拷贝了，感谢ai帮我debug



代码：

```python
from copy import deepcopy
def press_button(matrix, row, col):
    for r, c in [(row, col), (row - 1, col), (row + 1, col), (row, col - 1), (row, col + 1)]:
        if 0 <= r < 5 and 0 <= c < 6:
            matrix[r][c] = 1 - matrix[r][c]
def solve_lights_out(matrix):
    original_matrix = deepcopy(matrix)
    # 枚举第一行按钮的按下或不按下的64种方法
    for i in range(64):  # 0到63的数字，表示64种方法
        cur = deepcopy(original_matrix)
        operate = [[0] * 6 for _ in range(5)]
        # 应用第一行按钮的状态
        states = [int(x) for x in f"{i:06b}"]
        for j, s in enumerate(states):
            if s == 1:
                press_button(cur, 0, j)
                operate[0][j] = 1
        # 根据当前状态逐行消除
        for i in range(1, 5):
            for j in range(6):
                if cur[i - 1][j] == 1:
                    press_button(cur, i, j)
                    operate[i][j] = 1
        # 检查最后一行是否有亮着的灯
        if all(1 not in row for row in cur):
            return operate
    return None

matrix = [list(map(int, input().split())) for _ in range(5)]
solution = solve_lights_out(matrix)
if solution is not None:
    for line in solution:
        print(" ".join(str(s) for s in line))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### 08210: 河中跳房子

binary search, greedy, http://cs101.openjudge.cn/practice/08210/

思路：标了greedy就还是想挑战下，不出意外的TLE了。题解很厉害，我想不到。



代码：

```python
L, n, m = map(int, input().split())
rock = [0]
for i in range(n):
    rock.append(int(input()))
rock.append(L)
def check(x):
    num = 0
    now = 0
    for i in range(1, n + 2):
        if rock[i] - now < x:
            num += 1
        else:
            now = rock[i]

    if num > m:
        return True
    else:
        return False
lo, hi = 0, L + 1
ans = -1
while lo < hi:
    mid = (lo + hi) // 2

    if check(mid):
        hi = mid
    else:
        ans = mid
        lo = mid + 1
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241220172718302](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241220172718302.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

  思维量大，题解很精妙。现在的难点不是把题目转化成代码了，是要想到精妙的算法，再顺便做成题解。这一个学期下来感觉学到了很多，一步一步从语法到算法，到独立写出来一道难题的成就感。

​	最无助的是，我的脑力好像到此为止了，接下来是大佬的领域，我只能学习，再学习。我现在在做的就是多看一点，多写一点，熟练一点，希望期末考试能达到平均水平吧。

​																				此致，
​																				敬礼！



