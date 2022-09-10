# Linux运维面试题

## 命令相关

### Linux 查看哪个服务程序占用了 8089 端口用什么命令？

> netstat –anpt | grep 8089

### 杀死 PID 为 4728 的进程用什么命令？

> kill -9 4728

### 找出当前目录以及子目录下文件名以 en 为首字母的文件用什么命令？

> find . -name "en* "

### Linux 下，为 filename.sh 脚本赋予可执行权限？

> chmod +x filename.sh

### 解压 gz 包、zip 包分别用什么命令和参数？

> tar -xf .gz -C /usr/local 
> unzip .zip

### 支持 rpm 包的 linux 环境，查询是否安装过 smile 包用何命令? 如安装过，卸载此包用何命令？

> rpm -q smile 
> rpm -e smile

### 写出 linux 下设置 jdk 环境变量的方法

> 环境变量配置文件位置：/etc/profile

vi /etc/profile添加内容 JAVA_HOME=/usr/local/java/jdk1.8.0_221 PATH=JAVAHOME/bin:PATH CLASSPATH=.:JAVAHOME/lib/dt.jar:JAVA_HOME/lib/tools.jar export JAVA_HOME export PATH export CLASSPATH

### systemctl disable firewalld 此命令的含义是， systemctl stop firewalld 此命令的含义是

> 永久关闭Firewalld防火墙；
>
> 后面命令是临时关闭

### 查看 docker 运行状态命令 构建一个镜像，镜像名为 swub,标签 v1 从 swub 镜像启动一个容器 swubsa，要求将容器的 8080 和 9527 端口分别映射到宿主机的 8080 和 9527 端口，主机文件夹 /home/lan/volume 容器卷/swdata

> 构建镜像：docker build 
> 增加镜像标签docker tag swub:v1
> docker run -itd --name swubsa -p 8080:8080 -p 9527:9527 -v /home/lan/volume:/swdata swub:v1

### nginx 如需设置两个服务地址的负载，要操作的配置文件名字是什么 ？启动 nginx 命令 关闭 nginx 命令

答：/etc/nginx/nginx.conf;systemctl start nginx;systemctl stop nginx

### 1、Linux常见的日志文件都有哪些，各自的用途？日志轮询配置文件在哪里？欢迎界面配置文件在哪里？

答：/var/log/messages 内核及公共消息日志
/var/log/cron 计划任务日志
/var/log/dmesg 系统引导日志
/var/log/maillog 邮件系统日志
/var/log/secure 记录与访问限制相关日志
/etc/logrotate.d/ 此目录下是各服务的日志轮询配置文件
/etc/issue 登陆前欢迎界面 /etc/motd 或者 /etc/update-motd.d/ 登录后欢迎界面
Ubuntu默认就已经有一个动态的MOTD信息提示，在/etc/update-motd.d/下，centos可通过环境变量文件，将需要执行的命令或脚本添加到这些文件末尾，参考https://developer.aliyun.com/article/560280

### 2、简述 /etc/fstab 里面各字段的含义？

答：因为 mount 挂载在重启服务后会失效，所以需要将分区信息写到 /etc/fstab 文件中，使其永久挂载
磁盘分区 挂载目录 文件格式
/dev/sdb1 /mnt/david ext4 defaults 0 0

### 3、讲一下 Keepalived 的工作原理？

答：在一个虚拟路由器中，只有作为 MASTER 的 VRRP 路由器会一直发送 VRRP 通告信息，BACKUP 不会抢占 MASTER，除非它的优先级会更高。当 MASTER 不可用时(BACKUP收不到通告信息) 多台 BACKUP 中优先级最高的这台会被抢占为 MASTER ，这种抢占是非常快速的(<1s)，以保证服务的连续性为了安全性考虑，VRRP包使用了加密协议进行加密。BACKUP 不会发送通告信息，只会接收通告信息。

### 4、如何查看Linux系统每个ip的连接数？

```sh
netstat -n  | awk '/^tcp/ {print $5}' | awk -F: '{print $1}' | sort | uniq -c | sort -rn
```

### 5、ps aux 中 VSZ RSS 各代表什么意思？

答：VSZ：虚拟内存集，进程占用的虚拟内存空间
RSS：物理内存集，进程占用的实际内存空间

6、如何让 history 命令显示具体时间？
答：HISTTIMEFORMAT="%F %T"
export HISTTIMEFORMAT
重新开机后会还原，可以写到/etc/profile

