---
title: python-threading介绍
date: 2018-04-03 11:01:00
tags: [python, threading]
categories: 技术借鉴
---

# 线程模块

> * threading.currentThread()：返回当前的线程变量
> * threading.enumerate()：返回一个包含正在运行的线程list。正在运行线程指启动后、结束前，不包括启动前和终止后的线程。
> * threading.activeCount()：返回正在运行的线程数量与len(threading.enumerate())有相同的结果。
> * run()：用以表示线程活动的方法。
> * start()：启动线程活动。
> * join([time])：等待至线程中止。这阻塞调用线程直至线程的join()方法被调用中止-正常退出或未处理的异常，或者是可选的超时发生。
> * isAlive()：返回线程是否活动。
> * getName()：返回线程名。
> * setName()：设置线程名。

<!-----------------more----------------------->

# 使用threading模块创建线程

## 步骤

> 1. 从threading.Thread继承创建新的子类：class myThread(threading.Thread)
> 2. 在子类中重写run()方法，可将线程执行内容通过方法再细分：def run(self)
> 3. 实例化新的线程：thread1 = myThread(参数)
> 4. 启动新的线程： thread1.start()
> 5. 等待线程中止： thread1.join()

```
"""
直接从threading.Thread继承创建一个新的子类，并实例化后调用start()方法启动新线程，即它调用了线程的run()方法。
"""

# 导入需要用到的模块
import threading
import time

# 退出标志
exitFlag = 0

# 从threading.Thread继承创建新的子类
class myThread(threading.Thread):
    #构造函数，初始化变量
    def __init__(self, threadID, name, counter):
        threading.Thread.__init__(self)
        self.threadId = threadID
        self.name = name
        self.counter = counter

    # 调用start()方法的时候启动
    def run(self):
        print ("开始线程：" + self.name)
        # 调用线程执行方法
        print_time(self.name, self.counter, 5)
        print ("退出线程：" + self.name)

# 线程方法的具体实现
def print_time(threadName, delay, counter):
    while counter:
        if exitFlag:
            threadName.exit()
        time.sleep(delay)
        print ("%s %s" % (threadName, time.ctime(time.time())))
        counter -= 1

if __name__ == "__main__":
    # 创建两个线程
    thread1 = myThread(1, "Thread-1", 1)
    thread2 = myThread(2, "THread-2", 2)

    # 线程开始
    thread1.start()
    thread2.start()
    # 等待线程中止
    thread1.join()
    thread2.join()
    print("退出线程")
```

# 线程同步
如果多个线程共同对某个数据修改，则可能出现不可预料的结果，为了保证数据的正确性，需要对多个线程进行同步。
使用Thread对象的Lock和Rlock可以实现简单的线程同步，这两个对象都有acquire和release方法，对于那些需要每次只允许一个线程操作的数据，可以将其操作放到acquire和release方法之间。
多线程的优势在于可以同时运行多个任务（至少感觉起来是这样）。但是当线程需要共享数据的时候，可能存在数据不同步的问题。为了避免这种情况，引入了锁的概念。
锁有两种状态--锁定和未锁定。每当一个线程（set）要访问共享数据时，必须先获得锁定；如果已经有别的线程（print）获得锁定了，那么就让线程（set）暂停，也就是同步阻塞，等到线程（print）访问完毕，释放锁以后，再让线程（set）继续。

## 步骤
> 除在重写方法run()中要多一步获取锁和释放锁以外跟上面的步骤一样。

```
import threading
import time

class myThread(threading.Thread):
    def __init__(self, threadID, name, counter):
        threading.Thread.__init__(self)
        self.threadID = threadID
        self.name = name
        self.counter = counter

    def run(self):
        print ("开启线程：" + self.name)
        # 获取锁，用于线程同步
        threadLock.acquire()
        print_time(self.name, self.counter, 5)
        # 释放锁，开启下一个线程
        threadLock.release()

def print_time(threadName, delay, counter):
    while counter:
        time.sleep(delay)
        print ("%s %s" % (threadName, time.ctime(time.time())))
        counter -= 1

threadLock = threading.Lock()
threads = []

thread1 = myThread(1, "Thread-1", 1)
thread2 = myThread(2, "Thread-2", 2)

thread1.start()
thread2.start()

threads.append(thread1)
threads.append(thread2)

for t in threads:
    t.join()
print ("退出线程")
```

# 申明

本文参考菜鸟教程——[Python3 多线程](http://www.runoob.com/python3/python3-multithreading.html)