# Assignment #C: 五味杂陈 

Updated 1148 GMT+8 Dec 10, 2024

2024 fall, Complied by <mark>李欣妤、地空学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 1115. 取石子游戏（1.5h）

dfs, https://www.acwing.com/problem/content/description/1117/

思路：假设a>b，a = kb+c。如果（b，c）是必胜情况且k>1，那么先手取到(a,b+c)就是必胜，反之(b,c)必输的话，先手取到(c,b)也是必胜。只有在k = 1时先手可能会输，这时候递归考虑



代码：

```python
def can_first_player_win(a, b):
    if a < b:
        a, b = b, a
    if a / b >= 2:
        return True
    elif a % b == 0:
        return True
    else:
        a -= b
        if can_first_player_win(a, b):
            return False
        else:
            return True
while True:
    a, b = map(int, input().split())
    if (a, b) == (0, 0):
        break
    print("win" if can_first_player_win(a, b) else "lose")
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![截屏2024-12-12 21.13.47](/Users/hanfangfang/Desktop/截屏2024-12-12 21.13.47.png)



### 25570: 洋葱（20min）

Matrices, http://cs101.openjudge.cn/practice/25570

思路：懒得费尽心思想什么很多东西了



代码：

```python
import math
n = int(input())
onion = []
for i in range(n):
    onion.append(list(map(int, input().split())))
layers = math.ceil(n/2)
sums = []
def sum_layer(onion, l_u, r_d):
    sum = 0
    for i in range(l_u, r_d + 1):
        sum += onion[l_u][i]  # 上
        sum += onion[r_d][i]  # 下
        sum += onion[i][l_u]  # 左
        sum += onion[i][r_d]  # 右
    # 减去四个角，因为它们被加了两次
    sum -= onion[l_u][l_u]
    sum -= onion[r_d][r_d]
    sum -= onion[r_d][l_u]
    sum -= onion[l_u][r_d]
    if l_u == r_d:
        sum += onion[l_u][r_d]
    return sum

for i in range(layers):
    sums.append(sum_layer(onion, i, n - 1 - i))
print(max(sums))
```



代码运行截图 ==（至少包含有"Accepted"）==

![截屏2024-12-12 21.16.13](/Users/hanfangfang/Library/Application Support/typora-user-images/截屏2024-12-12 21.16.13.png)



### 1526C1. Potions(Easy Version)（30min）

greedy, dp, data structures, brute force, *1500, https://codeforces.com/problemset/problem/1526/C1

思路：如果喝完是正的就直接喝，如果喝完了会变成负的就考虑之前有没有喝过更负的，如果有就替换掉，这样可以保证喝的药水数量单调不减，用heapq来实现。



代码：

```python
import heapq
def max_potions(n, a):
    heap = []
    health = 0
    potions_drunk = 0
 
    for potion in a:
        if health + potion >= 0:
            health += potion
            heapq.heappush(heap, potion)
            potions_drunk += 1
        elif heap and potion > heap[0]:
            health += potion - heapq.heappushpop(heap, potion)
 
    return potions_drunk
 
n = int(input())
a = list(map(int, input().split()))
result = max_potions(n, a)
print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![截屏2024-12-12 21.24.10](/Users/hanfangfang/Desktop/截屏2024-12-12 21.24.10.png)



### 22067: 快速堆猪（20min）

辅助栈，http://cs101.openjudge.cn/practice/22067/

思路：一个栈就会超时，加一个辅助栈



代码：

