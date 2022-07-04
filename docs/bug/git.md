# GIT 相关问题解决

### 1.push失败

#### a.Failed to connect to github.com port 443: Timed out

这个错误大致是说连接到github的时候超时了。那么该怎么解决呢？很简单，这个超时了无非就是你的代理出了点问题，不过好在git上用几个命令就能够很快搞定。

`git config --global --unset http.proxy`

`git config --global --unset https.proxy`

然后再push，就很nice!



### b.OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 10053                                 

git config --global http.sslVerify false

---

#### c.文件过大，无法push

#### d.     ` the remote end hung up unexpectedley                                     `    

有以下几种解决方案：
如果你的 git 是使用 http 协议的，改用ssh协议，再次 push 即可
运行 git remote set-url origin git@git.xxx.com:gitbook/notes.git
在 .git/config 文件中修改url为ssh协议url
[remote "origin"]
url =git@git.xxx.com:gitbook/notes.git
修改 git 缓存大小 (这里给出的是全局的,也可以设置该文件夹的,也可以从config文件中添加/修改大小)
git config --global http.postBuffer 524288000
强制推送
git push -u origin master -f

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

