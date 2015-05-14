lab5-X 内存泄露发现方法改进

袁源 2012011294
黄杰 2012011272
杜鹃 2012011354

#问题描述
在lab5中 make run-forktest 或者 make run-forktree时，会发现有2个page发生了泄露。

#发现方法
记录每个进程号alloc和free page的过程，存在num_pages数组中。alloc则加n，free则减n
这里一般情况下，alloc和free对应的进程就是current。而需要注意的是do_wait中kfree(proc)是针对proc进程的回收，所以此时free时应该对num_pages[proc->pid]而非num_pages[current->pid]进行操作。

#代码修改
1. proc.c中定义

```
struct proc_struct *page_current;
int num_pages[50];
```
即当前alloc和free对应的进程和每个进程号alloc和free的操作记录（alloc+, free-）。pmm.c中extern这两个变量。


2. do_wait函数中将kfree(proc)以下面方式包围

```
    page_current = proc;
    kfree(proc);
    page_current = current;
```

#实验结果
proc 1 剩下了两个page
这是因为没有将pid=1的进程进行put_kstack();

```
process 1, 2 pages left
process 2, 0 pages left
process 3, 0 pages left
process 4, 0 pages left
process 5, 0 pages left
process 6, 0 pages left
process 7, 0 pages left
process 8, 0 pages left
process 9, 0 pages left
process 10, 0 pages left
process 11, 0 pages left
process 12, 0 pages left
process 13, 0 pages left
process 14, 0 pages left
process 15, 0 pages left
process 16, 0 pages left
process 17, 0 pages left
process 18, 0 pages left
process 19, 0 pages left
process 20, 0 pages left
process 21, 0 pages left
process 22, 0 pages left
process 23, 0 pages left
process 24, 0 pages left
process 25, 0 pages left
process 26, 0 pages left
process 27, 0 pages left
process 28, 0 pages left
process 29, 0 pages left
process 30, 0 pages left
process 31, 0 pages left
process 32, 0 pages left
process 33, 0 pages left
process 34, 0 pages left

```