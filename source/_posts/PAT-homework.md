title: PAT-数据结构
date: 2015-01-21 09:15:36
tags: [算法]
---
在[PAT](http://www.patest.cn/)上做题缘起于参加了网易云课程的[数据结构](http://www.icourse163.org/learn/zju-93001#/learn/announce),一开始的时候是为了学习Java顺便补补数据结构，现在呢，还是为了宝贵的时间，用Python作为主力吧。（惊奇的发现，同样的方法，Python的运算时间竟然更短，说好的Java更具有效率呢，或许是跟Java的Scanner有关吧，或许吧。。。。）

## PAT 01-1 最大子列和问题
### 最暴力的做法
直接做两个for循环，对于每一个固定的a[i]为开端的子序列做一个遍历，然后比较子序列和。这样做思路是最暴力简单的，但是也是带来了许多重复计算的问题，其算法复杂度为O(n^2)结果这种方法也是没有AC。
<!-- more -->

``` python
def normal(num_list):
    max_len_list = -1
    for i in range(len(num_list)):
        tem = num_list[i]
        max_len_list = max(tem,max_len_list)
        for j in range(i+1 , len(num_list)):
            tem += num_list[j]
            max_len_list = max(max_len_list,tem)
    if max_len_list < 0:
        return 0
    else:
        return max_len_list

num_input = raw_input()
line = raw_input()
num_list = []
for tem in line.split():
    num_list.append(int(tem))
print normal(num_list)
```

测试点|结果|用时(ms)|内存(kB)|得分/满分
------- | -------- | ----- |---- | --- |
0 |答案正确 |9 |5111|4/4
1 |答案正确 |10 |5112|4/4
2 |答案正确 |96|5236|4/4
3 |运行超时 |	|	|0/4
4 |运行超时 |	|	|0/4

### 在线比较方法
所谓的在线，就是指每输入一个数据，就更新现在的最优值，也就是说只需要遍历一次所有的数据，就能够得出所有的结构，所以这是一个非常好的算法，其算法复杂度为O(n)。

**关键点在于**

1. 用一个数记录下当前子序列和的值，如果新增的数据使得当前的子序列的和小于零，则表示这一段子序列无论如何也用不上，所以抛弃之，使它的值更新为零。如果新增的数据使得子序列的和还是大于零，这个新增的数加入子序列中，继续操作。
2. 用一个数记录每次更新的子序列的值，保存每次的最大值。

``` python
def oneline(num_list):
    max_len_list = 0
    tem = 0;
    for i in range(len(num_list)):
        tem += num_list[i]
        max_len_list = max(tem,max_len_list)
        if tem < 0:
            tem = 0
    return max_len_list

num_input = raw_input()
line = raw_input()
num_list = []
for tem in line.split():
    num_list.append(int(tem))
print oneline(num_list)
```

测试点|结果|用时(ms)|内存(kB)|得分/满分
--- | --- | --- | ---  | --- |
0|答案正确|9|3408|4/4
1 |答案正确 	|9 |	5112 	|4/4
2 |答案正确 |	10 |	3240 |	4/4
3 |答案正确 |17 	|3624 |	4/4
4 |答案正确 |84 	|8784 |	4/4

## PAT01-2 最大子序列和（变形）
这道题是01-1的变形，只是在输出最大子序列和的基础上，再输出这个子序列头尾的数字。基本思路还是在线处理方法，但是用多几个数去存储计算最大子序列和时候头尾的坐标，所以也是很容易实现的。

这道题有两个坑需要注意的：第四个测试点是需要计算当所有的数字都为负数时，输出0和这个数组的头尾两个数。第五个测试点当中有一部分数为零，其他均为负数，这时候输出的结果应该是0，0，0。注意到这两个坑之后，程序就很简单实现了。

``` python
def oneline(num_list):
    tem = 0 #临时存储最大子序列和
    begin = 0#记录当前子序列的开端
    end = -1#记录当前子序列的结尾
    max_len_list = -1 #不设置为零的原因在于需要针对测试点5的情况：有一部分为零，其他均为负数
    max_begin  = 0
    max_end = -1
    for i in range(len(num_list)):
        tem += num_list[i]
        end += 1
        if (max_len_list < tem):
            max_len_list = tem
            max_begin = begin
            max_end = end
        if tem < 0:
            tem = 0
            begin = i+1
            end = i
    if max_len_list < 0:
        print 0,num_list[0],num_list[-1]
    else:
        print max_len_list,num_list[max_begin],num_list[max_end]

num_input = raw_input()
line = raw_input()
num_list = []
for tem in line.split():
    num_list.append(int(tem))
oneline(num_list)
```

测试点 |	结果 |	用时(ms)| 	内存(kB)| 	得分/满分
----| ---|---|---|---|
0 |	答案正确 |	9 	|4192| 	2/2
1 |	答案正确 |	10 |	4300 |	1/1
2 |	答案正确 |	9 |	2876 |	3/3
3 |	答案正确 |	8 |	2576 |	4/4
4 |	答案正确 |	9 |	2568 |	4/4
5 |	答案正确 |	9 |	2616 |	3/3
6 |	答案正确 |	9 |	4508 |	3/3
7 |	答案正确 |	16 |	4144| 	5/5

## PAT02-2 一元多项式求导
非常简单的一道题，用Python实现的优势就凸显出来了，处理起来很快。用数组来实现链表的功能。就不多解释了。需要注意的是求导出来是零时候输出应该是0 0。所以在程序中对这种情况额外判断即可。

``` python
line = raw_input()
num_list = line.split()
a = 0
b = 1
flag = 0
while (a < len(num_list)):
    j = int(num_list[b])-1
    i = int(num_list[a])* int(num_list[b])
    a += 2
    b += 2
    if i != 0:
        print i,j,
        flag = 1
if flag == 0:
    print 0,0
```

测试点 |	结果 |	用时(ms)| 	内存(kB)| 	得分/满分
----| ---| ---| ---| ---|
0 	|答案正确 |	9 |	5112 |	12/12
1 	|答案正确 |	9 |	5112 	|4/4
2 	|答案正确 |	9 |	5108 |	3/3
3 	|答案正确 |	9 |	5112 	|3/3
4 	|答案正确 |	9 |	5112 	|3/3

## PAT02-3 求前缀表达式的值
这道题主要是用到了栈。Python中的数组本身就具有许多栈的操作，a=[],a.pop(),a.append(obj)。也让我这种懒人方便了很多。

主要步骤如下：

1. 读取一个字符串，压入栈中。
2. 读取第二个字符串，压入栈中，如果这个字符串是数字，则弹出三个在栈中的数据，进行计算，将计算结果压入栈中。然后剩下的就是递归。

当然在实际编写的过程当中并没有简单。感觉自己编写的程序还是过于人类的思考方式，而不是计算机的思考方式。编程路上还是任重道远啊。

``` python
import string

def is_digit(string):
    if string == "*" or string == "/" or string == "+" or string == "-":
        return False
    else:
        return True

def cal(num1,string,num2):
    if string == "*":
        return num1*num2
    elif string == "/":
        return num1/num2
    elif string == "+":
        return num1+num2
    elif string == "-":
        return num1-num2
    else:
        raise Exception("ERROR")

list = raw_input().split()
stack = []
for elem in list:
    try:
        if is_digit(elem):
            tem = string.atof(elem)
            stack.append(tem)
            while is_digit(stack[-2]) :
                num2 = stack.pop()
                num1 = stack.pop()
                calculator = stack.pop()
                stack.append(cal(num1,calculator,num2))
        else:
            stack.append(elem)
    except:
        if len(stack) == 1:
            print round(stack[0],1)
        else:
            print "ERROR"
        break
```
### **Python 小技巧**：

 1. ****string.atof****是个很实用的函数，可以将带符号的字符串转为数字。例如“-1”“+1”，都可以识别出来。在编写的时候有一段时间是用了isdigit(str)这个函数，但是无法识别符号是个硬伤。
 2. **round(float,n)**保留n个小数点
 3. 在Python中print是默认有回车的，如果想不要回车，那么在print后加“，”就可以实现，例如："print a,b，”
 4. **try,except**是实用的函数。主要是用来处理异常信息，也可以让自己在编程当中偷懒不少，不过就是要是编出来的程序答案不对，那么就需要自己去找异常信息了。
 
 
 ## PAT03-1 二分法求多项式单根
 
 写得一点都不优雅，还是将就着吧。
 
 ``` python
 import string

def result(a3,a2,a1,a0,x):
    return a3*pow(x,3)+a2*pow(x,2)+a1*x+a0

line1 = raw_input()
line2 = raw_input()
a = line1.split()
x = line2.split()
a3 = string.atof(a[0])
a2 = string.atof(a[1])
a1 = string.atof(a[2])
a0 = string.atof(a[3])
x1 = string.atof(x[0])
x2 = string.atof(x[1])

left_result = result(a3,a2,a1,a0,x1)
right_result = result(a3,a2,a1,a0,x2)


while (abs(x1-x2)> pow(10,-3)):
    mid = (x1+x2)/2
    if result(a3,a2,a1,a0,mid) * result(a3,a2,a1,a0,x2) < 0:
        x1 = mid
    elif result(a3,a2,a1,a0,mid) * result(a3,a2,a1,a0,x1) < 0:
        x2 = mid
    else:
        break
if abs(result(a3,a2,a1,a0,x1)) < pow(10,-4):
    print "%.2f"%x1
elif abs(result(a3,a2,a1,a0,x2)) < pow(10,-4):
    print "%.2f"%x2
else:
    print "%.2f"%((x1+x2)/2)
```