# lec5 SPOC˼����


NOTICE
- ��"w3l1"��ǵ���������Ҫ�ύ��ѧ�������ϵġ�
- ��"w3l1"��"spoc"��ǵ�����Ҫ�����廪ѧ�ֵ�ͬѧҪ��ʵ�������ɣ�����ʱ�ύ��ѧ����Ӧ��git repo�ϡ�
- ��"hard"��ǵ�����һ���Ѷȣ�����ʵ�֡�
- ��"easy"��ǵ��������ʵ�֣�����ʵ�֡�
- ��"midd"��ǵ�����һ��ˮƽ������ʵ�֡�


## ����˼����
---

���Ҫ��������ƥ�䣬���ƥ�䣬����ƥ�䣬buddy systemm�����㷨�����ƺ����ƣ����������һ�ָ���Ч�������ڴ�����㷨 (w3l1)
```
  + �ɷֵ㣺˵�������㷨���ŵ��ȱ��
  - ��û���漰����3�㣻��0�֣�
  - ��ȷ�����˶��ַ����㷨�����ƺ����ƣ�1�֣�
  - ��ȷ���������ַ����㷨�����ƺ����ƣ�2�֣�
  - �����������⣬��һ��������һ�ָ���Ч�ķ����㷨��3�֣�
 ```
- [x]  

>  
����ƥ��     
�ŵ㣺�󲿷ַ���ĳߴ��С��ʱ��Ч���ã������Ŀ��з�������֡��ɼ����ⲿ��Ƭ�Ĵ�С��ʵ����Լ򵥡�   
ȱ�㣺�������ⲿ��Ƭ���������ͷŷ����ٶ��������ײ����ܶ����õ���Ƭ��
���ƥ�� 
�ŵ㣺�еȴ�С�ķ���϶��ʱ��Ч����á��������̫��С����Ƭ��      
ȱ�㣺�ͷŷ����ٶ�����������ⲿ��Ƭ�������ƻ���Ŀ��з�������˺������Է���������
����ƥ��   
�ŵ㣺ʵ�ּ򵥣��ڸߵ�ַ�д����з�����     
ȱ�㣺���ײ����ⲿ��Ƭ������ϴ��ʱ�ٶȽ�����   
buddy systemm�����㷨
�ŵ㣺�������ڴ����Ƭ��ʹ�ڴ��нϸߵ������ʡ�
ȱ�㣺ʵ����Ը��ӡ����ܻ����С��Ƭ��Ӱ���Ŀ��з����ĺϲ���   

����Ч�������ڴ�����㷨��   
�������ƥ������ƥ�����ʵ�õ�������ŵ㣬���Եõ�����һ�ַ��䷽ʽ������һ������ֵp����ʾ�ҴӴ���п����ĸ��ʣ�
�����1-p�ĸ��������С���п鿪ʼ���䡣������ÿ�η��䣬���Ѿ��ź���Ŀ��п��У��ҿ��ܴӴ�˿�ʼ��Ҳ���ܴ�С�˿�ʼ��
ͬʱ���������ֵp�����ǹ̶��ģ�������ݷ�������������������������Сʱ����Сp��ֵ����֮����p��ֵ�������Ϳ��Գ��
��������ƥ�������ƥ����ŵ㣬����ڴ�������ʡ�

## С��˼����

��ο�ucore lab2���룬
����`struct pmm_manager` �������`ѧ�� mod 4`�Ľ��ֵ��ѡ������
��0:����ƥ�䣬1:���ƥ�䣬2:����ƥ�䣬3:buddy systemm��
�����㷨�е�һ�ֻ���֣���Ӧ�ó������(���� ��python,ruby,C++��C��LISP�ȸ�����)
��ʵ�֣����������˼·������������������ (spoc)
```
��α�ʾ���п飿 ��α�ʾ���п��б� 
[(start0, size0),(start1,size1)...]
��һ��malloc���������ĳ��˳����ҷ���mallocҪ��Ŀ��п飿��ΰ�һ�����п�ı������һ�����п飬������������п飿��θ��¿��п��б�
��һ��free����ΰ���ʹ�ÿ�ת��ɿ��п飬������ĳ��˳����ʼ��ַ�����С�����뵽���п��б��У�������Ҫ�ϲ����ڿ��п飬�γɸ���Ŀ��п飿
������ǵ�ַ���루���簴��4�ֽڶ��룩��Ӧ�������ƣ�
������ǿ���/ʹ�ÿ��б���֯���в���Ԫ���ݣ������ʾ������Ϣ����θ�malloc������Ч���õĿ��п��ַ�����ƻ�
Ԫ������Ϣ��
����������һ������ʵ��
http://coolshell.cn/tag/buddy
```

ʵ�ִ��룺
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
���˼·��
    �ҵ�ѧ��Ϊ2012011272��ʵ������ƥ���㷨��
    ��һ������freelist��������з�������Ҫʵ��my_free,my_malloc,init,display,my_merge�����������
	init���г�ʼ������ʼʱӵ��һ������ΪMemmax�Ŀ��з�����
	display����������۲����е�freelist�����
	my_malloc�����ڴ���䣬��������ƥ�䣬ÿ��ѡ����С�Ŀ������ɵĿ��з������������ؿ���ʹ�õ��ڴ濪ʼ��ַ��
	my_free�����ڴ��ͷţ������п����freelist�У�Ȼ�����my_merge���������freelist�еĿ��Ժϲ��ķ������кϲ���
����������
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
���Խ����
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
����˵����
'''
	��ʼʱҪ������СΪ100,400,600,100�Ŀռ䣬����600����̫�����ʧ�ܣ��������Գɹ����䡣
	�����ʣ��һ��freeblock1  address: 4223620 size:388
	Ȼ�������ͷ�400��100����һ�飩�ģ��ܹ��ɹ��ͷŲ�����ȷ�ϲ������ʱ��õ���
	freeblock1  address: 4223008 size:508
	freeblock2  address: 4223620 size:388
	Ȼ����Ҫ������СΪ300�Ŀռ䡣�������ڲ�������ƥ�䣬�����ѡ��388�Ŀ����ʹ�á�
	�������Ϊ��
	freeblock1  address: 4223008 size:508
	freeblock2  address: 4223924 size:84
	����ͷ�ʣ������ָ���ʼ�����˵��Ҫ������СΪn�������ʵ�ʻ�ʹ�ø���n+4�ֽڴ�С����Ϊ��Ҫ���洢ʹ�õĳ��ȡ�
'''
--- 

## ��չ˼����

�Ķ�[slab�����㷨](http://en.wikipedia.org/wiki/Slab_allocation)��������Ӧ�ó�����ʵ��slab�����㷨��������Ʒ����Ͳ���������


