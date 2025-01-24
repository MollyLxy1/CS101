# Assignment #B: Dec Mock Exam大雪前一天

Updated 1649 GMT+8 Dec 5, 2024

2024 fall, Complied by <mark>李欣妤、地空学院</mark>



**说明：**

1）⽉考： ==AC3== 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E22548: 机智的股民老张（15min）

http://cs101.openjudge.cn/practice/22548/

思路：虽然是月考里最简单的题，还是想了一会儿，要看清楚如何更新。

最低买入可以随时更新，最高获利只能在最低买入后面更新。



代码：

```python
prices = list(map(int,input().split()))
l = len(prices)
hp = 0
lp = float("inf")
for i in range(l):
    lp = min(lp,prices[i])
    hp = max(hp,prices[i]-lp)
print(hp)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![截屏2024-12-10 17.29.22](/Users/hanfangfang/Desktop/截屏2024-12-10 17.29.22.png)



### M28701: 炸鸡排（15min）

greedy, http://cs101.openjudge.cn/practice/28701/

思路：考场时没想这个题目，写作业的时候很快想到了思路，可能是因为在看群里消息的时候看到了同学清晰的思路，不知道是不是失去了一次思考的机会哈哈



代码：

```python
n,k = map(int,input().split())
times = list(map(int,input().split()))
times = sorted(times)
s = sum(times)
while True:
    if times[-1]<=(s/k):
        print(f"{s/k:.3f}")
        break
    else:
        s-=times.pop()
        k-=1
```



代码运行截图 ==（至少包含有"Accepted"）==

![截屏2024-12-10 17.31.23](/Users/hanfangfang/Library/Application Support/typora-user-images/截屏2024-12-10 17.31.23.png)



### M20744: 土豪购物（50min）

dp, http://cs101.openjudge.cn/practice/20744/

思路：最开始试图用dp更新丢掉最小数值直接减去，后来发现可能一个东西丢掉两遍。就抱着尝试的心态试了一下遍历列表，尝试扔掉每一个然后超时。求助一下ai最后改成了提前处理左侧和右侧的和，才过了。做法比较暴力

（ai加上了注释）

代码：

```python
def max_value(nums):
    n = len(nums)  # 获取输入数组的长度

    # 计算不放回商品的最大子数组和 (Kadane算法)
    def no_remove_max(arr):
        max_sum = curr_sum = arr[0]  # 初始化最大和和当前和为数组的第一个元素
        for i in range(1, len(arr)):  # 从第二个元素开始遍历
            curr_sum = max(arr[i], curr_sum + arr[i])  # 选择当前元素和当前和相加或只选择当前元素
            max_sum = max(max_sum, curr_sum)  # 更新最大和
        return max_sum  # 返回最大子数组和

    # 计算不放回商品的最大子数组和
    max_no_remove = no_remove_max(nums)

    # 初始化两个数组，用来存储从左到右和从右到左的最大子数组和
    left_max = [0] * n  # left_max[i]表示从第0个元素到第i个元素的最大子数组和
    right_max = [0] * n  # right_max[i]表示从第i个元素到最后一个元素的最大子数组和

    # 填充left_max数组
    left_max[0] = nums[0]  # 第一个位置的最大和就是它本身
    for i in range(1, n):
        left_max[i] = max(nums[i], left_max[i - 1] + nums[i])  # 当前元素与前面部分的和比较

    # 填充right_max数组
    right_max[n - 1] = nums[n - 1]  # 最后一个位置的最大和就是它本身
    for i in range(n - 2, -1, -1):  # 从倒数第二个元素开始往前遍历
        right_max[i] = max(nums[i], right_max[i + 1] + nums[i])  # 当前元素与后面部分的和比较

    # 计算放回一个商品后的最大值
    max_with_remove = float('-inf')  # 初始化放回一个商品后的最大值为负无穷
    for i in range(1, n - 1):  # 遍历数组中间的元素（不包括第一个和最后一个）
        max_with_remove = max(max_with_remove, left_max[i - 1] + right_max[i + 1])  # 将第i个元素移除，计算左部分和右部分的最大和

    # 处理特殊情况：如果放回第一个商品或最后一个商品
    max_with_remove = max(max_with_remove, right_max[1])  # 放回第一个商品
    max_with_remove = max(max_with_remove, left_max[n - 2])  # 放回最后一个商品

    # 返回最终结果：不放回商品的最大和和放回商品的最大和中的较大值
    return max(max_no_remove, max_with_remove)

# 输入并输出结果
nums = list(map(int, input().split(',')))  # 输入一个由逗号分隔的整数列表
print(max_value(nums))  # 输出结果


