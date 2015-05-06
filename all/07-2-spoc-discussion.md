# 同步互斥(lec 18) spoc 思考题


- 有"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的ucore_code和os_exercises的git repo上。

## 个人思考题

### 基本理解
 - 什么是信号量？它与软件同步方法的区别在什么地方？
 - 什么是自旋锁？它为什么无法按先来先服务方式使用资源？
 - 下面是一种P操作的实现伪码。它能按FIFO顺序进行信号量申请吗？
```
 while (s.count == 0) {  //没有可用资源时，进入挂起状态；
        调用进程进入等待队列s.queue;
        阻塞调用进程;
}
s.count--;              //有可用资源，占用该资源； 
```

> 参考回答： 它的问题是，不能按FIFO进行信号量申请。
> 它的一种出错的情况
```
一个线程A调用P原语时，由于线程B正在使用该信号量而进入阻塞状态；注意，这时value的值为0。
线程B放弃信号量的使用，线程A被唤醒而进入就绪状态，但没有立即进入运行状态；注意，这里value为1。
在线程A处于就绪状态时，处理机正在执行线程C的代码；线程C这时也正好调用P原语访问同一个信号量，并得到使用权。注意，这时value又变回0。
线程A进入运行状态后，重新检查value的值，条件不成立，又一次进入阻塞状态。
至此，线程C比线程A后调用P原语，但线程C比线程A先得到信号量。
```

### 信号量使用

 - 什么是条件同步？如何使用信号量来实现条件同步？
 - 什么是生产者-消费者问题？
 - 为什么在生产者-消费者问题中先申请互斥信息量会导致死锁？

### 管程

 - 管程的组成包括哪几部分？入口队列和条件变量等待队列的作用是什么？
 - 为什么用管程实现的生产者-消费者问题中，可以在进入管程后才判断缓冲区的状态？
 - 请描述管程条件变量的两种释放处理方式的区别是什么？条件判断中while和if是如何影响释放处理中的顺序的？

### 哲学家就餐问题

 - 哲学家就餐问题的方案2和方案3的性能有什么区别？可以进一步提高效率吗？

### 读者-写者问题

 - 在读者-写者问题的读者优先和写者优先在行为上有什么不同？
 - 在读者-写者问题的读者优先实现中优先于读者到达的写者在什么地方等待？
 
## 小组思考题

1. （spoc） 每人用python threading机制用信号量和条件变量两种手段分别实现[47个同步问题](07-2-spoc-pv-problems.md)中的一题。向勇老师的班级从前往后，陈渝老师的班级从后往前。请先理解[]python threading 机制的介绍和实例](https://github.com/chyyuu/ucore_lab/tree/master/related_info/lab7/semaphore_condition)


博物馆-公园问题 Jurassic公园有一个恐龙博物馆和一个花园，有m 个旅客租卫辆车，每辆车仅能乘 一个一旅客。旅客在博物馆逛了一会，然后，排队乘坐旅行车，挡一辆车可用喊飞它载 入一个旅客，再绕花园行驶任意长的时间。若n 辆车都己被旅客乘坐游玩，则想坐车的 旅客需要等待。如果一辆车己经空闲，但没有游玩的旅客了，那么，车辆要等待。试用 信号量和P 、V 操作同步m 个旅客和n 辆车子。

这是一个汇合机制，有两类进程：顾客进程和车辆进程，需要进行汇合、即顾客要坐进车辆后才能游玩，开始时让车辆进程进入等待状态

解答:

var sc1 , sck xc ，mutex : semaphore ;
sck:=kx:=0；
sc1:=n ；mutex : = 1 ;
sharearea ：一个登记车辆被服务乘客信息的共享区；
cobegin
    process 顾客i ( i = 1 , 2 ，… ）
    begin
        P (sc1) ; /*车辆最大数量信号量
        P (mutex) ; /*封锁共享区，互斥操作
        在共享区sharearea登记被服务的顾客的信息：
        起始和到达地点，行驶时间
        P(xc); /* 待游玩结束之后，顾客等待下车
        V(sc1) ; /*空车辆数加1
    End
    Process 车辆j(j=1,2,3…)
    Begin
        L:P(sck); /*车辆等待有顾客来使用
        在共享区sharearea登记一辆车被使用，并与顾客进程汇合；
        V(mutex); /*这时可开放共享区，让另一顾客雇车
        车辆载着顾客开行到目的地；
        V(xc); /*允许顾客下车
        Goto L;
    End
coend

Condition:
```
#coding=utf-8
import threading  
import random  
import time
class myCondition():

    def __init__(self,num,name):
        self.num = num
        self.name = name;
        self.condition = threading.Condition()
    def acquire(self):
        #print "acquire %d %s\n" % (self.num,self.name),
   
        if self.condition.acquire():
            if self.num>=1:
                self.num=self.num-1
                self.condition.notify()
            else:
                self.condition.wait()
            self.condition.release()
            
  
    def release(self):
         #print "release %d %s\n" % (self.num,self.name),
         if self.condition.acquire():
            self.num=self.num+1
            self.condition.notify()
            self.condition.release()
       
    
car_num = 2;
person_num = 5;

per_sem=myCondition(0,'pre');
car_sem=myCondition(car_num,'car');
mutex_sem =myCondition(1,'mutex');
play_sem = myCondition(0,'play');

class Car(threading.Thread):  
    def __init__(self):  
       threading.Thread.__init__(self)
       
    def run(self):
        while True:
            print "car %s is waiting\n" % self.name,
            per_sem.acquire();
            mutex_sem.release();
            print "car %s is driving\n" % self.name,
            time.sleep(1);
            play_sem.release();

class Person(threading.Thread):  
    def __init__(self):  
       threading.Thread.__init__(self)  
       
    def run(self):
        print "preson %s wait a car!\n" %  self.name,
        car_sem.acquire();
        mutex_sem.acquire();
        #print"??" 
        per_sem.release();
        print "preson %s is playing!\n" %  self.name,
        play_sem.acquire();
        print "preson %s play end\n" %  self.name,
        car_sem.release();
        
if __name__ == "__main__":
    
    for p in range(0, person_num):  
        p = Person()  
        p.start()  

    for c in range(0, car_num):  
        c = Car()  
        c.start() 
        
       
    
```

Semaphore:
```
#coding=utf-8
import threading  
import random  
import time

car_num = 2;
person_num = 5;

per_sem=threading.Semaphore(0);
car_sem=threading.Semaphore(car_num);
mutex_sem =threading.Semaphore(1);
play_sem = threading.Semaphore(0);

class Car(threading.Thread):  
    def __init__(self):  
       threading.Thread.__init__(self)
       
    def run(self):
        while True:
            print "car %s is waiting\n" % self.name,
            per_sem.acquire();
            mutex_sem.release();
            print "car %s is driving\n" % self.name,
            time.sleep(1);
            play_sem.release();

class Person(threading.Thread):  
    def __init__(self):  
       threading.Thread.__init__(self)  
       
    def run(self):
        print "preson %s wait a car!\n" %  self.name,
        car_sem.acquire();
        mutex_sem.acquire();
        per_sem.release();
        print "preson %s is playing!\n" %  self.name,
        play_sem.acquire();
        print "preson %s play end\n" %  self.name,
        car_sem.release();
        
if __name__ == "__main__":
    
    for p in range(0, person_num):  
        p = Person()  
        p.start()  

    for c in range(0, car_num):  
        c = Car()  
        c.start() 
```
       
