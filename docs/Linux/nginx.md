## 一、nginx简介

### 1.什么是 nginx 和可以做什么事情

Nginx 是高性能的 HTTP 和反向代理的web服务器，处理高并发能力是十分强大的，能经受高负 载的考验,有报告表明能支持高达 50,000 个并发连接数。

其特点是占有内存少，并发能力强，事实上nginx的并发能力确实在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。

### 2.Nginx 作为 web 服务器

Nginx 可以作为静态页面的 web 服务器，同时还支持 CGI 协议的动态语言，比如 perl、php 等。但是不支持 java。Java 程序只能通过与 tomcat 配合完成。Nginx 专为性能优化而开发， 性能是其最重要的考量,实现上非常注重效率 ，能经受高负载的考验,有报告表明能支持高 达 50,000 个并发连接数。
https://lnmp.org/nginx.html

### 3.正向代理

Nginx 不仅可以做反向代理，实现负载均衡。还能用作正向代理来进行上网等功能。 正向代理：如果把局域网外的 Internet 想象成一个巨大的资源库，则局域网中的客户端要访 问 Internet，则需要通过代理服务器来访问，这种代理服务就称为正向代理。

简单一点：通过代理服务器来访问服务器的过程 就叫 正向代理。
需要在客户端配置代理服务器进行指定网站访问

### 4.反向代理

反向代理，其实客户端对代理是无感知的，因为客户端不需要任何配置就可以访问。
我们只 需要将请求发送到反向代理服务器，由反向代理服务器去选择目标服务器获取数据后，在返 回给客户端，此时反向代理服务器和目标服务器对外就是一个服务器，暴露的是代理服务器 地址，隐藏了真实服务器 IP 地址。

### 5.负载均衡

> 增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器上的 情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负载均衡

​		客户端发送多个请求到服务器，服务器处理请求，有一些可能要与数据库进行交互，服务器处理完毕后，再将结果返回给客户端。

​		这种架构模式对于早期的系统相对单一，并发请求相对较少的情况下是比较适合的，成本也低。但是随着信息数量的不断增长，访问量和数据量的飞速增长，以及系统业务的复杂度增加，这种架构会造成服务器相应客户端的请求日益缓慢，并发量特别大的时候，还容易造成服务器直接崩溃。很明显这是由于服务器性能的瓶颈造成的问题，那么如何解决这种情况呢？

   我们首先想到的可能是升级服务器的配置，比如提高 CPU 执行频率，加大内存等提高机 器的物理性能来解决此问题，但是我们知道摩尔定律的日益失效，硬件的性能提升已经不能满足日益提升的需求了。最明显的一个例子，天猫双十一当天，某个热销商品的瞬时访问量 是极其庞大的，那么类似上面的系统架构，将机器都增加到现有的顶级物理配置，都是不能够满足需求的。那么怎么办呢？上面的分析我们去掉了增加服务器物理配置来解决问题的办法，也就是说纵向解决问题的办法行不通了，那么横向增加服务器的数量呢？这时候集群的概念产生了，单个服务器解决不了，我们增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器上的情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们所说的负载均衡。

![在这里插入图片描述](nginx.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMDM2NzU0,size_16,color_FFFFFF,t_70.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191014203303979.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMDM2NzU0,size_16,color_FFFFFF,t_70)



### 6.动静分离

为了加快网站的解析速度，可以把动态页面和静态页面由不同的服务器来解析，加快解析速 度。降低原来单个服务器的压力。

![在这里插入图片描述](nginx.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMDM2NzU0,size_16,color_FFFFFF,t_70-16569190686562.png)

## 二、Nginx 的安装(Linux:centos为例)

nginx安装时，用到的包，我都准备好啦，方便使用：
https://download.csdn.net/download/qq_40036754/11891855
本来想放百度云的，但是麻烦，所以我就直接上传到我的资源啦，大家也可以直接联系我，我直接给大家的。

### 1.准备工作

打开虚拟机，使用finallshell链接Linux操作系统
到nginx下载软件
http://nginx.org/

先安装其依赖软件，最后安装nginx。
依赖工具：**pcre-8.3.7.tar.gz， openssl-1.0.1t.tar.gz， zlib-1.2.8.tar.gz， nginx-1.11.1.tar.gz**。 我这里也提供下。

### 2.开始安装

都有两种方式，一种直接下载，第二种使用解压包方式。这里大多使用解压包方式。
我的安装路径：/usr/jayq/
安装pcre
yum install -y pcre pcre-devel

