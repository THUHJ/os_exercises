#lec 3 SPOC Discussion

## ������ �������жϡ��쳣��ϵͳ����-˼����

## 3.1 BIOS
 1. �Ƚ�UEFI��BIOS������
 UEFI ָͳһ�Ŀ���չ�̼��ӿڣ���һ����ϸ����ȫ�����ͽӿڵı�׼���������ڵ��Եı�׼�̼��ӿڡ�
 
 1. ����PXE�Ĵ����������̡�
 http://baike.baidu.com/link?url=h0fiIOD0aYE_X-PbiVdS9hF5M6AV85J5kNdwFKNRIapExNlfii9Xg47tQjBNeJ41E2Qujjzy7mJNhyOc_EhaK_#2
PXE(Pre-boot Execution Environment)����Intel��Ƶ�Э�飬������ʹ�����ͨ������������
������������Ϊ��
1.�ͻ��˸��˵��Կ���
2.���TCP/IP Bootrom�Լ죬��ÿ���Ȩ 
3.Bootprom �ͳ� BOOTP/DHCP Ҫ����ȡ�� IP��
4.�������յ����˵������ͳ���Ҫ�� �ͻ� BOOTP/DHCP ��Ӧ�����ݰ����ͻ��˵� IP ��ַ�� Ԥ�����أ� ������ӳ���ļ���
5.Bootprom �� TFTP ͨѶЭ��ӷ��������ؿ���ӳ���ļ���
6.���˵���ͨ���������ӳ���ļ������� ��������ļ�����ֻ�ǵ����Ŀ�����ʽҲ�����ǲ���ϵͳ��
7.����ӳ���ļ������� kernel loader ��ѹ������ kernel���� kernel ��֧��NTFS rootϵͳ��
8.Զ�̿ͻ��˸������ص��ļ�����������
 
 
## 3.2 ϵͳ��������
 1. �˽�NTLDR���������̡�
 NTLDR��һ�����صģ�ֻ����ϵͳ�ļ���λ����ϵͳ�̵ĸ�Ŀ¼������װ�ز���ϵͳ��
 һ�����ϵͳ�����������������Ĵ���
1����Դ�Լ����ʼ����
2����������¼��װ���ڴ棬���ҳ���ʼִ��
3�������������������װ���ڴ�
4��NTLDR������������װ�벢��ʼ��
5������������ʵģʽ��Ϊ32λƽ���ڴ�ģʽ
6��NTLDR��ʼ�����ʵ���С�ļ�ϵͳ��������
С�ļ�ϵͳ���������ǽ�����NTLDR�ڲ��ģ����ܶ�FAT��NTFS��
7��NTLDR��boot.ini�ļ�
8��NTLDRװ����ѡ����ϵͳ
���windows NT/windows 2000/windows XP/windows server 2003��Щ����ϵͳ��ѡ��NTLDR����Ntdetect��
���������Ĳ���ϵͳ��NTLDRװ�ز�����Bootsect.dosȻ���������ݿ��ơ�
windows NT���̽�����
9.Ntdetect���������Ӳ�������б��͸�NTLDR���Ա㽫��Щ��Ϣд��\\HKE Y_LOCAL_MACHINE\HARDWARE�С�
10.Ȼ��NTLDRװ��Ntoskrnl.exe��Hal.dll��ϵͳ��Ϣ���ϡ�
11.Ntldr����ϵͳ��Ϣ���ϣ���װ���豸���������Ա��豸������ʱ��ʼ����
12.Ntldr�ѿ���Ȩ����Ntoskrnl.exe����ʱ,�����������,װ�ؽ׶ο�ʼ
 
 1. �˽�GRUB���������̡�
 ��һ�׶Σ�BIOS���������׶Σ�
                        �ڸù�����ʵ��Ӳ���ĳ�ʼ���Լ������������ʣ�
                        ��MBR��װ������������������GRUB�������и�������������
