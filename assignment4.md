# Assignment #4: T-primes + 贪心

Updated 0337 GMT+8 Oct 15, 2024

2024 fall, Complied by <mark>李欣妤 地空学院</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 34B. Sale

greedy, sorting, 900, https://codeforces.com/problemset/problem/34/B



思路：将价格递增排序，依次购买电视，取最终结果为购买电视所得的最大值来排除电视价格为正后的影响



代码

```python
n,m= map(int,input().split())
tvsets = list(map(int,input().split()))
tvsets = sorted(tvsets)
sum = 0
for i in range(0,m):
    current = sum-int(tvsets[i])
    sum = max(sum,current)
print(sum)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241017111550772](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241017111550772.png)





### 160A. Twins

greedy, sortings, 900, https://codeforces.com/problemset/problem/160/A

思路：一个一个拿最大的硬币，直到超分



代码

```python
n = int(input())
coins = list(map(int,input().split()))
coins.sort(reverse=True)
sumc = 0
result = 0
for coin in coins:
        sumc += coin
        result += 1
        if sumc > sum(coins) - sumc:
            break
print(result)


```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241017115303085](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241017115303085.png)





### 1879B. Chips on the Board

constructive algorithms, greedy, 900, https://codeforces.com/problemset/problem/1879/B

思路：一开始没看懂题目想干什么，看测试数据猜测题目的意思，竟然猜对了哈哈



代码

```python
n = int(input())
for i in range(n):
    a = int(input())
    row_list = list(map(int, input().split()))
    col_list = list(map(int, input().split()))
    sr = sum(row_list)
    sc = sum(col_list)
    result = min((sr + min(col_list)*a),sc + min(row_list)*a)
    print(result)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241017165900559](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241017165900559.png)



### 158B. Taxi

*special problem, greedy, implementation, 1100, https://codeforces.com/problemset/problem/158/B

思路：

不知道这样做算不算greedy，感觉就是讨论了一下不同的情况

代码

```python
import math
n = int(input())
count = 0
students = list(map(int, input().split()))
ones = students.count(1)
twos = students.count(2)
threes = students.count(3)
fours = students.count(4)
count += fours
count += threes
ones = max(0,ones - threes)
count += math.ceil((ones + 2*twos)/4)
print(count)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241017173913846](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241017173913846.png)



### *230B. T-primes（选做）

binary search, implementation, math, number theory, 1300, http://codeforces.com/problemset/problem/230/B

思路：跟gpt学了一个埃氏筛，也算是提前处理数据的一种吧



代码

```python
import math
 
def sieve_of_eratosthenes(limit):
    primes = [True] * (limit + 1)
    p = 2
    while p * p <= limit:
        if primes[p]:
            for i in range(p * p, limit + 1, p):
                primes[i] = False
        p += 1
    return {p for p in range(2, limit + 1) if primes[p]}
 
def t_primes(n, prime_set):
    sqrt_n = math.isqrt(n)
    if sqrt_n * sqrt_n == n and sqrt_n in prime_set:
        print("YES")
    else:
        print("NO")
 
max_input = int(input())
prime_set = sieve_of_eratosthenes(10**6)
ns = list(map(int, input().split()))
for n in ns:
    t_primes(n, prime_set)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241017104643862](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241017104643862.png)



### *12559: 最大最小整数 （选做）

greedy, strings, sortings, http://cs101.openjudge.cn/practice/12559

思路：最开始自己写的东西很混乱，没找到明确的思路，跟ai学了lambda的用法，得到了简单好看的代码，还有这个挺妙的x*4



代码

```python
# 读取输入
n = int(input())
numbers = input().split()

# 对数组进行排序以构造最大整数
max_number = ''.join(sorted(numbers, key=lambda x: x * 4, reverse=True))
# 对数组进行排序以构造最小整数
min_number = ''.join(sorted(numbers, key=lambda x: x * 4))

# 输出结果
print(max_number, min_number)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241017195257286](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241017195257286.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

  在cf上的每日选做写完了（因为wa可以知道是哪里错了所以更喜欢写cf的题，也可能oj上的题目要难一些没有全做完），感觉这次的作业题前四道都比每日选做简单得多，也比较轻松解决了。T-primes自己做的tle，最大最小整数弄了挺多if else 处理字符串，感谢ai教我埃氏筛和key = lambda x的用法，记在了cheetsheet上，决定以后碰到这种还不会用的新东西就先记在上面。

  虽然只是部分跟进每日选做，但还是感觉自己水平确实有在一点点提高，希望这种进步不要停滞。有个小小的问题就是虽然题目写出来了但是并没有感觉自己哪里用到了贪心算法？（哈哈哈



