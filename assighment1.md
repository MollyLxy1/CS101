## 1. 题目



### 02733: 判断闰年



http://cs101.openjudge.cn/practice/02733/

思路：将是闰年的年份分区为两种情况，一种是整除4不能整除100的，一种是可以整除400但不能整除3200的，通过if……elif……else实现对闰年的判断

##### 代码

```python
a = int(input())
if a%4==0 and a%400==0 and a%3200 != 0:
    print ("Y")
elif a%4==0 and a%100 != 0:
    print ("Y")
else:
    print ("N")
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240915113344874](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20240915113344874.png)

### 02750: 鸡兔同笼



http://cs101.openjudge.cn/practice/02750/

思路：首先判断奇偶，若为奇数则输出零，若为偶数则最大动物数确定

再判断是否可以均为四只脚的·动物来确定最小动物数

##### 代码

```python
def counting_animals():
    legs = int(input( ))
    if legs % 2 != 0:
        return 0, 0
    elif legs % 4 == 0:
        return legs // 4, legs // 2
    else:
        return (legs // 4) + 1, legs // 2

def main():
    min_count, max_count = counting_animals()
    print(f"{int(min_count)} {int(max_count)}")

main()
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240916102005993](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20240916102005993.png)

### 50A. Domino piling



greedy, math, 800, http://codeforces.com/problemset/problem/50/A

思路：由题意可知，在矩形格子数为奇数时只能放下（x-1）//2张domino，而在矩形格子数为偶数时可以放下x//2张domino

##### 代码



```python
def count_dominos():
    input_string = input()
    m,n = map(int,input_string.split())
    x = m*n
    if x%2==0:
       result_dominos = x//2
       print(int(result_dominos))
    else:
       result_dominos = (x-1)//2
       print(int(result_dominos))
    return result_dominos
count_dominos()
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240915111415805](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20240915111415805.png)



### 1A. Theatre Square



math, 1000, https://codeforces.com/problemset/problem/1/A

思路：对于任意行/列所需的方砖数为边长//方砖边长（向上取整）

则总共所需的方块数为两边所需的方块书相乘

##### 代码



```python
n,m,a = map(int,input().split())
if n%a == 0:
    n_min = n//a
else:
    n_min = n//a+1
if m%a == 0:
    m_min = m//a
else:
    m_min = m//a+1
result = n_min*m_min
print(int(result))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240916103204496](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20240916103204496.png)

### 112A. Petya and Strings



implementation, strings, 1000, http://codeforces.com/problemset/problem/112/A

思路：按题目要求输入两个字符串后，将字符串转换为小写进行比较。

使用if语句判断字符串的大小关系后



##### 代码



```python
str1 = input().strip()
str2 = input().strip()
str1_low = str1.lower()
str2_low = str2.lower()
if str1_low < str2_low:
    print(-1)
elif str1_low > str2_low:
    print(1)
else:
    print(0)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240915110458068](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20240915110458068.png)

### 231A. Team



bruteforce, greedy, 800, http://codeforces.com/problemset/problem/231/A

思路：首先按照题目要求输入数据，然后判断每一道题是否有》=2的人数做出来，通过for循环叠加解题数量，最后输出

##### 代码



```python
def count_solved_problems():
    n = int(input())
    solved_problems = 0
    for i in range(n):
        p,v,t = map(int,input().split())
        if (p+v+t) >= 2:
             solved_problems +=1
    return solved_problems
def main():
    result = count_solved_problems()
    print(result)
main()
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240915110236486](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20240915110236486.png)

## 2. 学习总结和收获

练习了一些codeforces上最简单题目，在通义千问的帮助下学习了一些基本语法

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==