�ڶ��׶Σ�GRUB���������׶Σ�
                        װ��stage1
                        װ��stage1.5
                        װ��stage2
                        ��ȡ/boot/grub.conf�ļ�����ʾ�����˵���
                        װ����ѡ��kernel��initrd�ļ����ڴ���
�����׶Σ��ں˽׶Σ�
                        �����ں�����������
                        ��ѹinitrd�ļ�������initd�ļ�ϵͳ��װ�ر����������
                        ���ظ��ļ�ϵͳ
���Ľ׶Σ�Sys V init��ʼ���׶Σ�
                        ����/sbin/init����
                        ����rc.sysinit�ű�������ϵͳ����������swap���������͹����ļ�ϵͳ��
                        ��ȡ/etc/inittab�ļ���������/et/rc.d/rc<#>.d�ж���Ĳ�ͬ���м���ķ����ʼ���ű���
                        ���ַ��ն�1-6�ſ���̨/��ͼ����ʾ�����7�ſ���̨
 
 
 
 1. �Ƚ�NTLDR��GRUB�Ĺ����в��졣
 
 
 
 
 
 
 
 
 1. �˽�u-boot�Ĺ��ܡ�
U-Boot��֧�ֵ���Ҫ�����б�
*ϵͳ����֧��NFS���ء�RAMDISK(ѹ�����ѹ��)��ʽ�ĸ��ļ�ϵͳ��֧��NFS���ء���FLASH������ѹ�����ѹ��ϵͳ�ںˣ�
* ������������ǿ��Ĳ���ϵͳ�ӿڹ��ܣ���������á����ݶ���ؼ�����������ϵͳ���ʺ�ϵͳ�ڲ�ͬ�����׶εĵ���Ҫ�����Ʒ����������Linux֧����Ϊǿ����֧��Ŀ��廷���������ִ洢��ʽ����FLASH��NVRAM��EEPROM��
* CRC32У���У��FLASH���ںˡ�RAMDISK�����ļ��Ƿ���ã�
* �豸�������ڡ�SDRAM��FLASH����̫����LCD��NVRAM��EEPROM�����̡�USB��PCMCIA��PCI��RTC������֧�֣�
* �ϵ��Լ칦��SDRAM��FLASH��С�Զ���⣻SDRAM���ϼ�⣻CPU�ͺţ�
* ���⹦��XIP�ں�������


## 3.3 �жϡ��쳣��ϵͳ���ñȽ�
 1. ����˵��Linux������Щ�жϣ���Щ�쳣��
 ϵͳ���ã�Ӳ���жϣ�ʱ���жϡ�
 
 �쳣 �����ϣ�Fault) �� ȱҳ�쳣�������
        ���壨Trap) ��ִ��û�б�Ҫ����ִ������ֹ��ָ��ʱ���������塣
        �쳣��ֹ��Abort) ���ڱ������صĴ���
        ����쳣 ��Programmed exception)�ڱ���߷��������Ƿ���������int ��(int3)ָ��ʱ��
 
1. Linux��ϵͳ��������Щ�����µĹ��ܷ�������Щ��  (w2l1)
��Linux2.4.4�汾���ں��У������ϵͳ������221����������<�ں�Դ����Ŀ¼>���ҵ�ԭ�����������ں˵ĸ���ϵͳ���õ�����Ҳ�ڲ��ϸ���
ϵͳ������Ҫ��Ϊ���¼��ࣺ

1.����Ӳ������ϵͳ����������ΪӲ����Դ���û��ռ�ĳ���ӿڣ������д�ļ�ʱ�õ���write/read���á�
2.����ϵͳ״̬���ȡ�ں����ݡ�����Ϊϵͳ�������û��ռ���ں˵�ΨһͨѶ�ֶΣ������û�����ϵͳ״̬�����翪/��ĳ���ں˷�������ĳ���ں˱����������ȡ�ں����ݶ�����ͨ��ϵͳ���á�����getpgid��getpriority��setpriority��sethostname
3.���̹�����һϵͳ���ýӿ���������֤ϵͳ�н������Զ������������ڴ滷���µ������С����� fork��clone��execve��exit��
����ϸ�ڰ���:
���̿��ƣ�fork ����һ���½��̣�clone ��ָ�����������ӽ��̵ȣ�
�ļ�������fcntl	�ļ����ƣ� open	���ļ���
�ļ�ϵͳ������access	ȷ���ļ��Ŀɴ�ȡ�ԣ�chdir	�ı䵱ǰ����Ŀ¼��
ϵͳ���ƣ�ioctl	I/O�ܿ��ƺ�����_sysctl	��/дϵͳ������
�ڴ����brk	�ı����ݶοռ�ķ��䣻mlock	�ڴ�ҳ�������
�������(getdomainname	ȡ����;setdomainname	��������)
�û�����(getuid	��ȡ�û���ʶ��;setuid	�����û���־��)


