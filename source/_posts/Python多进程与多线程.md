---
title: Python多进程与多线程
date: 2021-06-24 12:40:25
tags:
---

# 一、多进程
## 1、多进程与多任务

### 1.1 进程的创建步骤

- 导入进程包

	import multiprocessing

- 通过进程类创建进程对象
	进程对象 = multiprocessing.Process()
	
- 启动进程执行任务
	进程对象.start()

### 1.2 通过进程类创建进程对象

进程对象 = multiprocessing.Process(target=任务名)

| 参数名   | 说明  |
|  ----  | ----  |
| target  | 执行的目标任务名，这里指的是函数名（方法名）|
| name  | 进程名，一般不用设置 |
| group	| 进程组，目前只能使用None|

### 1.3 进程创建与启动的代码

```python
import multiprocessing
import time


def print1():
    while True:
        time.sleep(3)
        print('11111')


def print2():
    while True:
        time.sleep(3)
        print('22222')


if __name__ == '__main__':
    sing_process1 = multiprocessing.Process(target=print1)
    sing_process2 = multiprocessing.Process(target=print2)
    sing_process1.start()
    sing_process2.start()

```
## 2、多进程与多任务
### 2.1 进程执行带有参数的任务
|参数名|说明|
|---|---|
|args| 以元组的方式给执行任务传参|
|kwargs| 以字典方式给执行任务传参|

### 2.2 args参数的使用

```python
def print1(s):
    while True:
        time.sleep(3)
        print(s)

if __name__ == '__main__':
	# target: 进程执行的函数名
    # args: 表示以元组的方式给函数传参
    sing_process1 = multiprocessing.Process(target=print1, args=("哈哈哈哈", ))
    sing_process1.start()
```
### 2.3 kwargs参数的使用
```python
def print2(s):
    while True:
        time.sleep(3)
        print(s)

if __name__ == '__main__':
	# target: 进程执行的函数名
    # args: 表示以元组的方式给函数传参
    sing_process2 = multiprocessing.Process(target=print2, kwargs={"s": "我是进程2"})
    sing_process2.start()
```



## 3、获取进程编号 
**进程编号的作用**
当程序中进程的数量越来越多时，如果没有办法区分主进程和子进程还有不同的子进程，那么就无法进行有效的进程管理，为了方便管理实际上每个进程都是有自己编号的。
### 3.1 获取当前进程的编号和父进程的编号：
```python
def work(s):
    while True:
        time.sleep(3)
        # 当前进程的编号（os.getpid()）
        print("p1-id:", os.getpid())
        # 父进程的编号 （os.getppid()）
        print("p1-pppp", os.getppid())
```

## 4 、进程的注意点

### 4.1 主进程默认会等待所有的子进程执行结束再结束

![image-20210624135941184](D:\data\projects\gitPro\ZhuBlogHexo\source\images\questions\image-202106241321.png)

想要主进程结束，子进程也跟着结束，需要设置守护主进程
```python
import multiprocessing
import time


def work():
    while True:
        time.sleep(1)
        print("子进程正在工作中")


if __name__ == '__main__':
    m_work = multiprocessing.Process(target=work)
    # 设置守护
    m_work.daemon = True

    m_work.start()
    time.sleep(3)
    print("主进程结束了")
```
## 5、多进程实现文件夹多任务白拷贝器

# 二、多线程