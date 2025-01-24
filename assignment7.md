# Assignment #7: Nov Mock Exam立冬

Updated 1646 GMT+8 Nov 7, 2024

2024 fall, Complied by <mark>李欣妤、地空学院</mark>



**说明：**

1）⽉考： AC5<mark>（请改为同学的通过数）</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E07618: 病人排队（15min）

sorttings, http://cs101.openjudge.cn/practice/07618/

思路：内置排序很稳定



代码：

```python
n = int(input())
old = []
young = []
for i in range (n):
    id,anni = input().split()
    anni = int(anni)
    if anni >= 60:
        old.append([id,anni])
    else:
        young.append([id,anni])
old = sorted(old, key = lambda x: x[1], reverse = True)
for p in old:
    print(p[0])
for p in young:
    print(p[0])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241108090810755](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241108090810755.png)



### E23555: 节省存储的矩阵乘法（30min）

implementation, matrices, http://cs101.openjudge.cn/practice/23555/

思路：记得不能用浅拷贝



代码：

```python
n, m1, m2 = map(int, input().split())
ma1 = [[0] * n for _ in range(n)]
ma2 = [[0] * n for _ in range(n)]
target = [[0] * n for _ in range(n)]
results = []
for _ in range(m1):
    i,j,a = map(int, input().split())
    ma1[i][j] = a
for _ in range(m2):
    i,j,b = map(int, input().split())
    ma2[i][j] = b
for i in range(n):
    for j in range(n):
        for k in range(n):
            target[i][j] += ma1[i][k] * ma2[k][j]
for i in range(n):
    for j in range(n):
        if target[i][j] != 0:
            results.append([i, j, target[i][j]])
for result in results:
    print(' '.join(map(str, result)))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241108090904220](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241108090904220.png)



### M18182: 打怪兽 （20min）

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/

思路：

试着用了一个初始化字典，很好用

代码：

```python
def Q(cases):
    for n, m, b, skills in cases:
        t_map = {}
        for ti, xi in skills:
            t_map.setdefault(ti, []).append(xi)

        for t in sorted(t_map):
            d = sum(sorted(t_map[t], reverse=True)[:m])
            b -= d
            if b <= 0:
                result = t
                break
        else:
            result = "alive"
        print (result )

num = int(input())

for _ in range(num):
    cases = []
    n, m, b = map(int, input().split())
    skills = [tuple(map(int, input().split())) for _ in range(n)]
    cases.append((n, m, b, skills))
    Q(cases)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241108091100593](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241108091100593.png)



### M28780: 零钱兑换3（15min）

dp, http://cs101.openjudge.cn/practice/28780/

思路：dp模板



代码：

```python
n, m = map(int, input().split())
coins = list(map(int, input().split()))

dp = [float('inf')] * (m + 1)
dp[0] = 0
for num in coins:
    for i in range(num, m + 1):
        dp[i] = min(dp[i], dp[i - num] + 1)
print(dp[m] if dp[m] != float('inf') else -1)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241108091346146](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241108091346146.png)



### T12757: 阿尔法星人翻译官（40min）

implementation, http://cs101.openjudge.cn/practice/12757

思路：million 和thousand分开，写一下999以内



代码：

```python
dic = {
    'zero': 0, 'one': 1, 'two': 2, 'three': 3, 'four': 4, 'five': 5,
    'six': 6, 'seven': 7, 'eight': 8, 'nine': 9, 'ten': 10, 'eleven': 11,
    'twelve': 12, 'thirteen': 13, 'fourteen': 14, 'fifteen': 15, 'sixteen': 16,
    'seventeen': 17, 'eighteen': 18, 'nineteen': 19, 'twenty': 20, 'thirty': 30,
    'forty': 40, 'fifty': 50, 'sixty': 60, 'seventy': 70, 'eighty': 80, 'ninety': 90,
    'hundred': 100, 'thousand': 1000, 'million': 1000000
}

def count(words):
    units = [1000000, 1000, 100]
    result = 0
    current = 0
    for word in words:
        n = dic[word]
        if n == 100:
            current *= n
        elif n >= 1000:
            result += current * n
            current = 0
        else:
            current += n
    return result + current

def transfer(s):
    value = 0
    negative = False
    if s[0] == 'negative':
        negative = True
        s = s[1:]
    value = count(s)
    if negative:
        return -value
    else:
        return value

alpha = input()
s = alpha.split()
print(transfer(s))

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241108174657156](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241108174657156.png)



### T16528: 充实的寒假生活（25min）

greedy/dp, cs10117 Final Exam, http://cs101.openjudge.cn/practice/16528/

思路：一开始按照每个项目的耗时来排序失败了，试着用截止时间排序了，看来我的贪心不一定是正确的贪心



代码：

```python
n = int(input())
activities = []
for i in range(n):
    start, end = map(int, input().split())
    activities.append((start, end))

activities.sort(key=lambda x: x[1])
result = 0
last_end = -1
for start, end in activities:
    if start > last_end:
        result += 1
        last_end = end
print(result)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20241108091909298](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241108091909298.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>
（感谢老师推荐的typora，现在已经被我拿来记笔记了)

小白能AC5已经很满意了。考试的时候感觉第五题比较繁琐就放了，果然课后也花了比较长时间才完全搞定（看到大佬1h ak非常震惊）感觉这次的题目没有下狠手，思路都不难想，平时的作业也有类似的。我自己的问题就是写东西太慢了，debug能力很弱，有时候思路错了更正要花很长时间。希望以后多练能熟能生巧。