```
  + �ɷֵ㣺˵����Linux�Ĵ����������ϰٸ�����˵����Linuxϵͳ���õ���Ҫ���ࣨ�ļ����������̹����ڴ����ȣ�
  - ��û���漰��������Ҫ�㣻��0�֣�
  - �𰸶���������Ҫ���е�ĳһ��Ҫ���������ȷ������1�֣�
  - �𰸶���������Ҫ���������ȷ������2�֣�
  - �𰸳��˶���������Ҫ�㶼��������ȷ�����⣬����������չ�͸��ḻ��˵����3�֣�
 ```
 
 1. ��ucore lab8��answerΪ����uCore��ϵͳ��������Щ�����µĹ��ܷ�������Щ��(w2l1)
 ����22��ϵͳ���ð�����sys_exit;sys_fork;sys_wait;sys_exec;sys_yield[���������ó�������];sys_kill;sys_getpid[�õ����̱��];
sys_putc;sys_pgdir;sys_gettime;sys_lab6_set_priority;sys_sleep;sys_open;sys_close;sys_read;
sys_write;sys_seek;sys_fstat;sys_fsync;sys_getcwd;sys_getdirentry;
sys_dup[�����ơ�һ���򿪵��ļ��ţ�ʹ�����ļ��Ŷ�ָ��ͬһ���ļ�];)
���µĹ��ܷ����У��ļ�����(sys_open;sys_close;sys_read;��),�ļ�ϵͳ����(sys_getdirentry��),���̹���sys_fork;sys_kill;�ȣ�
 
 
 
 ```
  + �ɷֵ㣺˵����ucore�Ĵ�����������ʮ��������˵����ucoreϵͳ���õ���Ҫ���ࣨ�ļ����������̹����ڴ����ȣ�
  - ��û���漰��������Ҫ�㣻��0�֣�
  - �𰸶���������Ҫ���е�ĳһ��Ҫ���������ȷ������1�֣�
  - �𰸶���������Ҫ���������ȷ������2�֣�
  - �𰸳��˶���������Ҫ�㶼��������ȷ�����⣬����������չ�͸��ḻ��˵����3�֣�
 ```
 
## 3.4 linuxϵͳ���÷���
 1. ͨ������[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)
 �˽�LinuxӦ�õ�ϵͳ���ñ�д�ͺ��塣(w2l1)
 

 ```
  + �ɷֵ㣺˵����objdump��nm��file�Ĵ�����;��˵����ϵͳ���õľ��庬��
  - ��û���漰��������Ҫ�㣻��0�֣�
  - �𰸶���������Ҫ���е�ĳһ��Ҫ���������ȷ������1�֣�
  - �𰸶���������Ҫ���������ȷ������2�֣�
  - �𰸳��˶���������Ҫ�㶼��������ȷ�����⣬����������չ�͸��ḻ��˵����3�֣�
 
 ```
 
 1. ͨ������[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)
 �˽�LinuxӦ�õ�ϵͳ����ִ�й��̡�(w2l1)
 strace ������һ��ǿ��Ĺ��ߣ����ܹ���ʾ�������û��ռ���򷢳���ϵͳ���á�
 �������������£�
 �õ���ͬϵͳ���õ�ʱ�䣺
 % time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 30.54    0.000102          26         4           mprotect   [ �����ڴ�ӳ�񱣻� ]
 23.05    0.000077          10         8           mmap       [ ��һ���ļ�������������ӳ����ڴ� ]
 14.37    0.000048          48         1           write     
  8.98    0.000030          15         2           open      
  8.68    0.000029          29         1           munmap     [ ����ڴ�ӳ�� ]
  8.38    0.000028           9         3         3 access     [ �����ý����Ƿ���Զ�ָ�����ļ�ִ��ĳ�ֲ��� ]
  2.10    0.000007           2         3           fstat      [ ���ļ�������ȡ���ļ�״̬ ]
  1.20    0.000004           4         1           read
  0.90    0.000003           2         2           close
  0.90    0.000003           3         1           execve
  0.60    0.000002           2         1           brk        [ ʵ�������ڴ浽�ڴ��ӳ�� ]
  0.30    0.000001           1         1           arch_prctl [ ���üܹ��ض����߳�״̬ ]