7、请写出下列端口所运行的服务 21、22、23、111、2049、3306、6379？在UDP/TCP协议中端口的范围？centos6如何禁用telnet服务？
答：21:ftp 22:ssh 23:telnet 111:rpcbind 2049:nfsd 3306:mysql 6379:redis
端口范围：0~65535
禁用telnet服务:chkconfig telnet off

8、将 /test/a 目录建立软链接到 /test/b 目录，请写出完整的操作命令。
创建软链接

ln -s /test/a  /test/b
1
删除软链接

rm -rf /test/b  ##后面千万不要加/,如果是 rm -rf /test/b/  那么会直接删除/test/a/下的文件。
1
9、shell 脚本中 continue 命令的作用？
答：continue 命令不同于 break 命令，它只是跳出当前循环的迭代，而不是整个循环。

10、简述Linux系统开机启动顺序
答：1.加载BIOS的硬件信息，获取第一个启动设备
2.读取第一个启动设备MBR的引导加载程序(grub)的启动信息
3.加载核心操作系统的核心信息，核心开始解压缩，并尝试驱动所有的硬件设备
4.核心执行init程序，并获取默认的运行信息
5.init程序执行/etc/rc.d/rc.sysinit文件
6.启动核心的外挂模块
7.init执行运行的各个批处理文件(scripts)
8.init执行/etc/rc.d/rc.local
9.执行/bin/login程序，等待用户登录
10.登录之后开始以Shell控制主机

### 11、什么叫网站灰度发布？

答：灰度发布是指在黑与白之间，能够平滑过渡的一种发布方式，AB test 就是一种灰度发布方式，让一部分用户继续使用A，一部分用户开始使用B，如果用户对B没有什么反对意见，那么逐渐扩大范围，把所有用户都迁移到B上，灰度发布可以保证整体系统的稳定性，在初始灰度的时候就可以发现、调整问题，以保证其影响性。

### 12、当文件系统受到破坏时，如何检查和修复系统？

答：成功修复文件系统的前提是要有两个以上的主文件系统，并保证在修复之前首先卸载将被修复的文件系统。
使用命令 fsck 对受到破坏的文件系统进行修复。fsck 检查文件系统分为5步，每一步检查系统不同部分的连接特性并对上一步进行验证和修改。在执行 fsck 命令时，检查首先从超级块开始，然后是分配的磁盘块、路径名、目录的连接性、链接数目以及空闲块链表、inode。

### 13、如何调试shell脚本？

答：
检测脚本中的语法错误
bash -n /path/to/some_script
调试执行
bash -x /path/to/some_script

### 14、shell 下生成32位随机密码

```
cat /dev/urandom | head -1 | md5sum | head -c 32 >> /pass
```

### 15、如何用 tcpdump 嗅探 80 端口的访问看看谁最高？

tcpdump -i eth0 -tnn dst port 80 -c 1000 | awk -F"."  '{print $1"."$2"."$3"."$4"."}' | sort | uniq -c | sort -nr | head -5
1
16、简述网络文件系统NFS，并说明其作用。
答：网络文件系统是应用层的一种应用服务，它主要应用于Linux和Linux系统、Linux和Unix系统之间的文件或目录的共享。对于用户而言，可以通过NFS方便的访问远地的文件系统，使之成为本地系统的一部分。采用NFS之后省去了登录的过程，方便了用户访问系统资源。

17、进程的查看和调度分别使用什么命令？
答：进程查看命令：ps 和 top
进程调度命令：at、crontab、batch、kill

18、Linux 如何挂载 windows 下的共享目录？

mount.cifs  //ip地址/server  /mnt/server -o user=admin,password=123456  
1
linux下的server需要自己手动建一个 后面的 user 和 pass 是 windows 主机的账号和密码，注意空格和逗号。

19、快速生成一个10G的文件

dd if=/dev/zero of=test bs=10M count=1024
1
20、ping 命令通过什么协议(internet 控制信息协议)来实现？
答：icmp

21、如何发布和回滚，用 jenkins 又是怎么实现？
答：发布：jenkins 配置好代码路径（SVN或GIT），然后拉代码，打tag。需要编译就编译，编译之后推送到发布服务器（jenkins 里面可以调脚本），然后从分发服务器往下分发到业务服务器上。

回滚：按照版本号到发布服务器找到对应的版本推送。

22、系统管理员的职责包括哪些？管理的对象是什么？
答：职责是进行系统资源管理、设备管理、系统性能管理、安全管理和系统性能监测。管理的对象是服务器、用户、服务器的进程及系统的各种资源等。