```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![截屏2024-12-10 17.44.19](/Users/hanfangfang/Desktop/截屏2024-12-10 17.44.19.png)



### T25561: 2022决战双十一

brute force, dfs, http://cs101.openjudge.cn/practice/25561/

思路：最可怕的是思路不难但是不知道如何实现，学习了product生成交叉列表



代码：

```python
from itertools import product
def calculate_min_cost(n, m, prices, coupons):
    # 将优惠券解析为每个店铺的可用券列表
    shop_coupons = []
    for coupon_str in coupons:
        shop_coupons.append([tuple(map(int, c.split('-'))) for c in coupon_str.split()])

    # 枚举所有购买方案
    min_cost = float("inf")
    for selection in product(*[list(range(len(prices[i]))) for i in range(n)]):
        # 计算每家店铺的标价总和
        shop_totals = [0] * m
        for item_idx, shop_idx in enumerate(selection):
            shop_id, price = prices[item_idx][shop_idx]
            shop_totals[shop_id - 1] += price

        # 应用店铺优惠券
        original_totals = []
        discounted_totals = []
        for shop_idx, total in enumerate(shop_totals):
            best_discount = 0
            for q, x in shop_coupons[shop_idx]:
                if total >= q:
                    best_discount = max(best_discount, x)
            original_totals.append(total)
            discounted_totals.append(total - best_discount)

        # 计算跨店满减后的总价
        total_price = sum(discounted_totals)
        total_price -= (sum(original_totals) // 300) * 50

        # 更新最小成本
        min_cost = min(min_cost, total_price)

    return min_cost
n, m = map(int, input().split())
prices = []
for i in range(n):
    pairs = input().split()
    item_prices = []
    for pair in pairs:
        shop, price = map(int, pair.split(':'))
        item_prices.append((shop, price))
    prices.append(item_prices)

# 读取每个店铺的优惠券
coupons = []
for j in range(m):
    coupon = input()
    coupons.append(coupon)
result = calculate_min_cost(n, m, prices, coupons)
print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![截屏2024-12-10 17.46.24](/Users/hanfangfang/Library/Application Support/typora-user-images/截屏2024-12-10 17.46.24.png)



### T20741: 两座孤岛最短距离

dfs, bfs, http://cs101.openjudge.cn/practice/20741/

思路：dfs，bfs模版相加

一开始dfs找到一个岛的全部，再bfs找到到另一个岛的最小距离，超时了所以改成从第一个岛的边界出发，好烦琐的题目

数据输入有点小坑，元素没有空格分开，我只好当作字符串处理



代码：

```python
from collections import deque

matrix = []
n = int(input())
for i in range(n):
    s = input()
    matrix.append(list(s))  
rows = len(matrix)
cols = len(matrix[0])
def dfs(matrix, x, y, visited):
    visited.add((x, y))
    island = [(x, y)]
    boundary = []  
    stack = [(x, y)]
    while stack:
        cx, cy = stack.pop()
        for dx, dy in [(-1, 0), (0, 1), (1, 0), (0, -1)]:
            nx, ny = cx + dx, cy + dy
            if 0 <= nx < rows and 0 <= ny < cols and (nx, ny) not in visited:
                if matrix[nx][ny] == '1':
                    visited.add((nx, ny))
                    island.append((nx, ny))
                    stack.append((nx, ny))
                elif matrix[nx][ny] == '0':
                    boundary.append((cx, cy)) 
    return boundary
def bfs(matrix, boundary, visited):
    queue = deque([(x, y, 0) for x, y in boundary])  
    for x, y in boundary:
        visited.add((x, y))

    while queue:
        cx, cy, dist = queue.popleft()
        for dx, dy in [(-1, 0), (0, 1), (1, 0), (0, -1)]:
            nx, ny = cx + dx, cy + dy
            if 0 <= nx < rows and 0 <= ny < cols and (nx, ny) not in visited:
                if matrix[nx][ny] == '1': 
                    return dist
                visited.add((nx, ny))
                queue.append((nx, ny, dist + 1))
visited = set()
start_x, start_y = -1, -1
for x in range(rows):
    for y in range(cols):
        if matrix[x][y] == '1':
            start_x, start_y = x, y
            break
    if start_x != -1:
        break

boundary = dfs(matrix, start_x, start_y, visited)
result = bfs(matrix, boundary, visited)
print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![截屏2024-12-10 17.49.00](/Users/hanfangfang/Desktop/截屏2024-12-10 17.49.00.png)



### T28776: 国王游戏

greedy, http://cs101.openjudge.cn/practice/28776

思路：

也是考场没看。greedy难题果然是想到了就很好实现啊。

代码：

```python
n = int(input())
ministers = []
king_l,king_r = map(int,input().split())
a = king_l
for i in range(n):
    l,r = map(int,input().split())
    ministers.append((l,r))
ministers.sort(key=lambda x: (x[0] * x[1]))
ans = 0
for i in range(n):
    ans = max(ans, a//ministers[i][1])
    a*=ministers[i][0]
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![截屏2024-12-10 17.52.27](/Users/hanfangfang/Desktop/截屏2024-12-10 17.52.27.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

好难啊！很妙的思路想不到，思路好想的题又烦琐，写什么题都很慢。上周还觉得dfs和bfs模版都算是学会了，结果碰到双十一我都看不出来怎么dfs。计划在考前狠狠锻炼一下greedy和dp，给自己的思维上点强度看看能不能有点题感。理科思维不好的人要死掉了。