------ ----------- ----------- --------- --------- ----------------
100.00    0.000334                    28         3 total
����ϵͳ����ִ������Ϊ��
execve("./lab1-ex1.exe", ["./lab1-ex1.exe"], [/* 73 vars */]) = 0
brk(0)                                  = 0x11bc000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7ff6185c5000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=93381, ...}) = 0
mmap(NULL, 93381, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7ff6185ae000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\320\37\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1845024, ...}) = 0
mmap(NULL, 3953344, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7ff617fdf000
mprotect(0x7ff61819a000, 2097152, PROT_NONE) = 0
mmap(0x7ff61839a000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1bb000) = 0x7ff61839a000
mmap(0x7ff6183a0000, 17088, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7ff6183a0000
close(3)                                = 0
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7ff6185ad000
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7ff6185ab000
arch_prctl(ARCH_SET_FS, 0x7ff6185ab740) = 0
mprotect(0x7ff61839a000, 16384, PROT_READ) = 0
mprotect(0x600000, 4096, PROT_READ)     = 0
mprotect(0x7ff6185c7000, 4096, PROT_READ) = 0
munmap(0x7ff6185ae000, 93381)           = 0
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 0), ...}) = 0
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7ff6185c4000
write(1, "hello world\n", 12hello world)           = 12
exit_group(12)                          = ?

����ִ�й��̣�
���Ƚ��������ڴ浽�ڴ��ӳ�䣬Ȼ���ж�ld.so.nohwcap��ld.so.preload�����ļ��Ƿ����ʹ�á��ж�֮���ļ�
ӳ�䵽�ڴ棬�������ڴ�ӳ�񱣻���Ȼ�����üܹ��ض����߳�״̬�����յ���write�ĺ���������Ļ����ƶ����ַ�����

 ```
  + �ɷֵ㣺˵����strace�Ĵ�����;��˵����ϵͳ���õľ���ִ�й��̣�����Ӧ�ã�CPUӲ��������ϵͳ��ִ�й��̣�
  - ��û���漰��������Ҫ�㣻��0�֣�
  - �𰸶���������Ҫ���е�ĳһ��Ҫ���������ȷ������1�֣�
  - �𰸶���������Ҫ���������ȷ������2�֣�
  - �𰸳��˶���������Ҫ�㶼��������ȷ�����⣬����������չ�͸��ḻ��˵����3�֣�
 ```
 
## 3.5 ucoreϵͳ���÷���
 1. ucore��ϵͳ�����в������ݴ��������
 1. ucore��ϵͳ�����з��ؽ���Ĵ��ݴ��������
 1. ��ucore lab8��answerΪ��������ucore Ӧ�õ�ϵͳ���ñ�д�ͺ��塣
 1. ��ucore lab8��answerΪ���������޸Ĳ����д��룬����ucoreӦ�õ�ϵͳ����ִ�й��̡�
 
## 3.6 ������������ú�ϵͳ���õ�����
 1. ��Ӵ����д��ִ�й�����˵����
   1. ˵��`int`��`iret`��`call`��`ret`��ָ��׼ȷ����
 
