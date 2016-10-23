title: Python实现数据结构——栈，队列
date: 2015-01-20 23:29:55
tags: [Python,数据结构]
---
## 前言
栈和队列是数据结构当中最基础的两个结构，所以作为这个系列的开山第一篇，来确定一下以后文章的结构是怎样，做一个好的框架。打算主要分为**基本定义**，**Python代码实现**，**主要用途**。

## 栈、队列的定义
用来描述栈和队列可以非常简单，先入后出的是栈，先入先出的队列。

## Python代码实现
在Python中使用一维数组来实现栈，使用循环数组来实现队列。
<!-- more -->

```python
class Stack:
    def __init__(self,size):
        self.stack = []
        self.size = size
        self.Top = -1
    def isFull(self):
        if self.Top+1 >= self.size :
            return True
        else:
            return False
    def isEmpty(self):
        if self.Top == -1 :
            return True
        else :
            return False
    def push(self,obj):
        if self.isFull():
            raise Exception( "error,stack is full")
        else:
            self.stack.append(obj)
            self.Top += 1
    def pop(self):
        if self.isEmpty() :
            raise Exception( "error,stack is empty")
        else :
            self.Top -= 1
            return self.stack.pop()
    def peek(self):
        if self.isEmpty() :
            raise Exception( "error,stack is empty")
        else :
            return self.stack[self.Top]
    def show(self):
        print self.stack
```
队列的实现是用循环数组，这样的好处是数组的大小是可以事先固定下来的，但是就是需要多使用一个位置来记录下来是否空和满的情况。

```python
class Queue:
    def __init__(self,x):
        self.size = x
        self.queue = [0]*x
        self.front = 0
        self.rear = 0
    def isEmpty(self):
        if self.front == self.rear:
            return True
        else:
            return False
    def isFull(self):
        if (self.rear+1)%self.size == self.front:
            return True
        else:
            return False
    def add(self,obj):
        if self.isFull():
            raise Exception("error,queue is full")
        else:
            self.rear = (self.rear+1)%self.size
            self.queue[self.rear] = obj
    def delete(self):
        if self.isEmpty():
            raise Exception("error,queue is empty")
        else:
            tem = self.queue[self.front]
            self.front = (self.front+1)%self.size
            return tem
    def peek(self):
        if self.isEmpty():
            raise Exception("error,queue is empty")
        else:
            return self.queue[self.front+1]
    def show(self):
        if self.isEmpty():
            raise Exception("error,queue is empty")
        else:
            a = self.front
            b = self.rear
            while (a != b):
                print self.queue[a+1],
                a = (a+1)%self.size
 ```
 
 ## 主要用途
1.栈和队列都可以用在一些递归当中。不过一般的深度递归，都可以直接在函数中调用，当然程序底层是类似做了个栈的行为。在广度递归当中，就需要首先定义一个队列，然后再进行递归了，队列在递归中的用途会更广泛一些。
2. 想到再继续补充
