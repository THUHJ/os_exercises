# lec5 SPOC思考题


NOTICE
- 有"w3l1"标记的题是助教要提交到学堂在线上的。
- 有"w3l1"和"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的git repo上。
- 有"hard"标记的题有一定难度，鼓励实现。
- 有"easy"标记的题很容易实现，鼓励实现。
- 有"midd"标记的题是一般水平，鼓励实现。


## 个人思考题
---

请简要分析最优匹配，最差匹配，最先匹配，buddy systemm分配算法的优势和劣势，并尝试提出一种更有效的连续内存分配算法 (w3l1)
```
  + 采分点：说明四种算法的优点和缺点
  - 答案没有涉及如下3点；（0分）
  - 正确描述了二种分配算法的优势和劣势（1分）
  - 正确描述了四种分配算法的优势和劣势（2分）
  - 除上述两点外，进一步描述了一种更有效的分配算法（3分）
 ```
- [x]  

>  
最优匹配     
优点：大部分分配的尺寸较小的时候效果好，避免大的空闲分区被拆分。可减少外部碎片的大小。实现相对简单。   
缺点：会增加外部碎片的数量，释放分区速度慢，容易产生很多无用的碎片。
最差匹配 
优点：中等大小的分配较多的时候，效果最好。避免出现太多小的碎片。      
缺点：释放分区速度慢，会产生外部碎片。容易破坏大的空闲分区，因此后续难以分配大分区。
最先匹配   
优点：实现简单，在高地址有大块空闲分区；     
缺点：容易产生外部碎片，分配较大块时速度较慢。   
buddy systemm分配算法
优点：减少了内存的碎片，使内存有较高的利用率。
缺点：实现相对复杂。可能会出现小碎片，影响大的空闲分区的合并。   

更有效的连续内存分配算法：   
结合最优匹配和最差匹配各自实用的情况与优点，可以得到这样一种分配方式。设置一个概率值p，表示我从大空闲块分配的概率，
另外的1-p的概率情况从小空闲块开始分配。即对于每次分配，从已经排好序的空闲块中，我可能从大端开始，也可能从小端开始。
同时，这个概率值p并不是固定的，它会根据分配请求的情况调整。当分配块较小时，减小p的值，反之增加p的值。这样就可以充分
利用最优匹配与最差匹配的优点，提高内存的利用率。

## 小组思考题

请参考ucore lab2代码，
采用`struct pmm_manager` 根据你的`学号 mod 4`的结果值，选择四种
（0:最优匹配，1:最差匹配，2:最先匹配，3:buddy systemm）
分配算法中的一种或多种，在应用程序层面(可以 用python,ruby,C++，C，LISP等高语言)
来实现，给出你的设思路，并给出测试用例。 (spoc)
```
如何表示空闲块？ 如何表示空闲块列表？ 
[(start0, size0),(start1,size1)...]
在一次malloc后，如果根据某种顺序查找符合malloc要求的空闲块？如何把一个空闲块改变成另外一个空闲块，或消除这个空闲块？如何更新空闲块列表？
在一次free后，如何把已使用块转变成空闲块，并按照某种顺序（起始地址，块大小）插入到空闲块列表中？考虑需要合并相邻空闲块，形成更大的空闲块？
如果考虑地址对齐（比如按照4字节对齐），应该如何设计？
如果考虑空闲/使用块列表组织中有部分元数据，比如表示链接信息，如何给malloc返回有效可用的空闲块地址而不破坏
元数据信息？
伙伴分配器的一个极简实现
http://coolshell.cn/tag/buddy
```