23、什么是静态路由，其特点是什么？什么是动态路由，其特点是什么？
答：静态路由是由系统管理员设计与构建的路由表规定的路由。适用于网关数量有限的场合，且网络拓扑结构不经常变化的网络。其缺点是不能动态的适用网络状况的变化，当网络状况变化后必须由网络管理员修改路由表。
动态路由是由路由选择协议而动态构建的，路由协议之间通过交换各自所拥有的路由信息实时更新路由表的内容。动态路由可以自动学习网络的拓扑结构，并更新路由表。其缺点是路由广播更新信息将占据大量的网络宽带。

24、“#!/bin/bash” 的作用
答：#!/bin/bash 是 shell 脚本的第一行，称为释伴(shebang)行。这里 # 符号叫做 hash ，而 ！ 叫做 bang 。 它的意思是命令通过 /bin/bash 来执行。

25、suid，sgid，stick bit 的作用？
答：suid：以属主的身份运行程序 比如查看/etc/shadow 这个是只允许root管理员查看的，但是用root用户执行chmod u+s /usr/bin/cat 切换普通用户也可以查看了
sgid：在目录中创建的文件继承所在目录的属主，root用户chmod g+s /dir 执行这条命令后，之后在/dir下创建的文件，无论是哪个用户，它的所属主都是root
stricky bit：用户只能修改、删除自己的文件。执行chmod o+t /dir 就算这个文件夹是777的权限，但是只有这个文件夹的所属者才能删除/dir

26、显示CPU利用率的命令，查看系统版本的命令
答：查看CPU利用率的命令：top或sar(centos不自带此命令需要安装sysstat包）
查看系统版本的命令：cat /etc/redhat-release 或 uname -a

27、关闭swap分区

swapoff -a  关闭所有的交换分区
swapoff /dev/sde  关闭sde交换分区
1
2
28、解释 i 节点在文件系统中的作用
答：在linux文件系统中，是以块为单位存储信息的，为了找到某一个文件在存储空间中存放的位置，用 i 节点对一个文件进行索引。i 节点包含了描述一个文件所必须的全部信息，所以 i 节点是文件系统管理的一个数据结构

29、什么叫数据的事务？
答：事务是应用程序中一系列严密的操作，所有操作必须成功完成，否则在每个操作中所作的所有更改都会被撤消。也就是事务具有原子性，一个事务中的一系列操作要么全部成功，要么一个都不做。
事务的结束有两种，当事务中的所有步骤全部成功执行时，事务提交。如果其中一个步骤失败，将发生回滚操作，撤消之前到开始事务的所有操作。

30、一个 ext3 的文件分区，当用 touch 新建文件时报错，错误信息是磁盘已满，但是用 df -h 查看分区信息时只用了50%，请分析具体原因？
答：两种情况，一种是磁盘配额问题，另外一种是 ext3 文件系统的设计不适合很多小文件和大文件的一种文件格式，出现很多小文件时，容易导致 inode 耗尽

31、Inode耗尽导致故障解决？

cd /tmp
sudo find /tmp -type f -exec rm {} \; #删除无用的临时文件，释放inode。
sudo find /home -type f -size 0 -exec rm {} \; #遍历寻找0字节的文件，并删除。
1
2
3
32、仅开放本机两个IP地址中的一个地址172.16.0.X上绑定的sshd和vsftpd服务给172.16.0.0/16网络中除了
172.16.0.0/24网络中的主机之外的所有主机，但允许172.16.0.200访问,每次的用户访问都要记录于日志文件
中，注：其中X为学号

vim /etc/hosts.allow
sshd,vsftpd@172.16.0.x:172.16.0.0/16 EXPECT 172.16.0.0/24 EXPECT 172.16.0.200:spawn echo
"`date` login  from %c to server %s,%d" >> /var/log/sshd.log
1
2
3
33、编写脚本/root/bin/checkip.sh，每5分钟检查一次，如果发现通过ssh登录失败次数超过10次，自动将此远程IP放入Tcp Wrapper的黑名单中予以禁止防问

#!/bin/bash
while true;do
cat /var/log/secure|awk '/Failed password/{ip[$(NF-3)]++}END{for(i in ip){if(ip[i]>=10)
{system("echo sshd:"i" >> /etc/hosts.deny" )}}}'
sleep 5m
done

### 34、为防止一个ip的恶意攻击，导致并发过大，请写出iptables命令防止一个ip的连接大于10，服务器ip为172.18.0.108

