#lec 3 SPOC Discussion

## 第三讲 启动、中断、异常和系统调用-思考题

## 3.1 BIOS
 1. 比较UEFI和BIOS的区别。
 UEFI 指统一的可扩展固件接口，是一种详细描述全新类型接口的标准，是适用于电脑的标准固件接口。    
 
 1. 描述PXE的大致启动流程。    
 http://baike.baidu.com/link?url=h0fiIOD0aYE_X-PbiVdS9hF5M6AV85J5kNdwFKNRIapExNlfii9Xg47tQjBNeJ41E2Qujjzy7mJNhyOc_EhaK_#2
PXE(Pre-boot Execution Environment)是由Intel设计的协议，它可以使计算机通过网络启动。    
大致启动流程为：    
1.客户端个人电脑开机       
2.完成TCP/IP Bootrom自检，获得控制权     
3.Bootprom 送出 BOOTP/DHCP 要求以取得 IP。    
4.服务器收到个人电脑所送出的要求， 送回 BOOTP/DHCP 回应，内容包括客户端的 IP 地址， 预设网关， 及开机映像文件。    
5.Bootprom 由 TFTP 通讯协议从服务器下载开机映像文件。    
6.个人电脑通过这个开机映像文件开机， 这个开机文件可以只是单纯的开机程式也可以是操作系统。    
7.开机映像文件将包含 kernel loader 及压缩过的 kernel，此 kernel 将支持NTFS root系统。    
8.远程客户端根据下载的文件启动机器。        
 
 
## 3.2 系统启动流程
 1. 了解NTLDR的启动流程。    
 NTLDR是一个隐藏的，只读的系统文件，位置在系统盘的根目录，用来装载操作系统。    
 一般情况系统的引导过程是这样的代码    
1、电源自检程序开始运行
2、主引导记录被装入内存，并且程序开始执行    
3、活动分区的引导扇区被装入内存    
4、NTLDR从引导扇区被装入并初始化    
5、将处理器的实模式改为32位平滑内存模式    
6、NTLDR开始运行适当的小文件系统驱动程序。        
小文件系统驱动程序是建立在NTLDR内部的，它能读FAT或NTFS。    
7、NTLDR读boot.ini文件    
8、NTLDR装载所选操作系统    
如果windows NT/windows 2000/windows XP/windows server 2003这些操作系统被选择，NTLDR运行Ntdetect。    
对于其他的操作系统，NTLDR装载并运行Bootsect.dos然后向它传递控制。    
windows NT过程结束。    
9.Ntdetect搜索计算机硬件并将列表传送给NTLDR，以便将这些信息写进\\HKE Y_LOCAL_MACHINE\HARDWARE中。    
10.然后NTLDR装载Ntoskrnl.exe，Hal.dll和系统信息集合。    
11.Ntldr搜索系统信息集合，并装载设备驱动配置以便设备在启动时开始工作    
12.Ntldr把控制权交给Ntoskrnl.exe，这时,启动程序结束,装载阶段开始    
 
 1. 了解GRUB的启动流程。
 第一阶段：BIOS启动引导阶段；    
                        在该过程中实现硬件的初始化以及查找启动介质；    
                        从MBR中装载启动引导管理器（GRUB）并运行该启动引导管理    
第二阶段：GRUB启动引导阶段；    
                        装载stage1    
                        装载stage1.5                
                        装载stage2            
                        读取/boot/grub.conf文件并显示启动菜单；        
                        装载所选的kernel和initrd文件到内存中    
第三阶段：内核阶段：
                        运行内核启动参数；        
                        解压initrd文件并挂载initd文件系统，装载必须的驱动；    
                        挂载根文件系统    
第四阶段：Sys V init初始化阶段：
                        启动/sbin/init程序    ；    
                        运行rc.sysinit脚本，设置系统环境，启动swap分区，检查和挂载文件系统；        
                        读取/etc/inittab文件，运行在/et/rc.d/rc<#>.d中定义的不同运行级别的服务初始化脚本；    
                        打开字符终端1-6号控制台/打开图形显示管理的7号控制台        
 
 
 
 1. 比较NTLDR和GRUB的功能有差异。    
 
 
 
 
 
 
 
 
 1. 了解u-boot的功能。
U-Boot可支持的主要功能列表：    
*系统引导支持NFS挂载、RAMDISK(压缩或非压缩)形式的根文件系统；支持NFS挂载、从FLASH中引导压缩或非压缩系统内核；        
* 基本辅助功能强大的操作系统接口功能；可灵活设置、传递多个关键参数给操作系统，适合系统在不同开发阶段的调试要求与产品发布，尤以Linux支持最为强劲；支持目标板环境参数多种存储方式，如FLASH、NVRAM、EEPROM；    
* CRC32校验可校验FLASH中内核、RAMDISK镜像文件是否完好；    
* 设备驱动串口、SDRAM、FLASH、以太网、LCD、NVRAM、EEPROM、键盘、USB、PCMCIA、PCI、RTC等驱动支持；    
* 上电自检功能SDRAM、FLASH大小自动检测；SDRAM故障检测；CPU型号；    
* 特殊功能XIP内核引导；    


