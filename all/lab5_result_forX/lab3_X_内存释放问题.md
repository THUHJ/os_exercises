2012011294 袁源
2012011272 黄杰
2012011354 杜鹃


目前ucore lab5_answer中，存在潜在的内存泄露现象，请通过设计一个方法来解决这个问题。

Lab5内存泄露？
实现完Lab5要求后，
执行make run-forktest，输出
 assertion failed: nr_free_pages_store == nr_free_pages()
Welcome to the kernel debug monitor!!
发现当fork的进程数max_child超过12时，会出现内存泄露。。。
打印上面两个值，输出如下：
should remain:31861 actually remain:31860
有1页没有被回收


解决方法：
动态计算expected nr_free_pages和actual nr_free_pages的差值（page_tot函数，参数只是为了标注调用这个page_tot的位置，诸如9999，8888本身没有任何意义）
从proc.c init_main着手测试，发现释放过程（即页数的变化）集中在
```
    while (do_wait(0, NULL) == 0) {
        schedule();
    }
```    

发现最后循环结束时最后一步是没有在do_wait的found出来的，也就是没有找到zombie。此时恰好差两页。

发现释放都是do_wait的put_kstack进行的，而pid=1的进程不会经过这一步（此时已经找不到zombie）。
所以只需在init_main的while循环结束后加上对current也就是pid=1的进程的put_kstack。此时通过（测试run-forktree，run-forktest均通过,且make grade成功）。


还有一种解决方法，在每次alloc或者free的时候记录进程号，最后自动判断哪个进程有没有被回收的情况。