iptables -I FORWARD -d 172.18.0.108 -p tcp --dport 80 -m connlimit --connlimit-above 10 -j REJECT
35、如何查看二进制文件的内容？

hexdump -C XXX(文件名)    #十六进制和ASCII码显示
1
36、公司机房的服务器接近254台了，请你设计一个解决方案，如何划分网段，并实现业务平滑迁移。
答：第一种方案：变长子网掩码，加大ip地址的可用范围，全网分发 /etc/hosts 文件
第二种方案：增加核心交换机，在核心交换机划分VLAN，将新增的服务器加入新的VLAN中，全网分发 /etc/hosts 文件。

### 37、Linux 记录 log 的服务叫什么名字？

答：rsyslog

### 38、符号链接（软链接）与硬链接的区别是什么？

答：符号（或软）链接
1、一个符号链接指向另一个文件
2、一个符号链接的内容是它引用文件的名称
3、可以对目录进行
4、可以跨分区
5、指向的是另一个文件的路径
6、其大小为指向的路径字符串的长度
7、不增加或减少目标文件inode的引用计数
硬链接
1、创建硬链接会增加额外的记录项以引用文件
2、对应于同一文件系统上一个物理文件
3、每个目录引用相同的inode号
4、创建时链接数递增
5、删除文件时：rm命令递减计数的链接；文件要存在，至少有一个链接数；当链接数为零时，该文件被删除
6、不能跨越驱动器或分区

### 39、网络装机工具 cobbler 了解吗，有哪些组件？

答：批量装机：pxe

40、请解释下 Telnet 和 SSH 的区别
答：Telnet：不安全，没有对传输的数据进行加密，容易被监听还有遭受中间人攻击，不能压缩传输，传输速度较慢
SSH：对数据进行了RSA加密，安全性高，ssh传输数据是经过压缩的，所以数据传输速度比较快。

41、在1-39 内取随机数

expr  "$[RANDOM%39+1]"    #RANDOM 随机数  %39 取值数范围 0~38
1
42、常见的Linux开机设置文件
答：/etc/fstab：实现开机自动挂载设备的配置文件
/etc/inittab：定义开机进入默认级别的配置文件
/etc/rc.local：定义开机自定义任务的配置文件
/etc/init.d/rcS：定义开机自定义任务的配置文件

43、如何查看 http 的并发请求数与其TCP连接状态？

netstat -tan | awk '/^tcp/ {++state[$NF]} END {for(key in state) print key,"\t",state[key]}'
1
还有ulimit -n 查看linux系统打开最大的文件描述符，这里默认是1024，不修改这里 web 服务器修改再大也没用。修改 /etc/security/limits.conf
soft nofile 10240
hard nofile 10240 重启后生效

44、提取出字符串Yd$C@M05MB%9Bdh7dq+YVixp3vpw中的所有数字？
利用awk函数gsub取出所有数字

echo "yd$C@M05MB%9&Bdh7dq+yVixp3vpw" | awk 'gsub(/[^[:digit:]]/," ",$0)'
1
利用for脚本循环取出所有数字

echo "yd$C@M05MB%9&Bdh7dq+yVixp3vpw" | awk '
{
     string=$0
     len=length(string)
     for(i=0;i<=len;i++) 
     {
          tmp=substr(string,i,1)
          if(tmp ~ /[0-9]/) 
          { 
              str=tmp
              str1=(str1 str)
         }
     }
     print str1
}'

### 45、统计apaceh的access.log中访问量最多的5个ip？

cat access_log | awk  '{print $1}' | sort | uniq -c | sort -nr | head -5
1
46、⼯作⽇时间，每10分钟执⾏⼀次磁盘空间检查，⼀旦发现任何分区利⽤率⾼于80%，就执⾏wall警报。
利用 sed 提取分区利用率

vim diskwarning.sh
#!/bin/sh
[ `df | sed  -nr  '/dev\/sd/s/.* ([0-9]+)%.*/\1/p' |sort -nr | head -n1` -gt 80 ] && wall disk will be full

crontab -e
*/10 * * *  1-5  /bin/bash  diskwarning.sh

利用 grep 加 cut 加 tr 提取分区利用率

vim diskwarning.sh
#!/bin/sh
disknum=`df | grep "^/dev/sd" | tr -s " " % | cut -d% -f5 | sort -nr | head -1`
[ "$disknum" -gt 80 ] && wall disk will be full

crontab -e
*/10 * * *  1-5  /bin/bash  diskwarning.sh
1
2
3
4
5
6
7
47、查看/etc/my.cnf配置文件没有注释的文件，空行和注释的行全都过滤掉。

