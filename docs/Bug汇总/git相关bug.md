# GIT 相关问题解决

> 首先看是不是网的问题

> 重连/换个网络，我搞了半天结果发现是网有问题

---

### 1.push失败

#### a.Failed to connect to github.com port 443: Timed out

这个错误大致是说连接到github的时候超时了。那么该怎么解决呢？很简单，这个超时了无非就是你的代理出了点问题，不过好在git上用几个命令就能够很快搞定。

`git config --global --unset http.proxy`

`git config --global --unset https.proxy`

然后再push，就很nice!

### b.OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 10053                                 

git config --global http.sslVerify false

#### c.文件过大，无法push

改缓存值

#### d. the remote end hung up unexpectedley 

#### protocol error: bad line length character错误解决

删除凭证

### 2 clone 失败

总结

1. 先查config

   ​     `git config --global -l                                                        `    

2. 如果用户名邮箱不对，就修改

   修改用户名和邮箱地址：

   ```powershell
   $ git config --global user.name "用户名"
   
   $ git config --global user.email "邮箱"
   ```

