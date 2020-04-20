如果没有安装requests，先使用下面的命令安装

```bash
pip install requests
```



### 发送请求

首先我们需要导入requests

```python
import requests
```

Get

```python
r.get('https://httpbin.org/get')
```

Post

```python
r = requests.post(' http://httpbin.org/post', data = {' key':' value'})
```

Put

```python
r = requests.put(' http://httpbin.org/put', data = {'key':' value'})
````

...



### 传递URL参数

比如说我们想要访问

```
httpbin.org/get?key=value
```

我们可以使用params来实现

```
payload={"key":"value"}

r = requests.get("httpbin.org/get", params=payload)

print(r.url)
# httpbin.org/get?key=value

```

value为None，key不会加到参数里面，如果一个key需要使用多次，对应key的 value可以使用列表

```python
payload = {'key1': 'value1', 'key2': [' value2', 'value3']}
```



###  响应内容

使用text属性即可获取

```python
import requests
r = requests.get(' https://api.github.com/events')

print(r.text)

# u' [{" repository":{" open_ issues":0," url":"
https://github.com/...

```

requests会自动的解码来自服务器的内容！大多数unicode字符集都可以被无缝 的解码

```python
r.encoding
```

获取text的编码方式主要是根据header里面的内容进行判断

如果需要进行更改，直接指定就可以

```python
r.encoding=r.apparent_encoding
```

获取二进制响应内容

```
r.content
```

获取json响应内容

```python
r.json()
```

以文本流的形式进行保存文件

```
with open(filename, 'wb') as fd:
	for chunk in r.iter_ content(chunk_ size):
	fd.write(chunk)
```



### 设置请求头

```python
# 配置请求头
headers = {'user-agent': 'my-app/ 0.0.1'}

# 指定请求头
r = requests.get(url, headers= headers)
```



### 使用post发送数据

```python
payload = {'key1': 'value1', 'key2': 'value2'}

r = requests.post(" http://httpbin.org/post", data= payload)

print(r.text)

"""
 {
...
"form": {
"key2": "value2",
"key1":"value1"
},
 ...
}

"""
```

你还可以为 data 参数传入一个元组列表,在表单中多个元素使用同一 key的时 候，这种方式尤其有效：

```python
payload = (('key1', 'value1'), ('key1', 'value2'))
r = requests.post(' http://httpbin.org/post', data= payload)
print(r.text)
"""
{
...
"form": {
"key1": ["value1","value2"]
},
...
}
"""

```

POST一个多部分编码(Multipart-Encoded)的文件

```python
url = 'http://httpbin.org/post'
files = {'file': open('report.xls', 'rb')}
r = requests.post(url, files=files)
print(r.text)
"""
{
...
"files": {
"file": "<censored...binary... data>"
},
...
}
""""
````

可以显式地设置文件名，文件类型和请求头

```python
files = {'file': ('report.xls', open(' report. xls', 'rb'), 'application / vnd. ms-excel', {' Expires': '0'})}
```



### 响应状态码

```python r = requests.get(' http://httpbin.org/get')
print( r.status_code )
# 200
```

为方便引用requests还附带了一个内置的状态码查询对象：

```python
r.status_code == requests.codes.ok
```

如果访问不成功，可以使用`raise_for_ status()`函数来抛出错误



### cookie

```python
url = 'http://httpbin.org/cookies'
cookies = dict(cookies_are=' working')
r = requests.get(url, cookies= cookies)
print( r.text)
# '{"cookies": {"cookies_are": "working"}}'

```



### 超时处理

你可以告诉 requests 在经过以timeout参数设定的秒数时间之后停止等待响应

```python
requests.get('http://github.com', timeout =10)
```



### 会话对象

会话对象让你能够跨请求保持某些参数。它也会在同一个Session 实例发出的所 有请求之间保持 cookie

```python
s=requests.Session()
s.get(http://httpbin.org/cookies/set/sessioncookie/123456')
s.close()
# 推荐使用
with requests.Session() as s:
	s.get(' http://httpbin.org/cookies/set/sessioncookie/123456789')

```



### 代理

如果需要使用代理，你可以通过为任意请求方法提供proxies参数来配 置单个请求:

```python
import requests
proxies = {"http": "http://10.10.1.10: 3128","https": "http://10.10.1.10: 1080", }
requests.get(" http://example.org", proxies=proxies)
```

如果你的代理需要认证可以使用`http://user:password@host/`语法：

```python
proxies = {"http":"http://user: pass@10.10.1.10:3128/", }
```

