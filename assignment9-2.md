# Assignment #9: dfs, bfs, & dp

Updated 2107 GMT+8 Nov 19, 2024

2024 fall, Complied by <mark>李欣妤、地空学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 18160: 最大连通域面积

dfs similar, http://cs101.openjudge.cn/practice/18160

思路：对每一个是W的格子进行dfs取max

中间因为n,m变量没有在函数里卡了一下，让ai给自己的代码加了一点注释（还被自动加了一个主程序入口）



代码：

```python
def dfs(board, visited, x, y):
    # 边界条件检查
    if x < 0 or y < 0 or x >= len(board) or y >= len(board[0]) or board[x][y] == '.' or visited[x][y]:
        return 0
    # 标记当前节点为已访问
    visited[x][y] = True
    # 初始化当前区域面积
    area = 1
    # 检查所有8个方向
    for dx in [-1, 0, 1]:
        for dy in [-1, 0, 1]:
            if dx == 0 and dy == 0:  # 忽略自身位置
                continue
            nx, ny = x + dx, y + dy
            # 对相邻节点进行深度优先搜索
            area += dfs(board, visited, nx, ny)
    return area


def find_max_area(board):
    # 初始化访问矩阵
    visited = [[False for _ in range(len(board[0]))] for _ in range(len(board))]
    max_area = 0
    # 遍历整个棋盘
    for i in range(len(board)):
        for j in range(len(board[0])):
            # 如果当前位置是 'W' 并且未被访问过
            if not visited[i][j] and board[i][j] == 'W':
                # 计算当前连通区域的面积
                current_area = dfs(board, visited, i, j)
                # 更新最大面积
                max_area = max(max_area, current_area)
    return max_area


# 主程序入口
if __name__ == "__main__":
    case_num = int(input())  # 测试案例数量

    for _ in range(case_num):
        n, m = map(int, input().split())  # 读取棋盘大小
        board = []
        for i in range(n):
            board.append(list(input()))  # 读取棋盘布局

        # 计算并输出当前测试案例的最大连通区域面积
        print(find_max_area(board))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241120130744992](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241120130744992.png)



### 19930: 寻宝

bfs, http://cs101.openjudge.cn/practice/19930

思路：bfs实在是没什么思路，就当是学习了



代码：

```python
from collections import deque
def bfs(board, start):
    m, n = len(board), len(board[0])
    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]  
    queue = deque([start])  # 初始化队列，将起始点加入队列
    visited = set([start])  # 初始化已访问集合，将起始点标记为已访问
    steps = 0  # 初始化步数计数器

    while queue:
        l = len(queue)  # 当前层的节点数
        for _ in range(l):
            x, y = queue.popleft()  # 从队列中取出一个节点
            if board[x][y] == 1:  # 如果当前节点是藏宝点
                return steps  # 返回当前步数
            for dx, dy in directions:  # 检查当前节点的四个方向
                nx, ny = x + dx, y + dy  # 计算邻居节点的坐标
                if 0 <= nx < m and 0 <= ny < n and (nx, ny) not in visited and board[nx][ny] != 2:
                    # 如果邻居节点在棋盘范围内，未被访问过，且不是陷阱
                    queue.append((nx, ny))  # 将邻居节点加入队列
                    visited.add((nx, ny))  # 标记邻居节点为已访问
        steps += 1  # 当前层的所有节点处理完毕，步数加1
    return -1  # 队列为空且未找到藏宝点，返回-1表示无法到达

m, n = map(int, input().split())  # 读取输入的行数和列数
board = []
for _ in range(m):
    row = list(map(int, input().split()))  # 读取每一行的棋盘内容
    board.append(row)

start = (0, 0)  # 起始点为左上角 (0, 0)
result = bfs(board, start)  # 调用BFS函数进行搜索

if result == -1:
    print("NO") 
else:
    print(result) 

```



代码运行截图 ==（至少包含有"Accepted"）==





### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### sy316: 矩阵最大权值路径

dfs, https://sunnywhy.com/sfbj/8/1/316

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>







### LeetCode62.不同路径

dp, https://leetcode.cn/problems/unique-paths/

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### sy358: 受到祝福的平方

dfs, dp, https://sunnywhy.com/sfbj/8/3/539

思路：



代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>





