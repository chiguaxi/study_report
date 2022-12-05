### 请求协议格式

![image-20220530144222458](C:\Users\silk\AppData\Roaming\Typora\typora-user-images\image-20220530144222458.png)

HTTP请求协议由首行、请求头（header）、空行、正文（body）组成。通过空行来区别header和body，body可有可无，若body存在，则在header中会定义一个content-length属性来表示body的长度。

![image-20220530141815593](C:\Users\silk\AppData\Roaming\Typora\typora-user-images\image-20220530141815593.png)

#### 1.首行

首行 = 方法 + URL + 版本号

##### 1.1 一个完整的URL包括：

协议：//主机名(域名):端口号/路径/查询字符串query string

```html
以一个比较复杂的URL做例：
https://www.example.com:80/path/to/myfile.html?key1=value1&key2=value2#anchor
```

- http:// 协议名
- www.example. 域名，等价于IP地址，能够表示一个网络上的主机
- com:80 端口号为80，一般情况下端口号都会省略。当端口号省略时，浏览器会根据协议类型自动决定使用哪个端口。默认情况下HTTP协议使用80端口，HTTPS协议使用443端口。
- /path/to/myfile.html 带层次的文件路径，用于定位到程序管理的资源。
- key1=value1&key2=value2#anchor 
？ 后面的都是查询字符串query string,本质是一个键值对结构，用于在请求中带上一些参数信息，传递个服务器。
- 片段标识，该URL中省略了片段标识，其主要用于页面内跳转。
以上URL中的各部分，皆可以省略。

##### 1.2 方法

HTTP首行的方法种类较多，但常用的只有前两个，以下只详细介绍GET和POST方法。

![image-20220530143218740](C:\Users\silk\AppData\Roaming\Typora\typora-user-images\image-20220530143218740.png)

#### 请求头

请求头是由若干个键值对的结构组成的，每个键值对独占一行，使用冒号分隔键与值，遇到空行表示header部分结束。

##### 2.1 HOST

HOST表示该请求对应的服务器的地址，地址可以是域名，可以是IP地址，也可以手动指定端口号。

##### 2.2 Content-Length

content-length表示body的长度，单位是字节。GET中一般没有body，所以抓包POST请求才能看到该header。

##### 2.3 Content-Type

content-type 表示body的格式。同上，只有该请求是POST时，才会带上content-length和content-type这来给你个header。

```
Content-Type 常见取值：
```

- application/x-www-form-urlencoded；
form 表单提交的数据格式. 此时 body 就是用类似于query string这样的格式来进行组织。
- multipart/form-data；
使用HTML时提交图片/文件时，会出现种格式。
- application/json；
数据为 json 格式,body格式形如下方的键值对形式。键值对之间使用 逗号 分割，键与值之间使用 冒号 分割。

### GET---最常见的一种请求方式

客户端从服务器中读取文档时，当点击网页上的链接或者通过在浏览器的地址栏输入网址来浏览网页的，使用的都是GET 方式。

GET方法要求服务器将URL定位的资源放在响应报文的数据部分，会送给客户端。

使用GET方法时，请求参数和对应的值附加在URL后面，利用一个问号（“?”）代表URL的结尾与请求参数的开始，传递参数长度受限制。

```html
例如，/index.jsp?id=100&op=bind,这样通过GET方式传递的参数直接表示在地址中
 
以用google搜索domety为例，Request报文如下：
 
GET /search?hl=zh-CN&source=hp&q=domety&aq=f&oq= HTTP/1.1
Accept: image/gif, image/x-xbitmap, image/jpeg, image/pjpeg, application/vnd.ms-excel, application/vnd.ms-powerpoint,application/msword, application/x-silverlight, application/x-shockwave-flash, */*
Referer: <a href="http://www.google.cn/">http://www.google.cn/</a>
Accept-Language: zh-cn
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; TheWorld)
Host: <a href="http://www.google.cn">www.google.cn</a>
Connection: Keep-Alive
Cookie: PREF=ID=80a06da87be9ae3c:U=f7167333e2c3b714:NW=1:TM=1261551909:LM=1261551917:S=ybYcq2wpfefs4V9g; NID=31=ojj8d-IygaEtSxLgaJmqSjVhCspkviJrB6omjamNrSm8lZhKy_yMfO2M4QMRKcH1g0iQv9u-2hfBW7bUFwVh7pGaRUb0RnHcJU37y-FxlRugatx63JLv7CWMD6UB_O_r
```

可以看到，GET方式的请求一般不包含”请求内容”部分，请求数据以地址的形式表现在请求行。地址链接如下：

```html
<a href="http://www.google.cn/search?hl=zh-CN&source=hp&q=domety&aq=f&oq=">http://www.google.cn/search?hl=zh-CN&source=hp&q=domety&aq=f&oq=</a> 
```

地址中“?”之后的部分就是通过GET发送的请求数据，在地址栏中可以看到，各个数据之间用“&”符号隔开。很显然，这种不适合传送私密数据。另外，由于不同的浏览器对地址的字符限制也有所不同，一般最多只能识别1024个字符，所以如果要传送大量数据的时候，也不适合使用GET方式。