# SHELL脚本

> 引自 [1](https://blog.csdn.net/weixin_42313749/article/details/120524768?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522165699958516782248533722%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=165699958516782248533722&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-120524768-null-null.142^v30^pc_rank_34,185^v2^control&utm_term=shell&spm=1018.2226.3001.4187)
>
> 有删改

---

> Shell是一个命令行解释器，它为用户提供了一个向Linux内核发送请求以便裕兴程序的界面系统级程序，用户可以用Shell来启动、挂起、停止甚至是编写一些程序。

> shell 本质上就是linux命令的组合

## 1.执行方式

### 格式要求

1. 脚本以#!/bin/bash开头
2. 脚本需要可执行权限
2. shell命名: Shell脚本名称命名-般为英文、大写、小写，后缀以.sh结尾

### hello world

vim hello.sh

```shell
#!/bin/bash
echo "hello,world!"
```

### 执行方式

方式1（输入脚本的绝对路径或相对路径）

- 首先要赋予xx.sh脚本的+x权限：chmod 744 myShell.sh
- 执行脚本：./myShell.sh

方式2（sh+脚本）：

- 说明：不用赋予+x权限，直接执行即可
- sh ./myShell.sh

---

## 2.shell的变量

> Linux Shell的变量分为，系统变量，环境变量和用户自定义变量

### 变量基本语法

- 定义变量：变量=值

- 撤销变量：unset 变量

- 声明静态变量：readonly 变量，注意：不能unset

快速入门

```shell
#!/bin/bash
# 定义变量A
A=100
echo A=$A
echo "A=$A"
# 撤销变量A
unset A
echo "A=$A"
# 声明静态的变量B=2,不能unset
readonly B=2
echo "B=$B"
unset B
```

### 定义变量的规则

- 变量名称可以由字母、数字和下划线组成，但是不能以数字开头
- **等号两侧不能有空格**
- 变量名称一般习惯为大写

### 将命令的返回值赋给变量

- A=`ls -la`这里有反引号（ESC下面），运行里面的命令，并把结果返回给变量A
- A=$(ls -la)等价于上面

```shell
#!/bin/bash
# 将命令的返回值赋给变量
A=`date`
B=$(date)
echo "A=$A"
echo "B=$B"
```

### 系统变量

> Shell常见的变量之一系统变量，主要是用于对参数判断和命令返回值判断时使用

```bash
$0 		当前脚本的名称；
$n 		当前脚本的第n个参数,n=1,2,…9；
$* 		当前脚本的所有参数(不包括程序本身)；
$# 		当前脚本的参数个数(不包括程序本身)；
$? 		令或程序执行完后的状态，返回0表示执行成功；
$$ 		程序本身的PID号。
```

### 环境变量

> Shell常见的变量之二环境变量，主要是在程序运行时需要设置

```
PATH  		命令所示路径，以冒号为分割；
HOME  		打印用户家目录；
SHELL 		显示当前Shell类型；
USER  		打印当前用户名；
ID    		打印当前用户id信息；
PWD   		显示当前所在路径；
TERM  		打印当前终端类型；
HOSTNAME    显示当前主机名；
PS1         定义主机命令提示符的；
HISTSIZE    历史命令大小，可通过 HISTTIMEFORMAT 变量设置命令执行时间;
RANDOM      随机生成一个 0 至 32767 的整数;
HOSTNAME    主机名
```

### 用户自定义变量

> 常见的变量之三用户变量，用户变量又称为局部变量，主要用在Shell脚本内部或者临时局部使用

```
a=rivers 				       自定义变量A；
Httpd_sort=httpd-2.4.6-97.tar  自定义变量N_SOFT；
BACK_DIR=/data/backup/         自定义变量BACK_DIR；
IPaddress=10.0.0.1			   自定义变量IP1；
```

> 显示当前shell中所有变量：set

---

## 3.shell基本语法

### 运算符

- 在Shell中进行各种运算操作

- 基本语法

  - “`$`((运算式))”或“`$`[运算时]”
  - expr m + n，注意expr运算符间要有空格
  - expr m - n
  - expr    `\*`     /     %，乘，除，取余

- 案例

  计算 (2 + 3) * 4

  ```shell
  #!/bin/bash
  RES1=$(((2+3)*4))
  RES2=$[(2+3)*4)] #推荐
  TEMP= expr 2 + 3
  RES3= expr $TEMP \* 4
  ```

### 条件判断

- 基本语法：

  ```shell
  if [ condition ]
  #注意condition前后有空格！
  #空返回false,非空返回true，可使用$?验证（0为true，>1为false）
  ```

- 判断语句

  1. =：字符串比较
  2. 两个整数比较
     - -lt：小于
     - -le：小于等于
     - -eq：等于
     - -gt：大于
     - -ge：大于等于
     - -ne：不等于
  3. 按照文件权限进行判断
     - -r：有读的权限
     - -w：有写的权限
     - -x：有执行的权限
  4. 按照文件类型进行判断
     - -f：文件存在并且是一个常规的文件
     - -e：文件存在
     - -d：文件存在并且是一个目录

- 案例

  ```shell
  #!/bin/bash
  if [ "ok" = "ok" ]
  then 
  		echo "equal"
  
  if [ 23 -ge 22]
  then 
  		echo "大于"
  ```


### 流程控制

#### **if语句**

- 基本语法：

  ```shell
  # If条件判断语句，通常以if开头，fi结尾。也可加入else或者elif进行多条件的判断
  
  # 单分支语句 ---比较大小
  	if (条件表达式);then
  		语句1
  	fi
  
  # 双分支if 语句
  	if (表达式)
  		语句1
  	else
  		语句2
  	fi
  
  # 多支条件语句 ---判断成绩
  	if (表达式)
  		语句1
  	elif
  		语句2
  	elif
  		语句2
  	fi  
  ```
  
- 案例 - 判断crond服务是否存在

  ```shell
  #!/bin/bash
  # this is check crond
  
  # 定义一个变量名
  name=crond
  num=$(ps -ef|grep $name|grep -vc grep)
  if [ $num -eq 1 ];then
      echo "$num running!"
  else
      echo "$num is not running!"
  fi
  ```

  

#### **case语句**

- 基本语法

  ```shell
  #Case选择语句，主要用于对多个选择条件进行匹配输出，与if elif语句结构类似，通常用于脚本传递输入参数，打印出输出结果及内容，其语法格式以Case…in开头，esac结尾。语法格式如下：
  case 模式名  in
    模式 1)
      命令
      ;;
    模式 2)
      命令
      ;;
  *)
  不符合以上模式执行的命令
  esac
  # 每个模式必须以右括号结束，命令结尾以双分号结束。
  
  ```


- 案例 - 编写 http 启动脚本

```java
[root@web-server01~/script]# vim httpd_start.sh 
# check http server start|stop|starus
# by author rivers on 2021-9-27
while true
do
    echo -e "
    \033[31m start \033[0m
    \033[32m stop \033[0m 
    \033[33m status \033[0m
    \033[34m quit \033[0m 
"
read -p "请输入你的选择start|stop|quit：" char
case $char in
start)
    systemctl start httpd && echo "httpd服务已经开启" || echo "开启失败"
;;
stop)
    systemctl stop httpd && echo "httpd服务已经关闭" || echo "关闭失败"
;;
restart)
    systemctl restart httpd && echo "httpd服务已经重启" || echo "重启失败
"
;;
status)
    systemctl status httpd && echo -e "
        httpd 的服务状态
  
;;
quit)
```



#### for循环

- 基本语法

```shell
#格式：for name [ [ in [ word ... ] ] ; ] do list ; done
  for 变量名 in 取值列表; do
    语句 1
  done
```

- 案例1 - 检查同一局域网，多台主机是否存活

```shell
#!/bin/bash
# check hosts is on/Off

Network=$1
for Host in $(seq 1 254)
do
ping -c 1 $Network.$Host > /dev/null && result=0 || result=1

if [ "$result" == 0 ];then
  echo -e "\033[32;1m$Network.$Host is up \033[0m"
  echo "$Network.$Host" >> /tmp/up.txt

else
  echo -e "\033[;31m$Network.$Host is down \033[0m"
  echo "$Network.$Host" >> /tmp/down.txt
fi
done
```

#### while循环

>  While循环语句与for循环功能类似，主要用于对某个数据域进行循环读取、对文件进行遍历，通常用于需要循环某个文件或者列表，满足循环条件会一直循环，不满足则退出循环，其语法格式以while…do开头，done结尾与 

> while 关联的还有一个 until 语句，它与 while 不同之处在于，是当条件表达式为 false 时才循环，实际使用中比较少，这里不再讲解。

```shell
while  (表达式)
do
  语句1
done
```

- break和continue语句

```shell
# break 和 continue 语句
  break 是终止循环。
  continue 是跳出当前循环。
#示例 1：在死循环中，满足条件终止循环
while true; do
  let N++
  if [ $N -eq 5 ]; then
    break
fi
  echo $N
done
输出： 1 2 3 4

#示例 2：举例子说明 continue 用法
N=0
while [ $N -lt 5 ]; do
  let N++
if [ $N -eq 3 ]; then
  continue
fi
  echo $N
done
输出： 1 2 4

# 打印 1-100 数字
i=0
while ((i<=100))
do
        echo  $i
        i=`expr $i + 1`
done
```

- 案例 1 - 求 1 - 100 的总和

```shell
#!/bin/bash

j=0
i=1
while ((i<=100))
do
     j=`expr $i + $j`
     ((i++))
done
echo $j
```

- 案例 2 - 每 10 s 循环判断 hbs 用户是否登录系统

```shell
[root@web-server01~/script]# vim login.sh 
#!/bin/bash
#Check File to change. 
#By author rivers 2021-9-27
USERS="hbs"
while true
do
        echo "The Time is `date +%F-%T`"
        sleep 10
        NUM=`who|grep "$USERS"|wc -l`
        if [[ $NUM -ge 1 ]];then
                echo "The $USERS is login in system."
        fi
done
```

#### select 选择

```shell
#select 是一个类似于 for 循环的语句
#Select语句一般用于选择，常用于选择菜单的创建，可以配合PS3来做打印菜单的输出信息，其语法格式以select…in do开头，done结尾：

select i in （表达式） 
do
语句
done

# 选择mysql 版本
#!/bin/bash
# by author rivers on 2021-9-27
PS3="Select a number: "
while true; do
select mysql_version in 5.1 5.6 quit;
 do
  case $mysql_version in
  5.1)
    echo "mysql 5.1"
      break
      ;;
  5.6)
    echo "mysql 5.6"
       break
       ;;
  quit)
    exit
    ;;
  *)
    echo "Input error, Please enter again!"
      break
esac
 done
done
```

#### 函数

```shell
# Shell允许将一组命令集或语句形成一个可用块，这些块称为Shell函数，Shell函数的用于在于只需定义一次，后期随时使用即可，无需在Shell脚本中添加重复的语句块，其语法格式以function name（）{开头，以}结尾。

# Shell编程函数默认不能将参数传入（）内部，Shell函数参数传递在调用函数名称传递，例如name args1 args2。

# 函数语法
func() {
command1
command1
……
}
fun  # 直接调用函数名
# Shell 函数很简单，函数名后跟双括号，再跟双大括号。通过函数名直接调用，不加小括号。
#!/bin/bash
func() {
VAR=$((1+1))
return $VAR
echo "This is a function."
}
func
echo $?
# bash test.sh 
2

```

#### 数组

> 数组是相同类型的元素按一定顺序排列的集合。

```
格式：array=(元素 1 元素 2 元素 3 ...)
用小括号初始化数组，元素之间用空格分隔。
 定义方法 1：初始化数组 array=(a b c)
 定义方法 2：新建数组并添加元素 array[下标]=元素
 定义方法 3：将命令输出作为数组元素array=($(command))
```

---

## 4.Shell编程综合案例

### 案例1 - Tar工具全备，增量备份网站，实现系统自动备份

```shell
#!/bin/bash
#Auto Backup Linux System Files
#by author rivers on 2021-09-28

SOURCE_DIR=(
    $*
)
TARGET_DIR=/data/backup/
YEAR=`date +%Y`
MONTH=`date +%m`
DAY=`date +%d`
WEEK=`date +%u`
A_NAME=`date +%H%M`
FILES=system_backup.tgz
CODE=$?
if
    [ -z "$*" ]；then
    echo -e "\033[32mUsage:\nPlease Enter Your Backup Files or Directories\n--------------------------------------------\n\nUsage: { $0 /boot /etc}\033[0m"
    exit
fi
#Determine Whether the Target Directory Exists
if
    [ ! -d $TARGET_DIR/$YEAR/$MONTH/$DAY ]；then
    mkdir -p $TARGET_DIR/$YEAR/$MONTH/$DAY
    echo -e "\033[32mThe $TARGET_DIR Created Successfully !\033[0m"
fi
#EXEC Full_Backup Function Command
Full_Backup()
{
if
    [ "$WEEK" -eq "7" ]；then
    rm -rf $TARGET_DIR/snapshot
    cd $TARGET_DIR/$YEAR/$MONTH/$DAY ；tar -g $TARGET_DIR/snapshot -czvf $FILES ${SOURCE_DIR[@]}
    [ "$CODE" == "0" ]&&echo -e  "--------------------------------------------\n\033[32mThese Full_Backup System Files Backup Successfully !\033[0m"
fi
}
#Perform incremental BACKUP Function Command
Add_Backup()
{
   if
        [ $WEEK -ne "7" ]；then
        cd $TARGET_DIR/$YEAR/$MONTH/$DAY ；tar -g $TARGET_DIR/snapshot -czvf $A_NAME$FILES ${SOURCE_DIR[@]}
        [ "$CODE" == "0" ]&&echo -e  "-----------------------------------------\n\033[32mThese Add_Backup System Files $TARGET_DIR/$YEAR/$MONTH/$DAY/${YEAR}_$A_NAME$FILES Backup Successfully !\033[0m"
   fi
}
sleep 3 
Full_Backup；Add_Backup


```

### 案例2 - 服务器信息自动收集

```shell
cat <<EOF
++++++++++++++++++++++++++++++++++++++++++++++
++++++++Welcome to use system Collect+++++++++
++++++++++++++++++++++++++++++++++++++++++++++
EOF
ip_info=`ifconfig |grep "Bcast"|tail -1 |awk '{print $2}'|cut -d: -f 2`
cpu_info1=`cat /proc/cpuinfo |grep 'model name'|tail -1 |awk -F: '{print $2}'|sed 's/^ //g'|awk '{print $1,$3,$4,$NF}'`
cpu_info2=`cat /proc/cpuinfo |grep "physical id"|sort |uniq -c|wc -l`
serv_info=`hostname |tail -1`
disk_info=`fdisk -l|grep "Disk"|grep -v "identifier"|awk '{print $2,$3,$4}'|sed 's/,//g'`
mem_info=`free -m |grep "Mem"|awk '{print "Total",$1,$2"M"}'`
load_info=`uptime |awk '{print "Current Load: "$(NF-2)}'|sed 's/\,//g'`
mark_info='BeiJing_IDC'
echo -e "\033[32m-------------------------------------------\033[1m"
echo IPADDR:${ip_info}
echo HOSTNAME:$serv_info
echo CPU_INFO:${cpu_info1} X${cpu_info2}
echo DISK_INFO:$disk_info
echo MEM_INFO:$mem_info
echo LOAD_INFO:$load_info
echo -e "\033[32m-------------------------------------------\033[0m"
echo -e -n "\033[36mYou want to write the data to the databases? \033[1m" ；read ensure

if      [ "$ensure" == "yes" -o "$ensure" == "y" -o "$ensure" == "Y" ];then
        echo "--------------------------------------------"
        echo -e  '\033[31mmysql -uaudit -p123456 -D audit -e ''' "insert into audit_system values('','${ip_info}','$serv_info','${
cpu_info1} X${cpu_info2}','$disk_info','$mem_info','$load_info','$mark_info')" ''' \033[0m '
        mysql -uroot -p123456 -D test -e "insert into audit_system values('','${ip_info}','$serv_info','${cpu_info1} X${cpu_info2}
','$disk_info','$mem_info','$load_info','$mark_info')"
else
        echo "Please wait，exit......"
        exit
fi
```



- 需求分析

  1. 每天凌晨2：10备份数据库atguiguDB到/data/backup/db
  2. 备份开始和备份结束能够给出相应的提示信息
  3. 备份后的文件要求以备份时间为文件名，并打包成.tar.gz的形式，比如：2018-03-12_230201.tar.gz
  4. 在备份的同时，检查是否有10天前备份的数据库文件，如果有就将其删除。

- 如果报错：mysqldump: command not found

  解决方案：

  1. 先找到mysqldump的位置：find / -name mysqldump -print
  2. 然后建立一个链接：ln -fs /usr/local/mysql/bin/mysql /usr/bin

- crontab -e

- 10 2 * * * /usr/sbin/mysql_backup_db.sh