## 3.3 中断、异常和系统调用比较
 1. 举例说明Linux中有哪些中断，哪些异常？    
 系统调用，硬件中断，时钟中断。    
 
 异常 ：故障（Fault) 如 缺页异常处理程序        
        陷阱（Trap) 当执行没有必要重新执行已终止的指令时，触发陷阱。        
        异常终止（Abort) 用于报告严重的错误。        
        编程异常 （Programmed exception)在编程者发出请求是发生。是由int 或(int3)指令时触        
 
1. Linux的系统调用有哪些？大致的功能分类有哪些？  (w2l1)
在Linux2.4.4版本的内核中，狭义的系统调用有221个，可以在<内核源代码目录>中找到原本。但随着内核的更新系统调用的数量也在不断更新
系统调用主要分为以下几类：    

1.控制硬件——系统调用往往作为硬件资源和用户空间的抽象接口，比如读写文件时用到的write/read调用。    
2.设置系统状态或读取内核数据——因为系统调用是用户空间和内核的唯一通讯手段，所以用户设置系统状态，比如开/关某项内核服务（设置某个内核变量），或读取内核数据都必须通过系统调用。比如getpgid、getpriority、setpriority、sethostname    
3.进程管理——一系统调用接口是用来保证系统中进程能以多任务在虚拟内存环境下得以运行。比如 fork、clone、execve、exit等    
具体细节包括:    
进程控制（fork 创建一个新进程；clone 按指定条件创建子进程等）    
文件操作（fcntl	文件控制； open	打开文件）    
文件系统操作（access	确定文件的可存取性；chdir	改变当前工作目录）    
系统控制（ioctl	I/O总控制函数；_sysctl	读/写系统参数）    
内存管理（brk	改变数据段空间的分配；mlock	内存页面加锁）    
网络管理(getdomainname	取域名;setdomainname	设置域名)    
用户管理(getuid	获取用户标识号;setuid	设置用户标志号)    


```
  + 采分点：说明了Linux的大致数量（上百个），说明了Linux系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
 1. 以ucore lab8的answer为例，uCore的系统调用有哪些？大致的功能分类有哪些？(w2l1)
 共有22个系统调用包括（sys_exit;sys_fork;sys_wait;sys_exec;sys_yield[进程主动让出处理器];sys_kill;sys_getpid[得到进程编号];        
sys_putc;sys_pgdir;sys_gettime;sys_lab6_set_priority;sys_sleep;sys_open;sys_close;sys_read;        
sys_write;sys_seek;sys_fstat;sys_fsync;sys_getcwd;sys_getdirentry;        
sys_dup[“复制”一个打开的文件号，使两个文件号都指向同一个文件];)        
大致的功能分类有，文件操作(sys_open;sys_close;sys_read;等),文件系统操作(sys_getdirentry等),进程管理（sys_fork;sys_kill;等）        
 
 
 
 ```
  + 采分点：说明了ucore的大致数量（二十几个），说明了ucore系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.4 linux系统调用分析
 1. 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)
 了解Linux应用的系统调用编写和含义。(w2l1)
 

 ```
  + 采分点：说明了objdump，nm，file的大致用途，说明了系统调用的具体含义
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 
 ```
 
 1. 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)
 了解Linux应用的系统调用执行过程。(w2l1)    
 strace 命令是一种强大的工具，它能够显示所有由用户空间程序发出的系统调用。    
 最终输出结果如下：    
 得到不同系统调用的时间：    
 % time     seconds  usecs/call     calls    errors syscall    
------ ----------- ----------- --------- --------- ----------------
 30.54    0.000102          26         4           mprotect   [ 设置内存映像保护 ]    
 23.05    0.000077          10         8           mmap       [ 将一个文件或者其它对象映射进内存 ]    
 14.37    0.000048          48         1           write         
  8.98    0.000030          15         2           open          
  8.68    0.000029          29         1           munmap     [ 解除内存映射 ]    
  8.38    0.000028           9         3         3 access     [ 检查调用进程是否可以对指定的文件执行某种操作 ]    
  2.10    0.000007           2         3           fstat      [ 由文件描述词取得文件状态 ]    
  1.20    0.000004           4         1           read    
  0.90    0.000003           2         2           close    
  0.90    0.000003           3         1           execve    
  0.60    0.000002           2         1           brk        [ 实现虚拟内存到内存的映射 ]    
  0.30    0.000001           1         1           arch_prctl [ 设置架构特定的线程状态 ]        
------ ----------- ----------- --------- --------- ----------------        
100.00    0.000334                    28         3 total        
具体系统调用执行流程为：    
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

首先进行虚拟内存到内存的映射，然后判断ld.so.nohwcap与ld.so.preload两个文件是否可以使用。判断之后将文件映射到内存，并设置内存映像保护。然后设置架构特定的线程状态。最终调用write的函数，向屏幕输出制定的字符串。    

 ```
  + 采分点：说明了strace的大致用途，说明了系统调用的具体执行过程（包括应用，CPU硬件，操作系统的执行过程）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.5 ucore系统调用分析
 1. ucore的系统调用中参数传递代码分析。
 1. ucore的系统调用中返回结果的传递代码分析。
 1. 以ucore lab8的answer为例，分析ucore 应用的系统调用编写和含义。
 1. 以ucore lab8的answer为例，尝试修改并运行代码，分析ucore应用的系统调用执行过程。
 
## 3.6 请分析函数调用和系统调用的区别
 1. 请从代码编写和执行过程来说明。
   1. 说明`int`、`iret`、`call`和`ret`的指令准确功能
 
