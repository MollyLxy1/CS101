# Assign #3: Oct Mock Exam暨选做题目满百

Updated 1537 GMT+8 Oct 10, 2024

2024 fall, Complied 李欣妤_地空学院



**说明：**

1）Oct⽉考： AC6 == 4 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++/C（已经在Codeforces/Openjudge上AC），截图（包含Accepted, 学号），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、作业评论有md或者doc。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E28674:《黑神话：悟空》之加密

http://cs101.openjudge.cn/practice/28674/



思路：当时没有想到使用ascii码来做这个题目，自己手打了一个字母表，不过也较快做出来了这个题



代码

```python
def anti_denuvo(miwen,n):
    low_alphabet = ["a","b","c","d","e","f","g","h","i","j","k","l","m","n","o","p","q","r","s","t","u","v","w","x","y"]
    up_alphabet = []
    mingwen = ""
    for string in low_alphabet :
        up_alphabet.append(string.upper())
    for letter in miwen:
        for i in range (0, 26):
            if letter == low_alphabet[i]:
                letter = low_alphabet[i-n]
                mingwen += letter
                break
            elif letter == up_alphabet[i]:
                letter = up_alphabet[i-n]
                mingwen += letter
                break
    print(mingwen)
a = int(input())
n = a%26
miwen = input()
anti_denuvo(miwen,n)

```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20241013155902607](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241013155902607.png)



### E28691: 字符串中的整数求和

http://cs101.openjudge.cn/practice/28691/



思路：感觉是很基础的语法题



代码

```python
a,b = map(str,input().split())
inta = a[0] + a[1]
intb = b[0] + b[1]
print(int(inta) + int(intb))

```



代码运行截图 ==（至少包含有"Accepted"）==![image-20241013155953735](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241013155953735.png)





### M28664: 验证身份证号

http://cs101.openjudge.cn/practice/28664/



思路：用到了比较多的列表，感觉思路比较容易想，顺着题目描述来就好了



代码

```python
xishu = [7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2]
haoma = ['1','0','X','9','8','7','6','5','4','3','2']

n = int(input())
def is_sfzh(sfzh):
    jym = 0
    for i in range(0,17):
        jym += int(sfzh[i])*xishu[i]
    jym = jym%11
    if sfzh[17] == haoma[jym]:
        print("YES")
    else:
        print("NO")
for i in range(n):
    sfzh = input()
    is_sfzh(sfzh)

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==![image-20241013160131934](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241013160131934.png)





### M28678: 角谷猜想

http://cs101.openjudge.cn/practice/28678/



思路：语法题，很顺手



代码

```python
n = int(input())
while n != 1:
    if n%2 == 0:
        print(str(n)+"/2="+str(n//2))
        n = n//2
    else:
        print(str(n)+"*3+1="+str(n*3+1))
        n = n*3+1
if n == 1:
    print("End"

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==![image-20241013160147846](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241013160147846.png)





### M28700: 罗马数字与整数的转换

http://cs101.openjudge.cn/practice/28700/



思路：一开始以为罗马数字转换为整数更困难，就先做了，结果后面的整数向罗马数字的把自己卡住了，后面才想到用列表的这种对应



##### 代码

```python

roman = ['I','V','X','L','C','D','M']
roman_trans = [1,5,10,50,100,500,1000]
arabic = ['1','2','3','4','5','6','7','8','9','0']
number = input()
target_arb = 0
if number[0] in roman :
    for _ in range(len(number)):
        for i in range(0,7):
            if number[_] ==roman[i]:
                target_arb += roman_trans[i]
                break
    if "IX" in number :
        target_arb -= 2
    if "IV" in number :
        target_arb -= 2
    if "XL" in number :
        target_arb -= 20
    if "XC" in number :
        target_arb -= 20
    if "CD" in number :
        target_arb -= 200
    if "CM" in number :
        target_arb -= 200
    print(target_arb)
def arabic_to_roman(number):
    val = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1]
    syms = ["M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"]
    roman_num = ""
    for i in range(len(val)):
        while number >= val[i]:
            roman_num += syms[i]
            number -= val[i]

    return roman_num
if number[0] in arabic:
    number = int(number)
    print(arabic_to_roman(number))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==![image-20241013160534677](C:\Users\Molly\AppData\Roaming\Typora\typora-user-images\image-20241013160534677.png)





### *T25353: 排队 （选做）

http://cs101.openjudge.cn/practice/25353/



思路：



代码

```python


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

  感觉自己的脑子已经跟不上计概的课程了，考试的时候题目都是用很笨的方法做出来的。排队这个题目自己尝试了很多次最终从WA变成TLE，也优化不出一点。最后崩溃去看题解，花了挺长时间来理解题解的思路，但自己想肯定是想不出来。

每日选做题大概还有十几题没做，一般难题或者涉及到新思路的题目都做不出来，真的很崩溃，不知道做题能不能对现状有所改变，可能自己就是没有数学/计算机思维吧