```python
pigs = []
min_stack = []
while True:
    try:
        command = input().split()
        if "push" in command:
            value = int(command[1])
            pigs.append(value)
            # 如果min_stack为空或者当前值小于等于min_stack的栈顶，推入min_stack
            if not min_stack or value <= min_stack[-1]:
                min_stack.append(value)
        if "pop" in command:
            if pigs:
                popped_value = pigs.pop()
                # 如果弹出的值是当前最小值，也从min_stack中弹出
                if min_stack and popped_value == min_stack[-1]:
                    min_stack.pop()
        if "min" in command:
            if min_stack:
                print(min_stack[-1])
    except EOFError:
        break
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![截屏2024-12-12 21.26.07](/Users/hanfangfang/Library/Application Support/typora-user-images/截屏2024-12-12 21.26.07.png)



### 20106: 走山路（40min）

Dijkstra, http://cs101.openjudge.cn/practice/20106/

思路：复习Dijkstra



代码：

```python
import heapq
directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
def min_cost_to_travel(m, n, grid, start, end):
    dist = [[float('inf')] * n for _ in range(m)]
    if grid[start[0]][start[1]] == '#' or grid[end[0]][end[1]] == '#':
        return "NO"
    # 使用优先队列存储当前的位置和体力消耗
    pq = []
    heapq.heappush(pq, (0, start[0], start[1]))  # (体力消耗, x, y)
    dist[start[0]][start[1]] = 0

    while pq:
        cost, x, y = heapq.heappop(pq)
        if (x, y) == end:
            return cost

        for dx, dy in directions:
            nx, ny = x + dx, y + dy
            if 0 <= nx < m and 0 <= ny < n and grid[nx][ny] != '#':
                new_cost = cost + abs(int(grid[nx][ny]) - int(grid[x][y])) if grid[nx][ny] != '#' else float('inf')

                # 如果找到更少消耗的路径，更新
                if new_cost < dist[nx][ny]:
                    dist[nx][ny] = new_cost
                    heapq.heappush(pq, (new_cost, nx, ny))

    return "NO"
m, n, p = map(int, input().split())
grid = []
for _ in range(m):
    grid.append(input().split())
for _ in range(p):
    sx, sy, ex, ey = map(int, input().split())
    result = min_cost_to_travel(m, n, grid, (sx, sy), (ex, ey))
    print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![截屏2024-12-12 21.27.52](/Users/hanfangfang/Library/Application Support/typora-user-images/截屏2024-12-12 21.27.52.png)



### 04129: 变换的迷宫（1h）

bfs, http://cs101.openjudge.cn/practice/04129/

思路：三维！



代码：

```python
from collections import deque
def bfs(maze, R, C, K, start, end):
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    # 队列中的元素为 (r, c, t) -> 当前行列位置和当前时间
    queue = deque([(start[0], start[1], 0)])
    visited = [[[False] * K for _ in range(C)] for _ in range(R)]  # 3D数组，visited[r][c][t % K]表示位置(r,c)和时间t的状态
    visited[start[0]][start[1]][0] = True
    while queue:
        r, c, t = queue.popleft()
        if (r, c) == end:
            return t

        for dr, dc in directions:
            nr, nc = r + dr, c + dc

            if 0 <= nr < R and 0 <= nc < C:
                nt = t + 1
                if maze[nr][nc] == '#':
                    if nt % K != 0:  # 如果不是K的倍数，石头无法穿越
                        continue
                if not visited[nr][nc][nt % K]:
                    visited[nr][nc][nt % K] = True
                    queue.append((nr, nc, nt))
    return "Oop!"
n = int(input())
for _ in range(n):
    R, C, K = map(int, input().split())
    maze = [input().strip() for _ in range(R)]
    start = None
    end = None
    for r in range(R):
        for c in range(C):
            if maze[r][c] == 'S':
                start = (r, c)
            elif maze[r][c] == 'E':
                end = (r, c)
    result = bfs(maze, R, C, K, start, end)
    print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![截屏2024-12-12 21.28.38](/Users/hanfangfang/Desktop/截屏2024-12-12 21.28.38.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

确实是五味杂陈啊。potions和走山路两个题是heapq用法加上复习dijkstra，堆猪学一下辅助栈，小石子游戏是博弈论题目，洋葱是矩阵。感觉这次的题目都挺综合的。一开始石头没想清楚卡了挺久，后边的题目写起来还都挺爽的，就是用时太长了，感觉去考试根本做不完几个题目。变幻的迷宫让我第一次尝试了三维的数组。

heapq使用能力++
bfs熟练度++
害怕挂科程度++