安装 openssl
yum -y install pcre  pcre-devel zlib  zlib-devel openssl openssl-devel

安装 zlib
yum install -y zlib zlib-devel

**安装 nginx **
1）、解压文件， 回到 nginx 目录下，
2）、./configure 完成后，
3）、执行命令： make && make install

### 3.运行nginx

安装完nginx后，会在 路径 /usr/local 下自动生成 nginx 文件夹。这是自动生成的。
进入这个目录：
cd /usr/local/nginx
目录内容如下：

![image-20220704151158627](nginx.assets/image-20220704151158627.png)

进入sbin文件夹,里面有两个文件：nginx 和 nginx.old。

- 执行命令：./nginx 即可执行
- 测试启动： ps -ef | grep nginx

已经启动。
查看nginx默认端口（默认为80），使用网页的形式测试，（像Tomcat一样。）

![image-20220704151627862](nginx.assets/image-20220704151627862.png)
- 进入目录查看端口：cd /usr/local/nginx/conf 下的 nginx.conf文件。

  这个文件也是nginx的配置文件。

### 4.防火墙问题

在 windows 系统中访问 linux 中 nginx，默认不能访问的，因为防火墙问题 

解决方法：

（1）关闭防火墙 

（2）开放访问的端口号，80 端口

- 查看开放的端口号

> firewall-cmd --list-all 

- 设置开放的端口号

> firewall-cmd --add-service=http –permanent 
> firewall-cmd --add-port=80/tcp --permanent 

重启防火墙

> firewall-cmd –reload 

## 三、 Nginx 的常用命令和配置文件

### 1.Nginx常用命令

#### a. 使用nginx操作命令前提

使用nginx操作命令前提：**必须进入到nginx的自动生成目录的下/sbin文件夹下。**
nginx有两个目录：
第一个：安装目录，我放在：

> /usr/feng/

第二个：自动生成目录：

> /usr/local/nginx/

#### b. 查看 nginx 的版本号

./nginx -v


#### c. 启动 nginx

./nginx


#### d. 关闭nginx

./nginx -s stop


#### e. 重新加载 nginx

在目录：/usr/local/nginx/sbin 下执行命令，不需要重启服务器，自动编译。

./nginx -s reload

### 2.Nginx配置文件

#### a. 配置文件位置

/usr/local/nginx/conf/nginx.conf

#### b. nginx 的组成部分

```conf
worker_processes  1;

events {
	worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
		
		server {
    	listen       80;
    	server_name  localhost;

    	location / {
        root   html;
        index  index.html index.htm;
    	}
    	
    	error_page   500 502 503 504  /50x.html;
    	
    	location = /50x.html {
        root   html;
    	}
		}
}
```

nginx 配置文件有三部分组成

- 第一部分：全局块

  从配置文件开始到 events 块之间的内容，主要会设置一些影响nginx 服务器整体运行的配置指令，主要包括配置运行 Nginx 服务器的用户（组）、允许生成的 worker process 数，进程 PID 存放路径、日志存放路径和类型以 及配置文件的引入等。
  比如上面第一行配置的：

> worker_processes  1;

这是 Nginx 服务器并发处理服务的关键配置，worker_processes 值越大，可以支持的并发处理量也越多，但是 会受到硬件、软件等设备的制约。

- 第二部分：events块
  比如上面的配置：

```
events {
    worker_connections  1024; // 每个 work process 支持的最大连接数为 1024
}
```

events 块涉及的指令主要影响 **Nginx 服务器与用户的网络连接**，常用的设置包括是否开启对多 work process 下的网络连接进行序列化，是否 允许同时接收多个网络连接，选取哪种事件驱动模型来处理连接请求，每个 word process 可以同时支持的最大连接数等。
上述例子就表示每个 work process 支持的最大连接数为 1024.
这部分的配置对 Nginx 的性能影响较大，在实际中应该灵活配置。

- 第三部分：

    http {
        include       mime.types;
        default_type  application/octet-stream;
        sendfile        on;
        keepalive_timeout  65;
    		server {
        	listen       80;
        	server_name  localhost;
    
        	location / {
            root   html;
            index  index.html index.htm;
        	}
        	error_page   500 502 503 504  /50x.html;
        	location = /50x.html {
            root   html;
        	}
        }
    }
    这算是 Nginx 服务器配置中最频繁的部分，代理、缓存和日志定义等绝大多数功能和第三方模块的配置都在这里。