grep -v '#' /etc/my.cnf | grep -v '^$'
1
48、使用sed命令对文件指定行加注释并备份原文件，比如对文件/etc/my.cnf第13行到20行前面加#号，并备份/etc/my.cnf为/etc/my.cnf.bak。

sed -ri.bak '13,20s/.*/#&/g' /etc/my.cnf
1
49、如何在linux系统上查看BIOS版本、机器型号、序列号？查看⽹卡驱动版本？系统上查看载体为实体机（物理机）还是虚拟机？

 biosdecode命令 BIOS版本
 查看服务器型号： dmidecode | grep 'Product Name'
 查看主板的序列号： dmidecode |grep 'Serial Number'
 ethtool -i ens33 网卡驱动版本
 dmidecode |grep Product 查看载体为物理机还是虚拟机
1
2
3
4
5

### 50、简述linux下常⽤的⽂件系统有哪些，他们有什么区别？

1. EXT3
（1）最多只能支持32TB的文件系统和2TB的文件，实际只能容纳2TB的文件系统和16GB的文件
（2）Ext3目前只支持32000个子目录
（3）Ext3文件系统使用32位空间记录块数量和i-节点数量
（4）当数据写入到Ext3文件系统中时，Ext3的数据块分配器每次只能分配一个4KB的块
2. EXT4
EXT4是Linux系统下的日志文件系统，是EXT3文件系统的后继版本。
（1）Ext4的文件系统容量达到1EB，而文件容量则达到16TB
（2）理论上支持无限数量的子目录
（3）Ext4文件系统使用64位空间记录块数量和i-节点数量
（4）Ext4的多块分配器支持一次调用分配多个数据块
3. XFS
（1）根据所记录的日志在很短的时间内迅速恢复磁盘文件内容
（2）采用优化算法，日志记录对整体文件操作影响非常小
（3） 是一个全64-bit的文件系统，它可以支持上百万T字节的存储空间
（4）能以接近裸设备I/O的性能存储数据

### 51、Read-only File system错误与解决⽅法？

问题原因：系统没有正常关机，导致虚拟磁盘出现文件系统错误。
解决方法：先使用blkid命令查看文件系统，重启系统后使用root进入单用户模式，运行 fsck.ext4 -y /dev/sda

### 52、挂载本地yum源，起名为base。并实现开机自动挂载本地镜像源

mount -r /dev/cdrom /mnt
vim /etc/yum.repos.d/base.repo
[base]
name=base
baseurl=file:///mnt/
gpgcheck=0
enabled=1
先查看uuid，以下2个命令都可查看

lsblk -f
blkid
1
2

修改/etc/fstab文件，实现开机自动挂载本地镜像，和自己查找的对应起来。

以下为centos8的本地源配置，需要加上BaseOS路径，不然找不到repomd.xml文件，如果有错误可以查看日志/var/log/dnf.log


53、使用vim或者vi编辑文件，在前面显示行号，在家目录下设置变量，默认只对当前用户生效，如果想全局生效，可以写到/etc/profile下。

vim /root/.vimrc
set nu
保存退出，执行source /root/.vimrc 生效
1
2
3


54、centos7修改hostname为mail，并把主机名mail终端提示符改为紫色，使用history命令查看历史使用命令显示日期。

hostnamectl set-hostname mail
vim /etc/profile.d/env.sh
PS1="[\u@\[\e[1;35m\]\h\[\e[0m\] \W]\\$"
HISTTIMEFORMAT="%F %T"
保存退出，执行source /etc/profile.d/env.sh 生效
1
2
3
4
5


55、写一个脚本/root/mysqlbak.sh，备份mysql数据库，打成tar包放到/data/下，以备份时间命名，并只保留最近的2个tar包，做一个定时任务，每个月第一个周六的00:01执行/root/mysqlbak.sh。

备份mysql数据库到/data/下，并只保留/data/下最近的2个备份包，
chmod a+x /root/mysqlbak.sh 加上可执行权限