实现代码：
```
#include<stdio.h>
#include<malloc.h>
#include <iostream>
using namespace std;
const int Memmax = 1000;
char mem[Memmax];
typedef struct block
{
	char* address; 
    int size; 
    struct block *next;
}block;
block freelist;
void init()
{
	block *temp = new block;
    temp->address = mem;
    temp->size = Memmax;
    temp->next = NULL;
    freelist.next = temp;  
}
char* my_malloc(int size)
{
	cout<<"Malloc "<<size<<endl;
	size = size + sizeof(int);
	int min = Memmax;
	block* result = NULL;
	block* pre_result = NULL;
	block* pre = NULL;
	block*temp = freelist.next;
	while (temp!=NULL)
	{
		if (temp->size>=size && (temp->size-size<min))
		{
			min = temp->size-size;
			pre_result = pre;
			result = temp;
		}
		pre = temp;
		temp= temp->next;
	}
	if (result==NULL) {cout<<"Malloc failed"<<endl;return NULL;}//means fails;

	char *address = result->address;
	if (	result->size == result->size-size)//delete the block
	{
		
		pre_result = result->next;
		free(result);
	}
	else
	{
		result->size=result->size-size;
		result->address = result->address+size;
	}
	*(int *)address = size;
	address = address + sizeof(int);
	cout<<"Malloc succeed!"<<endl;
	return address;

}
void my_merge()
{
	block *temp = freelist.next;
	if (temp==NULL) return;
	block *pre = temp;
	temp = temp->next;
	while (temp!=NULL)
	{
		if ((int)pre->address+pre->size==(int)temp->address)
		{
			pre->size=pre->size+temp->size;
			pre->next = temp->next;
			free(temp);
			temp = pre->next;
		}
		else
		{
			pre=temp;
			temp=temp->next;
		}
	}
}
void my_free(char*address)
{
	int size = *(int *)(address-sizeof(int));
	cout<<"free address "<<(int)address-sizeof(int)<<" size: "<<size<<endl;
	block *newblock = new block;
	newblock->address =address-sizeof(int);
	newblock->next = NULL;
	newblock->size = size;
	
	block *temp = freelist.next;
	block *pre = &freelist;
	while (temp!=NULL)//insert
	{
		if ((int)temp->address>=(int)address)
		{
			pre->next = newblock;
			newblock->next = temp;
			break;
		}
		pre = temp;
		temp= temp->next;
	}
	my_merge();
	
}
void display()
{
	cout<<endl;
	cout<<"display begin--------------------"<<endl;
	block *temp = freelist.next;
	int n=0;
	while (temp!=NULL)
	{
		n++;
		cout<<"freeblock"<<n<<"  address: "<<(int)temp->address<<" size:"<<temp->size<<endl;
		temp=temp->next;
	}
	cout<<"display end--------------------"<<endl;
	cout<<endl;
}
int main()
{
	init();
	display();
	char * m1 = my_malloc(100);
	char * m2 = my_malloc(400);
	char * m3 = my_malloc(600);
	char * m4 = my_malloc(100);
	display();
	my_free(m2);
	display();
	my_free(m1);
	display();
	char * m5 = my_malloc(300);
	display();
	my_free(m4);
	my_free(m5);
	display();
}
'''
设计思路：
    我的学号为2012011272，实现最优匹配算法。
    用一个链表freelist来保存空闲分区，主要实现my_free,my_malloc,init,display,my_merge这五个函数。
	init进行初始化，开始时拥有一个长度为Memmax的空闲分区。
	display进行输出，观察现有的freelist情况。
	my_malloc进行内存分配，采用最优匹配，每次选择最小的可以容纳的空闲分区。函数返回可以使用的内存开始地址。
	my_free进行内存释放，将空闲块加入freelist中，然后调用my_merge这个函数对freelist中的可以合并的分区进行合并。
测试用例：
'''
	init();
	display();
	char * m1 = my_malloc(100);
	char * m2 = my_malloc(400);
	char * m3 = my_malloc(600);
	char * m4 = my_malloc(100);
	display();
	my_free(m2);
	display();
	my_free(m1);
	display();
	char * m5 = my_malloc(300);
	display();
	my_free(m4);
	my_free(m5);
	display();
'''
测试结果：
'''
	display begin--------------------
	freeblock1  address: 4223008 size:1000
	display end--------------------
	
	Malloc 100
	Malloc succeed!
	Malloc 400
	Malloc succeed!
	Malloc 600
	Malloc failed
	Malloc 100
	Malloc succeed!

	display begin--------------------
	freeblock1  address: 4223620 size:388
	display end--------------------

	free address 4223112 size: 404

	display begin--------------------
	freeblock1  address: 4223112 size:404
	freeblock2  address: 4223620 size:388
	display end--------------------

	free address 4223008 size: 104

	display begin--------------------
	freeblock1  address: 4223008 size:508
	freeblock2  address: 4223620 size:388
	display end--------------------

	Malloc 300
	Malloc succeed!

	display begin--------------------
	freeblock1  address: 4223008 size:508
	freeblock2  address: 4223924 size:84
	display end--------------------

	free address 4223516 size: 104
	free address 4223620 size: 304

	display begin--------------------
	freeblock1  address: 4223008 size:1000
	display end--------------------
'''
用例说明：
'''
	开始时要求分配大小为100,400,600,100的空间，其中600由于太大分配失败，其他可以成功分配。
	分配后剩余一个freeblock1  address: 4223620 size:388
	然后依次释放400、100（第一块）的，能够成功释放并且正确合并，这个时候得到：
	freeblock1  address: 4223008 size:508
	freeblock2  address: 4223620 size:388
	然后再要求分配大小为300的空间。这里由于采用最优匹配，程序会选择388的块进行使用。
	分配后结果为：
	freeblock1  address: 4223008 size:508
	freeblock2  address: 4223924 size:84
	最后释放剩余两块恢复开始情况。说明要求分配大小为n的情况，实际会使用给它n+4字节大小，因为需要来存储使用的长度。
'''
--- 

## 扩展思考题

阅读[slab分配算法](http://en.wikipedia.org/wiki/Slab_allocation)，尝试在应用程序中实现slab分配算法，给出设计方案和测试用例。


