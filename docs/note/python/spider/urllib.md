<!-- urllib -->

## 1.urllib简介

urllib是python内置的一个http请求库，不需要额外的安装。只需要关注请求的链接，参数，提供了强大的

解析。

## 2.主要模块

- urllb.request 

  请求模块

- urllib.error 

  异常处理模块

- urllib.parse 

  解析模块

## 3.用法

### (1) 简单的get请求

```python
import urllib.request
reponse = urllib.request.urlopen('http://www.baidu.com')
print(reponse.read().decode('utf-8'))
```



### (2) 简单的post请求

```python
import urllib.parse
import urllib.request
data = bytes(urllib.parse.urlencode({'hello':'world'}),encoding='utf-8')
reponse = urllib.request.urlopen('http://httpbin.org/post',data=data)
print(reponse.read())
```

### (3) 超时处理

```python
import urllib.request
import socket
import urllib.error
try:
	response = urllib.request.urlopen('http://httpbin.org/get',timeout=0.01)
except urllib.error.URLError as e:
	if isinstance(e.reason,socket.timeout):#判断错误原因
		print('time out!')
```

### (4) 打印响应类型，状态码，响应头

```python
import urllib.request
response=urllib.request.urlopen('http://www.baidu.com')
print(type(response)) # <class 'http.client.HTTPResponse'>

print(response.status) # 状态码 判断请求是否成功 200
print(response.getheaders()) # 响应头 得到的一个元组组成的列表
print(response.getheader('Server')) #得到特定的响应头
print(response.read().decode('utf-8')) #获取响应体的内容，字节流的数据，需要转成utf-8
格式
```

### (5) `urlopen`详解

由于使用`urlopen`无法传入参数，我们需要解决这个问题

我们需要声明一个request对象，通过这个对象来添加参数

```python
import urllib.request
request = urllib.request.Request('https://python.org') #由于urlopen无法传参数，声明一个Request对象
response = urllib.request.urlopen(request)
print(response.read().decode('utf-8'))
```

我们还可以分别创建字符串、字典等等来带入到request对象里面

```python
from urllib import request,parse
url='http://httpbin.org/post'
headers={
	'user-agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36',
'Host':'httpbin.org'}
dict={
	'name':'jay'
}
data = bytes(parse.urlencode(dict),encoding='utf-8')
req=request.Request(url=url,data=data,headers=headers,method='POST')
response=request.urlopen(req)
print(response.read().decode('utf-8'))
```

我们还可以通过addheaders方法不断的向原始的requests对象里不断添加

```python
from urllib import request,parse
url ='http://httpbin.org/post'
dict = {
	'name':'cq'
}
data=bytes(parse.urlencode(dict),encoding='utf-8')
req = request.Request(url=url,data=data,method='POST')
req.add_header('user-agent', 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36')
response=request.urlopen(req)
print(response.read().decode('utf-8')
```

### (6) cookies

下面这段程序获取response后声明的cookie对象会被自动赋值

```python
import http.cookiejar,urllib.request
cookie = http.cookiejar.CookieJar()
handerler=urllib.request.HTTPCookieProcessor(cookie)
opener=urllib.request.build_opener(handerler)
response=opener.open('http://www.baidu.com') #获取response后cookie会被自动赋值
for item in cookie:
	print(item.name+'='+item.value)
```

保存cookie文件,两种格式

```python
import http.cookiejar,urllib.request
filename='cookie.txt'
cookie = http.cookiejar.MozillaCookieJar(filename)
handerler=urllib.request.HTTPCookieProcessor(cookie)
opener=urllib.request.build_opener(handerler)
response=opener.open('http://www.baidu.com') #获取response后cookie会被自动赋值
cookie.save(ignore_discard=True,ignore_expires=True) #保存cookie.txt文件
```

用文本文件的形式维持登录状态

```python
import http.cookiejar,urllib.request
cookie = http.cookiejar.MozillaCookieJar()
cookie.load('cookie.txt',ignore_discard=True,ignore_expires=True)
handerler=urllib.request.HTTPCookieProcessor(cookie)
opener=urllib.request.build_opener(handerler)
response=opener.open('http://www.baidu.com')
print(response.read().decode('utf-8'))
```

### (7) 异常处理

关于异常处理部分，需要了解有**httperror**和**urlerror**两种，父类与子类的关系。

```python
#父类，只有一个reason
from urllib import request,error
try:
	response = request.urlopen('http://www.bai.com/index.html')
except error.URLError as e:
	print(e.reason)
    
#子类，有更多的属性
from urllib import request,error
try:
	response = request.urlopen('http://abc.123/index.html')
except error.HTTPError as e:
	print(e.reason,e.code,e.headers,sep='\n')
```

### (8) 解析

将一个url解析

```python
from urllib.parse import urlparse
result = urlparse('http://www.baidu.com/index.html;user?id=5#comment')
print(result)#协议内容、路径、参数
# ParseResult(scheme='http', netloc='www.baidu.com', path='/index.html', params='user', query='id=5', fragment='comment')
print(type(result))
# <class 'urllib.parse.ParseResult'>

result = urlparse('www.baidu.com/index.html;user?id=5#comment',scheme='https')

result = urlparse('http://www.baidu.com/index.html;user?id=5#comment',scheme='https')

result = urlparse('http://www.baidu.com/index.html;user?id=5#comment',allow_fragments=False) #会被拼接

result =urlparse('http://www.baidu.com/index.html#comment',allow_fragments=False) #会被拼接到path没有query
```

### (9) url拼接

```python
from urllib.parse import urlunparse
data=['http','www.baidu.com','index.html','user','a=6','comment']
print(urlunparse(data))
```
- 拼接两个url

```python
from urllib.parse import urljoin
print(urljoin('http://www.baidu.com','HAA.HTML'))
# http://www.baidu.com/HAA.HTML

# 如果前面相同，会跳过相同的部分
print(urljoin('https://wwww.baidu.com','https://www.baidu.com/index.html;question=2'))
# https://www.baidu.com/index.html;question=2 
```
- 字典方式直接转换成url参数

```python
from urllib.parse import urlencode
params = {
	'name':'germey',
	'age':'122'
}
base_url='http://www.baidu.com?'
url=base_url+urlencode(params)
print(url)
```

-------------------

更多细节可以参考：https://www.cnblogs.com/qikeyishu/p/10748497.html