cat /root/mysqlbak.sh
#!/bin/bash
tar -zcvf /data/backupMysql-`date '+%Y%m%d-%H%M%S'`.tar.gz  /var/lib/mysql/*
ls -t /data/* | awk 'NR>2' | xargs rm -rf
1
2
3
4
每个月第一个周六的00:01执行/root/mysqlbak.sh，
systemctl restart crond 重启服务生效

cat /etc/crontab
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
  1  0  *  *  6  root    [ $(date +"\%d") -lt 7 ] && /root/mysqlbak.sh

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
56、如何将本地80端⼝的请求转发到8080端⼝，当前主机ip为192.168.16.1 ？

iptables -t nat -A PREROUTING -d 192.168.16.1 -p tcp --dport 80 -j DNAT --to-destination 192.168.16.1:8080
1
57、添加默认路由192.168.1.254

route add default gw 192.168.1.254
1
58、linux下清理系统缓存（cache）并释放内存。

sync
echo 3 > /proc/sys/vm/drop_caches 
1
2
drop_caches的值可以是0-3之间的数字，代表不同的含义：
0：不释放（系统默认值）
1：释放页缓存
2：释放dentries和inodes
3：释放所有缓存
1
2
3
4
5
59、Linux命令详解用法_hash
hash 缓存表 系统初始hash表为空，当外部命令执行时，默认会从PATH路径下寻找该命令，找到后会将这条命令的路径记录到hash表中，当再次使用该命令时，shell解释器首先会查看hash表，存在将执行之，如果不存在，将会去PATH路径下寻找，利用hash缓存表可大大提高命令的调用速率

hash                     #显示hash缓存
hash –l                  #显示hash缓存，可作为输入使用
hash –p path name        #将命令全路径path起别名为name
hash –t name             #打印缓存中name的路径
hash –d name             # 清除name缓存
hash –r                  # 清除缓存
1
2
3
4
5
6
60、chattr命令详解
A：即Atime，告诉系统不要修改对这个文件的最后访问时间。
S：即Sync，一旦应用程序对这个文件执行了写操作，使系统立刻把修改的结果写到磁盘。
a：即Append Only，系统只允许在这个文件之后追加数据，不允许任何进程覆盖或截断这个文件。如果目录具有这个属性，系统将只允许在这个目录下建立和修改文件，而不允许删除任何文件。
b：不更新文件或目录的最后存取时间。
c：将文件或目录压缩后存放。
d：当dump程序执行时，该文件或目录不会被dump备份。
D:检查压缩文件中的错误。
i：即Immutable，系统不允许对这个文件进行任何的修改。如果目录具有这个属性，那么任何的进程只能修改目录之下的文件，不允许建立和删除文件。
s：彻底删除文件，不可恢复，因为是从磁盘上删除，然后用0填充文件所在区域。
u：当一个应用程序请求删除这个文件，系统会保留其数据块以便以后能够恢复删除这个文件，用来防止意外删除文件或目录。
t:文件系统支持尾部合并（tail-merging）。
X：可以直接访问压缩文件的内容。
常用命令展示

chattr +i /etc/my.cnf     #加锁:不能删除、修改、改名/etc/my.cnf文件
lsattr /etc/my.cnf        #显示特定属性
chattr -i /etc/my.cnf     #解锁
1
2
3


61、如何查询文件夹下面哪些文件包含了特定字符，例如查询/var/下面哪些文件包含了error字符

grep  -nr  error  /var
1
62、centos如何修改网卡信息，请至少说出3种方法

1、修改配置文件 /etc/sysconfig/network-scripts/ifcfg-ens33
2、使用 ip 或者 ifconfig 或者 nmcli 命令修改
3、使用命令 nmtui 调用图形化界面修改，redhat可以用 setup 命令，调用图形化界面修改
1
2
3
63、docker中启动所有容器的命令
docker中启动所有容器的命令

docker start $(docker ps -a | awk '{ print $1}' | tail -n +2)
1
docker中停止所有容器的命令

docker stop $(docker ps -a | awk '{ print $1}' | tail -n +2)
1
docker中删除所有容器的命令

docker rm $(docker ps -a | awk '{ print $1}' | tail -n +2)
1
docker中删除所有镜像的命令

docker rmi $(docker images | awk '{print $3}' |tail -n +2)
1
docker进入容器名称nginx命令

docker exec -it nginx bash
1
列出docker容器，显示完整信息

docker ps -a --no-trunc
1
将系统的文件拷贝到docker容器中

docker cp 本地文件路径 容器ID/容器NAME:容器内路径    ##文件调换下位置即可把容器中的文件拷贝到系统 
1
获取docker容器nginx的元数据

docker inspect nginx
1
拉取完镜像后，如何对应镜像的dockerfile

docker histroy 镜像id
docker history --format {{.CreatedBy}} --no-trunc=true 镜像id |sed "s/\/bin\/sh\ -c\ \#(nop)\ //g"|sed "s/\/bin\/sh\ -c/RUN/g" | tac    ##详细查看
1
2
查看已运行容器的docker run参数

docker run --rm -v /var/run/docker.sock:/var/run/docker.sock nexdrew/rekcod 容器id
1
64、当前登录用户普通用户xsx ，怎么以管理员 root 来后台启动 docker 服务，例如启动docker命令为 /usr/bin/dockerd

su - root -c "/usr/bin/dockerd &"
1
65、centos如何修改默认运行级别(centos6及以下没有 systemctl 命令)，列出临时修改和永久修改的方法，比如现在默认开机是图形化界面，为节省内存，怎么换成字符界面。

运行级别：
0==> runlevel0.target, poweroff.target==> 关机模式
1 ==> runlevel1.target, rescue.target==>单用户模式
2 ==> runlevel2.target, multi-user.target==>多用户模式，无NFS
3 ==> runlevel3.target, multi-user.target==>多用户模式
4 ==> runlevel4.target, multi-user.target==>不使用的
5 ==> runlevel5.target, graphical.target==>图形化模式
6 ==> runlevel6.target, reboot.target==>重启模式

临时换成字符界面，下次开机默认还是图形化界面：

init 3
1
临时换成图形化界面：

init 5
1
关机：

init 0
1
重启：

init 6
1
永久换成字符界面，下次重启默认就是字符界面：

systemctl set-default multi-user.target
1
永久换成图形化界面，下次重启默认就是图形化界面：

systemctl set-default graphical.target
1
centos6及以下可以修改/etc/inittab文件来修改默认运行级别。

查看默认的运行级别：
查看文件方式

ll  /etc/systemd/system/default.target
1
使用 systemctl 命令查看

systemctl get-default
1
查看当前的运行级别：
使用 who 命令方式查看

who  -r
1
66、Ubuntu 18.04如何添加开机自启。比如开机执行 /usr/local/tomcat/bin/startup.sh 脚本。
buntu 18.04不再使用initd管理系统，改用systemd，包括用systemctl命令来替换了service和chkconfig的功能。
systemd 默认读取 /etc/systemd/system 下的配置文件，该目录下的文件会链接/lib/systemd/system/下的文件。
不同于以往的版本，ubuntu18.04默认不带/etc/rc.local文件，我们需要通过配置来让rc.local.service生效。
然后我们就可以像以前那样，直接把启动脚本写入/etc/rc.local文件，这样机器启动时就会自动运行它。
1、修改自启服务，增加 [Install] 项

sudo vim /lib/systemd/system/rc.local.service
1
[Unit]
Description=/etc/rc.local Compatibility
Documentation=man:systemd-rc-local-generator(8)
ConditionFileIsExecutable=/etc/rc.local
After=network.target

[Service]
Type=forking
ExecStart=/etc/rc.local start
TimeoutSec=0
RemainAfterExit=yes
GuessMainPID=no

[Install]
WantedBy=multi-user.target
Alias=rc-local.service

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
cp -a  /lib/systemd/system/rc.local.service  /etc/systemd/system/
1
chmod +x /etc/systemd/system/rc.local.service
1
2、创建 /etc/rc.local 文件，把脚本执行命令写入。

sudo vim /etc/rc.local
1
#! /bin/bash
sh  /usr/local/tomcat/bin/startup.sh
1
2
注意 rc.local 一定得加释伴(shebang)行 #! /bin/bash ，不然服务启动会有问题
3、给 /etc/rc.local 加上执行权限。

sudo chmod +x /etc/rc.local
1
4、启动 rc.local 服务,并执行开机自启 rc.local 服务。

sudo systemctl start rc.local && sudo systemctl enable rc.local
1
67、搭建了 tomcat 服务器，写了一个网页，访问路径为 http://192.168.190.178:8080/test/xsx.html ，如何只对 /test下的资源做账户密码验证？
1、在conf中编辑 tomcat-users.xml 文件，增加用户角色，用户名和密码。

<role rolename="user"/>
<user username="xsx" password="123456" roles="user"/>
1
2


2、在网站目录 WEB-INF 中编辑 web.xml 文件，在</web-app>上一行添加以下代码

<security-constraint>
<web-resource-collection>
<web-resource-name>/test/xsx.html</web-resource-name>
<url-pattern>/test/*</url-pattern>
</web-resource-collection>

<auth-constraint>
<role-name>user</role-name>
</auth-constraint>
</security-constraint>
<security-role>
<role-name>user</role-name>
</security-role>
<login-config>
<!-- <auth-method>BASIC</auth-method>-->
<auth-method>DIGEST</auth-method>
<realm-name>/test/xsx.html</realm-name>
</login-config>

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18


3、重启 tomcat 服务，再次访问 http://192.168.190.178:8080/test/xsx.html 需要账户密码验证


68、tomcat 服务 如何实现 https ？

属性	描述
-genkey	在用户主目录中创建一个默认文件".keystore",还会产生一个mykey的别名，mykey中包含用户的公钥、私钥和证书
-alias	产生别名
-keystore	指定密钥库的名称(产生的各类信息将不在.keystore文件中)
-keyalg	指定密钥的算法 (如 RSA DSA（如果不指定默认采用DSA）)
-validity	指定创建的证书有效期多少天
-keysize	指定密钥长度
-storepass	指定密钥库的密码(获取keystore信息所需的密码)
-keypass	指定别名条目的密码(私钥的密码)
-dname	指定证书拥有者信息 例如： “CN=名字与姓氏,OU=组织单位名称,O=组织名称,L=城市或区域名称,ST=州或省份名称,C=单位的两字母国家代码”
-list	显示密钥库中的证书信息 keytool -list -v -keystore 指定keystore -storepass 密码
-v	显示密钥库中的证书详细信息
-export	将别名指定的证书导出到文件 keytool -export -alias 需要导出的别名 -keystore 指定keystore -file 指定导出的证书位置及证书名称 -storepass 密码
-file	参数指定导出到文件的文件名
-delete	删除密钥库中某条目 keytool -delete -alias 指定需删除的别 -keystore 指定keystore -storepass 密码
-printcert	查看导出的证书信息 keytool -printcert -file yushan.crt
-keypasswd	修改密钥库中指定条目口令 keytool -keypasswd -alias 需修改的别名 -keypass 旧密码 -new 新密码 -storepass keystore密码 -keystore sage
-storepasswd	修改keystore口令 keytool -storepasswd -keystore /root/xsx.keystore(需修改口令的keystore) -storepass 123456(原始密码) -new xsx(新密码)
-import	将已签名数字证书导入密钥库 keytool -import -alias 指定导入条目的别名 -keystore 指定keystore -file 需导入的证书
1、创建一个证书库

keytool -genkey -alias httpsweb -keypass 12345678  -keysize 1024 -keyalg RSA -validity 360 -keystore "/root/httpsweb.keystore"  -storepass 12345678
1

2、找到tomcat下server.xml 中添加以下代码，指定keystoreFile和keystorePass

属性	描述
clientAuth	如果设为true，表示Tomcat要求所有的SSL客户出示安全证书，对SSL客户进行身份验证
sslProtocol	指定套接字（Socket）使用的加密/解密协议，默认值为TLS，用户不应该修改这个默认值。
ciphers	指定套接字可用的用于加密的密码清单，多个密码间以逗号（,）分隔。如果此项没有设定，在默认情况下，套接字可以使用任意一个可用的密码。
keystorePass	不设置默认changeit
    <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true" maxThreads="150" scheme="https" secure="true" clientAuth="false" sslProtocol="TLS" />
          <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
               maxThreads="150" scheme="https" secure="true"
               clientAuth="false" sslProtocol="TLS" 
               keystoreFile="/root/httpsweb.keystore"
               keystorePass="12345678"  />
1
2
3
4
5
6


3、重启 tomcat 服务，访问测试

在httpsweb.keystore在导入一张证书https.crt

keytool -import -alias https -file https.crt -keystore httpsweb.keystore
1
证书查看

keytool -list -keystore httpsweb.keystore -rfc
1


69、如何让 tomcat 在网页上显示文件目录，如何修改 tomcat 默认图标？
tomcat 默认网页不会显示文件目录


修改 conf/web.xml 文件，搜索 listings，找到<param-value>true</param-value>，改为 true，重启 tomcat 后网页即可显示目录。

删除 webapps/ROOT/下 favicon.ico 文件，将自己要改的图标弄成 ico 格式上传上去，重启 tomcat 即可。


备注：如果想使用 tomcat 作为centos的yum源，除了将rpm包放到tomcat上，还需在提供yum源的相关路径上创建yum源（软件仓库）， 生成repodata 目录和数据，以我上面的环境为例。

createrepo  /usr/local/tomcat/webapps/ROOT/centos/7/os/x86_64/
1
其他机器base.repo配置

vim /etc/yum.repos.d/base.repo
[base]
name=base
baseurl=http://192.168.190.178:8080/centos/7/os/x86_64/
#baseurl=file:///mnt/
gpgcheck=0

