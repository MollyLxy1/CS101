# Assignment #2: 语法练习

Updated 0126 GMT+8 Sep 24, 2024

2024 fall, Complied by ==同学的姓名、院系==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 263A. Beautiful Matrix

https://codeforces.com/problemset/problem/263/A



思路：先找到1在矩阵中的定位，再通过计算其位置与中心的差值的绝对值来确认最小所需步数



##### 代码

```python
#def min_steps(matrix):
    therow = -1
    thecol = -1
    for i in range(5):
        for j in range(5):
            if matrix[i][j] == 1:
                therow = i
                thecol = j
                break
        if therow != -1:
            break
    row_diff = abs(therow-2)
    col_diff = abs(thecol-2)
    result = row_diff + col_diff
    return result
def main():
    matrix = []
    for _ in range(5):
     row = list(map(int, input().split()))
     matrix.append(row)
    print(min_steps(matrix))
    return matrix
main()

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240924173836070](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20240924173836070.png)

### 1328A. Divisibility Problem

https://codeforces.com/problemset/problem/1328/A



思路：一开始尝试使用while循环叠加步数，但在处理较大数据时总是超时，最后只能采取直接计算的数学方法了



##### 代码

```python
def main():
    n = int(input())
    for i in range(n):
     a,b = map(int,input().split())
     print((b-(a%b))%b)
main()

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240924180826172](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20240924180826172.png)



### 427A. Police Recruits

https://codeforces.com/problemset/problem/427/A



思路：基本吧题目描述翻译为python语言就可以完成，但在输入格式的转换方面有一点小坑，wa了几次都是因为没有处理好输入



##### 代码

```python
clients = 0
crimes = 0
events = []
n = int(input())
events_input = input().split()
events = list(map(int, events_input[:n]))
for event in events:
  if event >= 1:
    clients += event
  elif event == -1:
        if clients >= 1:
                clients -=1
        else:
                crimes +=1
print(crimes)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240924230202180](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20240924230202180.png)



### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



思路：这道题一开始自己写的时候思路比较混乱，用了一个特别长的列表来做，导致超时。后来让chatgpt优化之后发现了很简洁的做法（只需要考虑被砍伐的数目的编号的总数目，避免重复，而不需要定义为1然后做减法），也学习了set和tuple的用法



##### 代码

```python
def remaining_trees(L, M, regions):
    cut_trees = set()

    for start, end in regions:
        cut_trees.update(range(start, end + 1))

    return L + 1 - len(cut_trees)


L, M = map(int, input().split())
regions = [tuple(map(int, input().split())) for _ in range(M)]
print(remaining_trees(L, M, regions))


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240925102344185](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20240925102344185.png)



### sy60: 水仙花数II

https://sunnywhy.com/sfbj/3/1/60



思路：在str和int格式之间转换稍微有一些复杂，剩下的将题目描述翻译为python语言就好



##### 代码

```python
a,b = map(int,input().split())
result = []
for i in range(int(a),int(b)+1):
    s = str(i)
    if sum(int(digit)**3 for digit in s) == i:
        result.append(s)
if result:
    print(" ".join(result))
else:
    print("NO")


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240925105845275](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20240925105845275.png)



### 01922: Ride to School

http://cs101.openjudge.cn/practice/01922/



思路：



##### 代码

```python
import math

while True:
    n = int(input())
    if n == 0:
        break

    charley_time = float("inf")

    for _ in range(n):
        v, t = map(int, input().split())
        rider_time = (4.5 / v) * 3600 + t

        if t >= 0:
            charley_time = min(rider_time,charley_time)

    print(math.ceil(charley_time))

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240925115908670](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20240925115908670.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

我是零基础小白，感觉自己计算机思维很烂，在尝试很多遍写出一道题或者尝试一种更简单的代码后会很有成就感。

前面的题还是简单的语法练习，有试着用到一些集合，字典，排序，感觉现在基本语法掌握得更好了，也有在试着简化代码。但是校门口的树那道题有说的四种解法没有搞懂

感觉最后一道题对我来说挺难（自己想歪了）。最开始完全没有思路，试图模拟骑车过程，看了题解才知道其实就是一个骑行时间的比较，像这样的转化思维我还需要培养

总而言之，思路对了就很简单了，重点是自己把实现思路想出来



