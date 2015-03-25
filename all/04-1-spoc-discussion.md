#lec8 虚拟内存spoc练习


NOTICE
- 有"w4l2"标记的题是助教要提交到学堂在线上的。
- 有"w4l2"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。


## 个人思考题

### 内存访问局部性的应用程序例子
---
(1)(w4l2)下面是一个体现内存访问局部性好的简单应用程序例子，请参考，在linux中写一个简单应用程序，体现内存局部性差，并给出其执行时间。
```
#include <stdio.h>
#define NUM 1024
#define COUNT 10
int A[NUM][NUM];
void main (void) {
  int i,j,k;
  for (k = 0; k<COUNT; k++)
  for (i = 0; i < NUM; i++)
  for (j = 0; j	 < NUM; j++)
      A[i][j] = i+j;
  printf("%d count computing over!\n",i*j*k);
}
```
可以用下的命令来编译和运行此程序：
```
gcc -O0 -o goodlocality goodlocality.c
time ./goodlocality
```
可以看到其执行时间。
调换ij位置得到badlocality.c
```
#include <stdio.h>
#define NUM 1024
#define COUNT 10
int A[NUM][NUM];
void main (void) {
  int i,j,k;
  for (k = 0; k<COUNT; k++)
  for (i = 0; i < NUM; i++)
  for (j = 0; j	 < NUM; j++)
      A[j][i] = i+j;
  printf("%d count computing over!\n",i*j*k);
}
```
多次测量后得到两个程序的运行时间：
goodlocality    
10485760 count computing over!  
real 0m0.187s  
user 0m0.073s  
sys  0m0.010s  
badlovality  
10485760 count computing over!  
real 0m0.407s  
user 0m0.235s  
sys  0m0.037s  
可以看出利用程序的空间局部性，能够减少程序运行的时间。

## 小组思考题目
----

### 缺页异常嵌套

（1）缺页异常可用于虚拟内存管理中。如果在中断服务例程中进行缺页异常的处理时，再次出现缺页异常，这时计算机系统（软件或硬件）会如何处理？请给出你的合理设计和解释。

### 缺页中断次数计算
（2）如果80386机器的一条机器指令(指字长4个字节)，其功能是把一个32位字的数据装入寄存器，指令本身包含了要装入的字所在的32位地址。这个过程最多会引起几次缺页中断？

### 虚拟页式存储的地址转换

（3）(spoc) 有一台假想的计算机，页大小（page size）为32 Bytes，支持8KB的虚拟地址空间（virtual address space）,有4KB的物理内存空间（physical memory），采用二级页表，一个页目录项（page directory entry ，PDE）大小为1 Byte,一个页表项（page-table entries
PTEs）大小为1 Byte，1个页目录表大小为32 Bytes，1个页表大小为32 Bytes。页目录基址寄存器（page directory base register，PDBR）保存了页目录表的物理地址（按页对齐）。

PTE格式（8 bit） :
```
  VALID | PFN6 ... PFN0
```
PDE格式（8 bit） :
```
  VALID | PT6 ... PT0
```
其
```
VALID==1表示，表示映射存在；VALID==0表示，表示内存映射不存在（有两种情况：a.对应的物理页帧swap out在硬盘上；b.既没有在内存中，页没有在硬盘上）。
PFN6..0:页帧号或外存中的后备页号
PT6..0:页表的物理基址>>5
```

已经建立好了1个页目录表和8个页表，且页目录表的index为0~7的页目录项分别对应了这8个页表。

在[物理内存模拟数据文件](./04-1-spoc-memdiskdata.md)中，给出了4KB物理内存空间和4KBdisk空间的值，PDBR的值。

请回答下列虚地址是否有合法对应的物理内存，请给出对应的pde index, pde contents, pte index, pte contents，the value of addr in phy page OR disk sector。
```
Virtual Address 6653:
Virtual Address 1c13:
Virtual Address 6890:
Virtual Address 0af6:
Virtual Address 1e6f:
```

回答可参考以如下表示：
```
Virtual Address 7570:
  --> pde index:0x1d  pde contents:(valid 1, pfn 0x33)
    --> pte index:0xb  pte contents:(valid 0, pfn 0x7f)
      --> Fault (page table entry not valid)
      
Virtual Address 21e1:
  --> pde index:0x8  pde contents:(valid 0, pfn 0x7f)
      --> Fault (page directory entry not valid)

Virtual Address 7268:
  --> pde index:0x1c  pde contents:(valid 1, pfn 0x5e)
    --> pte index:0x13  pte contents:(valid 1, pfn 0x65)
      --> Translates to Physical Address 0xca8 --> Value: 16

Virtual Address 106f:
  --> pde index:0x3  pde contents:(valid 1, pfn 0x2d)
    --> pte index:0x14  pte contents:(valid 0, pfn 0x06)
      --> To Disk Sector Address 0x167 --> Value: 2c
```

代码,其中mem，disk是对应的内存和外存文件：
```
f=file('mem.txt')
mem = []
line = f.readline()
while line:
    t=line.strip().split(' ')
    tt=[]
    #print line
    for i in range(2,len(t)):
        tt.append(t[i])
    mem.append(tt)
    line = f.readline()
f=file('disk.txt')
disk = []
line = f.readline()
while line:
    t=line.strip().split(' ')
    tt=[]
    #print line
    for i in range(2,len(t)):
        tt.append(t[i])
    disk.append(tt)
    line = f.readline()
a=0x1c13
print 'pde index:',hex(a>>10)
print 'pde content',mem[int(0x6c)][int(a>>10)]
pde_content=mem[int(0x6c)][int(a>>10)]
if (int(pde_content,16))>>7==1:
    print 'valid 1'
    pfn = int(pde_content,16) & 0x7f
    print 'pfn',hex(pfn)
    print 'pte index:',hex(a>>5 & 0x1f)
    pte_index = a>>5 & 0x1f
    print 'pte content',mem[pfn][pte_index]
    pte_content = mem[pfn][pte_index]
    if (int(pte_content,16))>>7==1:
        print 'valid 1'
        pfn = int(pte_content,16) & 0x7f
        print 'pfn',hex(pfn)
        print 'physical address',hex((pfn << 5) |( a & 0x1f))
        physical_address = (pfn << 5) |( a & 0x1f)
        print 'Value',mem[physical_address>>5][physical_address & 0x1f]
    else:
        print 'valid 0'
        pfn = int(pte_content,16) & 0x7f
        print 'pfn',hex(pfn)
        print 'physical address',hex((pfn << 5) |( a & 0x1f))
        physical_address = (pfn << 5) |( a & 0x1f)
        print 'Value',disk[physical_address>>5][physical_address & 0x1f]
else:
    print 'valid 0'
    pfn = int(pde_content,16) & 0x7f
    print 'pfn',hex(pfn)

```
结果
```
6653 = 11001 10010 10011
11001在一级页表找到7f 无效 缺失
 
1c13 = 00111 00000 10011
找到bd 有效 3d里找第0个 f6 有效 找76的第19个 12
 
6809 = 11010 00000 01001
找到7f 无效
 
0af6 = 00010 10111 10110
找到a1 有效 26的第23个 7f 无效 在disk的7f里找10110(22) -> 03

1e6f = 00111 10011 01111
找到bd 有效 3d里的第19个 16 无效 在disk的16里找15 -> 1c
```



## 扩展思考题
---
(1)请分析原理课的缺页异常的处理流程与lab3中的缺页异常的处理流程（分析粒度到函数级别）的异同之处。
