# ͬ������(lec 17) spoc ˼����


- ��"spoc"��ǵ�����Ҫ�����廪ѧ�ֵ�ͬѧҪ��ʵ�������ɣ�����ʱ�ύ��ѧ����Ӧ��ucore_code��os_exercises��git repo�ϡ�

## ����˼����

### ����
 - �����������ȷ�ԵĶ������͡�
 - ��һ�������л����г�����Ϊ��ԭ����Ԥ�ڲ�һ�£��Ǵ�����
 - ���򲢷�ִ����ʲô�ô����ϰ���
 - ʲô��ԭ�Ӳ�����

### ��ʵ�����е�ͬ������

 - ��ͥ�ɹ��е�ͬ�����������ϵͳ�н���ͬ����ʲô����
 - ���ͨ��ö�ٺͷ��෽�����ͬ���㷨����ȷ�ԣ�
 - �������������ĵ���ȷ�ԡ�
 - ���⡢�����ͼ����Ķ�����ʲô��

### �ٽ����ͽ���Ӳ���ж�ͬ������

 - ʲô���ٽ�����
 - �ٽ����ķ��ʹ�����ʲô��
 - �����ж������ʵ�ֶ��ٽ����ķ��ʿ��Ƶģ���ʲô��ȱ�㣿

### ���������ͬ������

 - ����ͨ��ö�ٺͷ��෽�����Peterson�㷨����ȷ�ԡ�
 - ����׼ȷ����Eisenbergͬ���㷨����ͨ��ö�ٺͷ��෽���������ȷ�ԡ�

### �߼������ͬ������

 - ���֤��TSָ��ͽ���ָ��ĵȼ��ԣ�
 - ΪʲôӲ��ԭ�Ӳ���ָ���ܼ�ͬ���㷨��ʵ�֣�
 
## С��˼����

С���Ա  
��24 �ž� 2012011354  
��22 �ƽ� 2012011272  
��22 ԬԴ 2012011294

1. ��spoc���Ķ�[��x86�����ģ������ʹ��˵��](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab7/lab7-spoc-exercise.md)�������ڼ�x86������Ļ����롣

2. ��spoc)�˽�race condition. ����[race-condition����Ŀ¼](https://github.com/chyyuu/ucore_lab/tree/master/related_info/lab7/race-condition)��

 - ִ�� `./x86.py -p loop.s -t 1 -i 100 -R dx`�� ����`dx`��ֵ��ʲô��  
 dx= 0. ��Ϊֻ�����һ���Ƿ����0���˳��ˡ�  

 - ִ�� `./x86.py -p loop.s -t 2 -i 100 -a dx=3,dx=3 -R dx` �� ����`dx`��ֵ��ʲô��  
 dx = 3; ��Ϊ�����4����˳���������Ϊ�߳��л�ʱ�ᱣ��Ĵ�������Ϣ�������������̵�dx��ʼֵ����3.  

 - ִ�� `./x86.py -p loop.s -t 2 -i 3 -r -a dx=3,dx=3 -R dx`�� ����`dx`��ֵ��ʲô��  
 dx = 3; ����������ս�������ֻ���жϴ��������ˡ�  

 - ����x���ڴ��ַΪ2000, `./x86.py -p looping-race-nolock.s -t 1 -M 2000`, ���ʱ���x��ֵ��ʲô��  
 dx= 0;     
 - ����x���ڴ��ַΪ2000, `./x86.py -p looping-race-nolock.s -t 2 -a bx=3 -M 2000`, ���ʱ���x��ֵ��ʲô��Ϊ��ÿ���߳�Ҫѭ��3�Σ�  
 ��Ϊbx = 3 , ����ѭ��3�Ρ���Ϊ�̶߳�����Ա����ʼ��bx������ÿ���̶߳���ѭ��3�Ρ�  
 - ����x���ڴ��ַΪ2000, `./x86.py -p looping-race-nolock.s -t 2 -M 2000 -i 4 -r -s 0`�� ���ʱ���x��ֵ��ʲô��  
 1����2.  ��Ϊ�жϵ�Ƶ�ʲ�ͬ�������� move %ax 2000֮ǰ�����л���Ҳ������֮�������߳�2��ʼ�����ĵ�ַΪ2000��ֵ����Ϊ0Ҳ����Ϊ1���������ս����ͬ��  

 - ����x���ڴ��ַΪ2000, `./x86.py -p looping-race-nolock.s -t 2 -M 2000 -i 4 -r -s 1`�� ���ʱ���x��ֵ��ʲô��  
 ���������һ����  

 - ����x���ڴ��ַΪ2000, `./x86.py -p looping-race-nolock.s -t 2 -M 2000 -i 4 -r -s 2`�� ���ʱ���x��ֵ��ʲô��   
 ���������һ����  

 - ����x���ڴ��ַΪ2000, `./x86.py -p looping-race-nolock.s -a bx=1 -t 2 -M 2000 -i 1`�� ���ʱ���x��ֵ��ʲô�� 
x��1�� ÿһ����䶼�����һ���жϣ�bx = 1 �������ӵĲ���ֻ��һ�Ρ������̶߳����ĵ�ַΪ2000��ֵ����0�����Խ��ֻ����1�Σ�Ϊ1.  

3. ��spoc�� �˽�software-based lock, hardware-based lock, [software-hardware-lock����Ŀ¼](https://github.com/chyyuu/ucore_lab/tree/master/related_info/lab7/software-hardware-locks)

  - ���flag.s,peterson.s,test-and-set.s,ticket.s,test-and-test-and-set.s ��ͨ��x86.py������Щ�����Ƿ�ʵ���������ƣ���������ʵ����̺ͽ���˵�����ܷ�����µ�Ӳ��ԭ�Ӳ���ָ��Compare-And-Swap,Fetch-And-Add��  
  1. flag.s û��ʵ�������ƣ����� ./x86.py -p flag.s -M count -c -i 1 ����ִ���  
  2. peterson.s û��ʵ�������ƣ�����./x86.py -p peterson.s -i 1 -M count -c ����ִ���
  3. test-and-set.s ʵ���������ƣ���Ϊ����ʹ����xchg %ax, mutexԭ�Ӳ�������֤��ָ��ᱻ��ϡ� ��������в��ԣ��������ȷ�ġ�  
  4. ticket.s ʵ���������ơ���Ϊʹ����fetchadd ԭ�Ӳ�����ticket��ʾ��ʱ�������е��߳�ID��turn��ʾ�����е��̺߳ţ������߲����ʱ����æ�ȴ�����������ٽ������С�  
  5. test-and-test-and-set.s ʵ���������ơ�����mutexΪ1ʱ˵�����߳�ռ�ã�Ϊ0ʱ˵�����߳�ռ�á�������ȡֵ�͸�ֵ����һ��ԭ�Ӳ�����ʵ�ֵģ����Բ������ж϶�����Ӱ�졣
```
Compare-And-Swap

int CompareAndSwap(int *ptr, int expected, int new) {
  int actual = *ptr;
  if (actual == expected)
    *ptr = new;
  return actual;
}
```

```
Fetch-And-Add

int FetchAndAdd(int *ptr) {
  int old = *ptr;
  *ptr = old + 1;
  return old;
}
```