需要注意的是：http 块也可以包括 http全局块、server 块。

- http全局块
  http全局块配置的指令包括文件引入、MIME-TYPE 定义、日志自定义、连接超时时间、单链接请求数上限等。
- server 块
  这块和虚拟主机有密切关系，虚拟主机从用户角度看，和一台独立的硬件主机是完全一样的，该技术的产生是为了 节省互联网服务器硬件成本。
  每个 http 块可以包括多个 server 块，而每个 server 块就相当于一个虚拟主机。
  而每个 server 块也分为全局 server 块，以及可以同时包含多个 locaton 块。
- 全局 server 块
  最常见的配置是本虚拟机主机的监听配置和本虚拟主机的名称或IP配置。
- location 块
  一个 server 块可以配置多个 location 块。
  这块的主要作用是基于 Nginx 服务器接收到的请求字符串（例如 server_name/uri-string），对虚拟主机名称 （也可以是IP 别名）之外的字符串（例如 前面的 /uri-string）进行匹配，对特定的请求进行处理。 地址定向、数据缓 存和应答控制等功能，还有许多第三方模块的配置也在这里进行。

## 四、 Nginx 反向代理 配置实例 1.1

#### 1.实现效果

打开浏览器，在浏览器地址栏输入地址 www.123.com，跳转到 liunx 系统 tomcat 主页 面中

#### 2.准备工作

（1）在 liunx 系统安装 tomcat，使用默认端口 8080，我这里8080被其他应用占用，所以我已修改端口为8081。在conf目录下的server.xml配置文件中，如下，将port改为 8081，其实下面也有类似的Connector 标签，但是要看protocol协议为HTTP/1.1的标签修改即可。

```html
<Connector port="8081" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="8443" />
```

tomcat 安装文件放到 liunx 系统中，解压。
Tomcat的路径：/usr/feng/apach-tomcat/tomcat8081下
进入 tomcat 的 bin 目录中，./startup.sh 启动 tomcat 服务器。
（2）对外开放访问的端口 （我这里不需要）

firewall-cmd --add-port=8080/tcp --permanent
firewall-cmd –reload
查看已经开放的端口号 firewall-cmd --list-all
（3）在 windows 系统中通过浏览器访问 tomcat 服务器
别忘了开启tomcat，在bin目录下，使用 命令：

./startup.sh

#### 3.访问过程的分析

![在这里插入图片描述](nginx.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMDM2NzU0,size_16,color_FFFFFF,t_70-16569207193684-16569207213096.png)

#### 4.具体配置

- 第一步 在 windows 系统的 host 文件进行域名和 ip 对应关系的配置

添加内容在 host 文件中

- 第二步 在 nginx 进行请求转发的配置（反向代理配置）

#### 5.最终测试

如上配置，我们监听 80 端口，访问域名为 www.123.com，不加端口号时默认为 80 端口，故 访问该域名时会跳转到 127.0.0.1:8081 路径上。在浏览器端输入 www.123.com 结果如下：



## 五、 Nginx 反向代理 配置实例 1.2

#### 1.实现效果

实现效果：使用 nginx 反向代理，根据访问的路径跳转到不同端口的服务中
nginx 监听端口为 9001，
访问 http://127.0.0.1:9001/edu/ 直接跳转到 127.0.0.1:8081
访问 http://127.0.0.1:9001/vod/ 直接跳转到 127.0.0.1:8082

#### 2.准备工作

- 第一步，两个tomcat端口和测试页面
  准备两个 tomcat，一个 8081 端口，一个 8082 端口。
  在**/usr/feng/apach-tomcat/下 新建tomcat8081和tomcat8082两个文件夹，将 Tomcat安装包 分别上传到两个文件夹，进行解压缩安装，8081的Tomcat只改一个http协议默认端口号** 就行，直接启动即可。
  这里需要改8082的端口号，需要修改三个端口，只修改一个端口号的话，是启动不了的，我已经测试过了（如果只修改http协议默认端口的话，8081和8082只会启动一个）。因为默认的都是8080（没有的直接创建文件夹，好多都是刚建的，与上面的第一个示例示例有点改动）
  tomcat8081 解压包，然后进入到 /bin 下 ，使用命令 ./startup 启动

tomcat8082
使用命令 编辑 文件 ：/conf/server.xml 文件
vim server.xml
修改后如下：
1、修改server 的默认端口，由默认8005->8091
2、修改http协议的默认端口，由默认的8080->8082

