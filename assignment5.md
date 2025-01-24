# Assignment #5: Greedy穷举Implementation

Updated 1939 GMT+8 Oct 21, 2024

2024 fall, Complied by <mark>李欣妤 地空学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 04148: 生理周期

brute force, http://cs101.openjudge.cn/practice/04148

思路：没有用数学公式，直接一遍一遍加到d了，计算机做加法真的好快

利用while判断终止条件还不熟，卡了一下



代码：

```python
counter = 0
while True:
        p, e, i, d = map(int, input().split())
        if [p, e, i, d] == [-1, -1, -1, -1]:
          break
        counter +=1
        p %= 23
        e %= 28
        i %= 33
        origin = d
        d += 1
        while d % 23 != p or d % 28 != e or d % 33 != i:
            d += 1
        print(f'Case {counter}: the next triple peak occurs in {d-origin} days.')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241023141022855](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241023141022855.png)



### 18211: 军备竞赛

greedy, two pointers, http://cs101.openjudge.cn/practice/18211

思路：双指针好用，熟悉了双指针不难



代码：

```python
n = int(input())
weapons = list(map(int, input().split()))
our = 0
enemy = 0
result = 0
idx_l = 0
idx_r = len(weapons) - 1

weapons = sorted(weapons)

while idx_l <= idx_r:
    if n >= weapons[idx_l]:
        n -= weapons[idx_l]
        idx_l += 1
        our += 1
        result = max(result, our - enemy)
    elif enemy<our:
        n += weapons[idx_r]
        idx_r -= 1
        enemy += 1
    else:
        break
print(result)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241023144713737](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241023144713737.png)



### 21554: 排队做实验

greedy, http://cs101.openjudge.cn/practice/21554

思路：差点忘了数组这回事，还是要多练（求助了一下ai



代码：

```python
def calculate_average_wait_time(n, times):
    indexed_times = sorted((t, i + 1) for i, t in enumerate(times))
    total_wait_time, current_wait_time = 0, 0
    order = []

    for duration, index in indexed_times:
        order.append(index)
        total_wait_time += current_wait_time
        current_wait_time += duration
    
    return order, total_wait_time / n
n = int(input().strip())
times = list(map(int, input().strip().split()))
order, avg_wait_time = calculate_average_wait_time(n, times)
print(" ".join(map(str, order)))
print(f"{avg_wait_time:.2f}")
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241023234225847](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241023234225847.png)





### 01008: Maya Calendar

implementation, http://cs101.openjudge.cn/practice/01008/

思路：如何处理输入的字符串这部分还有点不熟

还好提前看到了同学的提醒记得输出n



代码：

```python
n = int(input())
result = []
maya_dict = {"pop": 0, "no": 20, "zip": 40, "zotz": 60, "tzec": 80, "xul": 100, "yoxkin": 120, "mol": 140, "chen": 160,
             "yax": 180, "zac": 200, "ceh": 220, "mac": 240, "kankin": 260, "muan": 280, "pax": 300, "koyab": 320,
             "cumhu": 340, "uayet": 360}
tzolkin_names = ['imix', 'ik', 'akbal', 'kan', 'chicchan', 'cimi', 'manik', 'lamat', 'muluk', 'ok', 'chuen', 'eb',
                 'ben', 'ix', 'mem', 'cib', 'caban', 'eznab', 'canac', 'ahau']

for _ in range(n):
    date = input().strip().split(".")
    day = int(date[0])
    month_year = date[1].strip().split()
    month = month_year[0]
    year = int(month_year[1])
    total_days = 365 * year + day + maya_dict[month]

    tzolkin_day_number = (total_days % 13) + 1
    tzolkin_day_name = tzolkin_names[total_days % 20]
    tzolkin_year = total_days // 260

    result.append(f"{tzolkin_day_number} {tzolkin_day_name} {tzolkin_year}")

print(n)
for res in result:
    print(res)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241023234159145](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241023234159145.png)



### 545C. Woodcutters

dp, greedy, 1500, https://codeforces.com/problemset/problem/545/C

思路：1500的题目并没有想象中的难，思路很顺畅就想出来了，大概多练真的有用

利用一个position标记哪里有树



代码：

```python
n = int(input())
trees = [tuple(map(int, input().split())) for _ in range(n)]
 
position = float('-inf')
result = 0
 
for i in range(n):
    x, h = trees[i]
 
    if x - h > position:
        result += 1
        position = x
    elif i == n-1 or x + h < trees[i + 1][0]:
        result += 1
        position = x + h
    else:
        position = x
 
print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241024113329059](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241024113329059.png)



### 01328: Radar Installation

greedy, http://cs101.openjudge.cn/practice/01328/

思路：把每一个岛需要雷达所在范围做成一个表，每次往最右侧加一个雷达

while语句判断的输入又卡了一下，还是要更熟练一些



代码：

```python
import math
import sys
def find_min(islands, d):
    # 按照雷达的右端点（可以覆盖的最远点）排序
    intervals = []
    for x, y in islands:
        if y > d:  # 如果岛的 y 坐标超出雷达的覆盖范围，返回 -1
            return -1
        # 计算每个岛屿的可覆盖区间
        dx = math.sqrt(d ** 2 - y ** 2)
        intervals.append((x - dx, x + dx))
    intervals.sort(key=lambda interval: interval[1])

    radars = 0
    current_coverage = -float('inf')

    for interval in intervals:
        if interval[0] > current_coverage:
            # 需要一个新的雷达，将雷达放在当前区间的右端点
            current_coverage = interval[1]
            radars += 1
    return radars
case_number = 1
while True:
    line = input().strip()
    if line == "0 0":
        break
    n, d = map(int, line.split())
    islands = []
    for _ in range(n):
        x, y = map(int, input().split())
        islands.append((x, y))
    sys.stdin.readline()
    result = find_min(islands, d)
    print(f"Case {case_number}: {result}")
    case_number += 1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241024113524511](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241024113524511.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

  作业难度整体还是逐渐上升的趋势吧。现在贪心题目都能基本有个思路了，甚至比月考那一会儿要顺手很多，更喜欢学编程序了。不过有时候自己的思路会不够好或者有漏洞。写作业的时候可以通过ai纠正，但还需要通过练习让自己思维更严谨，要能看出来自己的代码什么地方有问题。

每日选做没能完全跟进，但是做了的部分确实收获很大，接下来准备期中考试可能不能保持练习量了，计划考完期中后加大练习量。

有些奇怪的输入输出要求需要注意，比如输出n，或者测试数据用空行隔开。也是在作业题里补上了语法知识的一些空缺，比如sys.stdin.readline()

接下来主要学习矩阵了



