# Python爬虫

## 爬虫简介

### 爬虫分类

通用爬虫：抓取整张页面信息

聚焦爬虫：抓取页面中特定的局部内容

增量式爬虫：抓取网站的更新数据

### robots协议

规定网站中哪些数据能被爬取，哪些可以爬取。查看方法为在网站域名后加`robots.txt`

## http/https协议

### http协议

常用请求头： -`User-agent`：请求载体的身份标识

​					-`Connection`：请求完毕后是否断开链接

响应头信息： -`Content-Type`：服务器响应回客户端的数据类型

### https协议（安全的超文本传输协议）

公钥加密  私钥解密

数据加密的方式：对称密钥加密

​						非对称密钥加密

·			  		证书密钥加密（https）

## requests模块

-`urllib`模块

-`request`模块：模拟浏览器发起请求

浏览器请求模拟：

- -指定url
- -发起请求
- 获取响应数据
- 持久化存储

## 数据解析

解析的局部文本内容会在标签之间或标签携带的属性中

标签定位-->解析内容

### 正则数据解析

### bs4数据解析

实例化`BeautifulSoup`对象，并将源码数据加载到该对象中 

调用`BeautifulSoup`对象中的属性或方法解析数据

对象实例化：1.本地html 2.网页源码 

bs4的方法：`soup.tagName` 返回第一个tagName对应的标签

​				`soup.find('')`返回第一次出现的 xx 标签

​				`soup.find('div',class_='song')`属性后必须带_

​				`findall('tagName',class_=)`

​				`soup.select('某种选择器id（前加#),class(前加.)，标签(不加)')` 返回值为寻找到的tag集合

层级选择器：>表示一个层级，空格表示多个层级（子代选择器与后代选择器）

获取标签中的文本数据：`soup.a.text/string/get_text()` 

text/get_text获取标签中的所有内容

string只获取直系的文本内容

获取标签中的属性值：`soup.a['href']`

### xpath解析

1.`etree`实例化

2.调用`etree`对象中的`xpath`方法结合`xpath`表达式获取数据

实例化过程： 1.`etree.parse(filePath(本地html))`

​				   2.`etree.HTML('page_text')`

`xpath`表达式：1.`.xpath('/html/body/xx')`    / 表示从根节点定位，一个 / 一个层级  ，

`/html//div`// 表示多个层级

`//div`表示从任意位置定位div

2.`//div[@class='song']`属性定位

3.`//div[@class='song']/p[3]`索引定位，注意索引是从1开始的

4.如何取文本和属性：`//div[@class=“song”]/p[3]/text()`注意双引号,取标签的子文本，返回值为列表

`//div[@class='song']/p[3]//text()`获取标签中的后代文本内容

`//div[@class='song']/p[3]/@src`取属性值

`./`局部标签出发使用`xpath`

5.`xpath`表达式可以使用逻辑运算符 `|`

#### 多页解析

改变请求的url，可以采用`url='https://sc.chinaz.com/jianli/free_%d.html'`类似的形式，其中的 `%d`代表页码，使用时`new_url = format(url1%pageNum)`

#### 下载zip/rar文件

类型为2进制数据，`get().content`即可

## 验证码

图鉴脚本接口

## cookie

`http/https`协议，服务器不记录状态，需要利用`cookie`记录当前用户状态

- 手动处理：通过抓包获取`cookie`并封装到`headers`中
- 自动处理：`session`会话对象 ：可进行请求发送，并自动储存生成的`cookie`可以使用`session`完成`post/get`请求

## 代理

应对服务器封 `ip`的行为

作用：突破自身IP访问的限制/隐藏自身真实IP

网站：快代理/西祠代理/goubanjia

代理 `ip`的类型：http/https

代理 `IP`的透明度： 透明 知道使用了代理及真实ip

​							匿名 知道使用了代理，未知ip

​							高匿名 不知道使用了代理，也不知道真实ip

## 异步爬虫

- 单线程串行
- 多线程、多进程：使阻塞操作异步执行，但无法无限制开启
- 进程池、线程池：确定数量的进程/线程：处理阻塞，耗时的操作