3、修改默认ajp协议的默认端口，由默认的8009->9001


并准备好测试的页面
写一个a.html页面，
tomcat8081的tomcat，放到目录 /webapp/vod 下，内容：
<h1>fengfanchen-nginx-8081!!!</h1>
1
tomcat8082的tomcat，放到目录 /webapp/edu 下，内容：

<h1>fengfanchen-nginx-8082!!!</h1>
1
测试页面


b. 第二步，修改 nginx 的配置文件
修改 nginx 的配置文件 在 http 块中添加 server{}
修改其中注释的就行。

修改成功后


开发的端口： nginx监听端口：8001，tomcat8081端口：8081，tomcat8082端口：8082。
测试结果


location 指令说明
该指令用于匹配 URL。
语法如下：

1、= ：用于不含正则表达式的 uri 前，要求请求字符串与 uri 严格匹配，如果匹配 成功，就停止继续向下搜索并立即处理该请求。
2、~：用于表示 uri 包含正则表达式，并且区分大小写。
3、~*：用于表示 uri 包含正则表达式，并且不区分大小写。
4、^~：用于不含正则表达式的 uri 前，要求 Nginx 服务器找到标识 uri 和请求字 符串匹配度最高的 location 后，立即使用此 location 处理请求，而不再使用 location 块中的正则 uri 和请求字符串做匹配。

注意：如果 uri 包含正则表达式，则必须要有 ~ 或者 ~*标识。

## 六、 Nginx 负载均衡 配置实例 2

1. 实现效果
浏览器地址栏输入地址 http://208.208.128.122/edu/a.html，负载均衡效果，平均 8081 和 8082 端口中

2. 准备工作
a.准备两台 tomcat 服务器
准备两台 tomcat 服务器，一台 8081，一台 8082
上面的反向代理第二个实例中已经配置成功了。但是需要添加点东西，如下哦。
b. 修改一处
在两台 tomcat 里面 webapps 目录中，创建名称是 edu 文件夹，在 edu 文件夹中创建 页面 a.html，用于测试。
由于第二个实例中，8082中有了 edu 的文件夹，所以只在8081 文件夹下创建即可。
然后使用在vod文件下使用命令：
cp a.html ../edu/
1
即可完成，
查看命令

cd ../edu/  # 进入到 edu 目录下
cat a.html  #查看内容
1
2
c. 测试页面
测试URL

http://208.208.128.122:8081/edu/a.html

http://208.208.128.122:8082/edu/a.html


3. 在 nginx 的配置文件中进行负载均衡的配置
修改了第一个示例的 配置


    upstream myserver {
        server 208.208.128.122:8081;
        server 208.208.128.122:8082;
    }
    server {
        listen       80;
        server_name  208.208.128.122;
    
        #charset koi8-r;
    
        #access_log  logs/host.access.log  main;
    
        location / {
            root   html;
            proxy_pass   http://myserver;
            #proxy_pass   http://127.0.0.1:8081;
            index  index.html index.htm;
    }
4. 最终测试
测试url

http://208.208.128.122/edu/a.html

4. nginx 分配服务器策略
随着互联网信息的爆炸性增长，负载均衡（load balance）已经不再是一个很陌生的话题， 顾名思义，负载均衡即是将负载分摊到不同的服务单元，既保证服务的可用性，又保证响应 足够快，给用户很好的体验。快速增长的访问量和数据流量催生了各式各样的负载均衡产品， 很多专业的负载均衡硬件提供了很好的功能，但却价格不菲，这使得负载均衡软件大受欢迎， nginx 就是其中的一个，在 linux 下有 Nginx、LVS、Haproxy 等等服务可以提供负载均衡服 务，而且 Nginx 提供了几种分配方式(策略)：

a. 轮询（默认）
每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器 down 掉，能自动剔除。
配置方式：

b. weight
weight 代表权重, 默认为 1,权重越高被分配的客户端越多

    upstream myserver {
        server 208.208.128.122:8081 weight=10;   #  在这儿
        server 208.208.128.122:8082 weight=10;
    }
    server {
        listen       80;
        server_name  208.208.128.122;
        location / {
            root   html;
            proxy_pass   http://myserver;
            index  index.html index.htm;
    }

c. ip_hash
ip_hash 每个请求按访问 ip 的 hash 结果分配，这样每个访客固定访问一个后端服务器

    upstream myserver {
    	ip_hash;							//  在这儿
        server 208.208.128.122:8081 ;   
        server 208.208.128.122:8082 ;
    }
    server {
        listen       80;
        server_name  208.208.128.122;
        location / {
            root   html;
            proxy_pass   http://myserver;
            index  index.html index.htm;
    }
    d. fair（第三方）
    fair（第三方），按后端服务器的响应时间来分配请求，响应时间短的优先分配。
    upstream myserver {					
        server 208.208.128.122:8081 ;   
        server 208.208.128.122:8082 ;
        fair; 														#  在这儿
    }
    server {
        listen       80;
        server_name  208.208.128.122;
        location / {
            root   html;
            proxy_pass   http://myserver;
            index  index.html index.htm;
    }


## 七、 Nginx 动静分离 配置实例 3

1. 什么是动静分离

Nginx 动静分离简单来说就是把动态跟静态请求分开，不能理解成只是单纯的把动态页面和 静态页面物理分离。严格意义上说应该是动态请求跟静态请求分开，可以理解成使用 Nginx 处理静态页面，Tomcat 处理动态页面。动静分离从目前实现角度来讲大致分为两种：

一种是纯粹把静态文件独立成单独的域名，放在独立的服务器上，也是目前主流推崇的方案；

另外一种方法就是动态跟静态文件混合在一起发布，通过 nginx 来分开。

通过 location 指定不同的后缀名实现不同的请求转发。通过 expires 参数设置，可以使 浏览器缓存过期时间，减少与服务器之前的请求和流量。具体 Expires 定义：是给一个资 源设定一个过期时间，也就是说无需去服务端验证，直接通过浏览器自身确认是否过期即可， 所以不会产生额外的流量。此种方法非常适合不经常变动的资源。（如果经常更新的文件， 不建议使用 Expires 来缓存），我这里设置 3d，表示在这 3 天之内访问这个 URL，发送 一个请求，比对服务器该文件最后更新时间没有变化，则不会从服务器抓取，返回状态码 304，如果有修改，则直接从服务器重新下载，返回状态码 200。

2. 准备工作
在Linux 系统中准备 静态资源，用于进行访问。

www文件夹中 a.html
<h1>fengfanchen-test-html</h1>
1
image 中的 01.jpg
我的照片哈！！！（自动忽略）

3. 具体配置
a. 在 nginx 配置文件中进行配置


4. 最终测试
a. 测试 image
http://208.208.128.122/image/
http://208.208.128.122/image/01.jpg
1
2



b. 测试 www
http://208.208.128.122/www/a.html 
1

## 八、 Nginx 的高可用集群

1. 什么是nginx 高可用

配置示例流程：

需要两台nginx 服务器
需要keepalived
需要虚拟IP
2. 配置高可用的准备工作
需要两台服务器 208.208.128.122 和 208.208.128.85
在两台服务器安装 nginx(流程最上面有)
第二台服务器的默认端口 改为 9001 ，运行并测试，如下：

在两台服务器安装 keepalived
2. 在两台服务器安装keepalived
a)安装：
第一种方式：命令安装

yum install keepalived -y

查看版本：

rpm -q -a keepalived
1
2
3
第二种方式：安装包方式（这里我使用这个）
将压缩包上传至：/usr/feng/
命令如下：

cd /usr/feng/
tar -zxvf keepalived-2.0.18.tar.gz
cd keepalived-2.0.18
./configure
make && make install
1
2
3
4
5
b) 配置文件
安装之后，在 etc 里面生成目录 keepalived，有文件 keepalived.conf 。
这个就是主配置文件。
主从模式主要在这个文件里配置。

完成高可用配置（主从配置）
a) 修改 keepalived.conf 配置文件
修改/etc/keepalived/keepalivec.conf 配置文件

global_defs { 
   notification_email { 
     acassen@firewall.loc 
     failover@firewall.loc 
     sysadmin@firewall.loc 
   } 
   notification_email_from Alexandre.Cassen@firewall.loc 
   smtp_server 208.208.128.122
   smtp_connect_timeout 30 
   router_id LVS_DEVEL 
} 

vrrp_script chk_http_port { 

   script "/usr/local/src/nginx_check.sh" 

   interval 2      #（检测脚本执行的间隔） 

   weight 2 

} 

vrrp_instance VI_1 {     
	state MASTER   # 备份服务器上将 MASTER 改为 BACKUP       
	interface ens192  //网卡     
	virtual_router_id 51   # 主、备机的 virtual_router_id 必须相同     
	priority 100     # 主、备机取不同的优先级，主机值较大，备份机值较小 
    advert_int 1 
    authentication { 
        auth_type PASS 
        auth_pass 1111 
    } 
    virtual_ipaddress {         
		208.208.128.50 // VRRP H 虚拟地址 
    } 
}

b) 添加检测脚本
在/usr/local/src 添加检测脚本

#!/bin/bash
A=`ps -C nginx –no-header |wc -l`
if [ $A -eq 0 ];then
    /usr/local/nginx/sbin/nginx
    sleep 2
    if [ `ps -C nginx --no-header |wc -l` -eq 0 ];then
        killall keepalived
    fi
fi

c) 开启nginx 和 keepalived
把两台服务器上 nginx 和 keepalived 启动 ：
启动 nginx：./nginx
启动 keepalived：systemctl start keepalived.service

85服务一样。

4. 最终测试
a)在浏览器地址栏输入 虚拟 ip 地址 192.168.17.50



b)把主服务器（192.168.17.129）nginx 和 keepalived 停止，再输入 192.168.17.50



## 九、 Nginx 的原理

### 1.mater 和 worker

- nginx 启动后，是由两个进程组成的。master（管理者）和worker（工作者）。

- 一个nginx 只有一个master。但可以有多个worker

![image-20220704155524684](nginx.assets/image-20220704155524684-16569213270407.png)

- 过来的请求由master管理，worker进行争抢式的方式去获取请求。

  ![在这里插入图片描述](nginx.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMDM2NzU0,size_16,color_FFFFFF,t_70-16569214000208-165692140739510.png)

  ![在这里插入图片描述](nginx.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMDM2NzU0,size_16,color_FFFFFF,t_70-165692142014911-165692142196813.png)

### 2.master-workers 的机制的好处

- 首先，对于每个 worker 进程来说，独立的进程，不需要加锁，所以省掉了锁带来的开销， 同时在编程以及问题查找时，也会方便很多。
- 可以使用 nginx –s reload 热部署，利用 nginx 进行热部署操作
- 其次，采用独立的进程，可以让互相之间不会 影响，一个进程退出后，其它进程还在工作，服务不会中断，master 进程则很快启动新的 worker 进程。当然，worker 进程的异常退出，肯定是程序有 bug 了，异常退出，会导致当 前 worker 上的所有请求失败，不过不会影响到所有请求，所以降低了风险。

### 3.设置多少个 worker

Nginx 同 redis 类似都采用了 io 多路复用机制，每个 worker 都是一个独立的进程，但每个进 程里只有一个主线程，通过异步非阻塞的方式来处理请求， 即使是千上万个请求也不在话下。每个 worker 的线程可以把一个 cpu 的性能发挥到极致。所以 worker 数和服务器的 cpu 数相等是最为适宜的。设少了会浪费 cpu，设多了会造成 cpu 频繁切换上下文带来的损耗。

- worker 数和服务器的 cpu 数相等是最为适宜

### 4.连接数 worker_connection

第一个：发送请求，占用了 woker 的几个连接数？

- 答案：2 或者 4 个

第二个：nginx 有一个 master，有四个 woker，每个 woker 支持最大的连接数 1024，支持的 最大并发数是多少？

- 普通的静态访问最大并发数是： worker_connections * worker_processes /2，

- 而如果是 HTTP 作 为反向代理来说，最大并发数量应该是 worker_connections * worker_processes/4。

  这个值是表示每个 worker 进程所能建立连接的最大值，所以，一个 nginx 能建立的最大连接 数，应该是 worker_connections * worker_processes。当然，这里说的是最大连接数，对于 HTTP 请 求 本 地 资 源 来 说 ， 能 够 支 持 的 最 大 并 发 数 量 是 worker_connections * worker_processes，如果是支持 http1.1 的浏览器每次访问要占两个连接，所以普通的静态访 问最大并发数是： worker_connections * worker_processes /2，而如果是 HTTP 作 为反向代 理来说，最大并发数量应该是 worker_connections * worker_processes/4。因为作为反向代理服务器，每个并发会建立与客户端的连接和与后端服 务的连接，会占用两个连接。

  ![在这里插入图片描述](nginx.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwMDM2NzU0,size_16,color_FFFFFF,t_70-165692154944314-165692155089516.png)

> 原文链接：https://blog.csdn.net/qq_40036754/article/details/102463099
>
> 有删改