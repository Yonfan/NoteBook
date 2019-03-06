# python爬虫整理（包含实例）
- 先放一波课程资源（来源：传智播客）：
> [六节课掌握python爬虫视频](https://pan.baidu.com/s/1QG9sFPZaR1JKs9KSpNpTKw )
> 提取码：po9s 

## 一、requests模块的学习

### 使用事前
    pip install requests

### 发送get，post请求，获取相应
- response = requests.get(url) #发送get请求，请求url地址对应的响应<br/>
实例：使用手机版的百度翻译：<br/>
- response = requests.post(url, data={请求体的字典}) #发送post请求，请求url地址对应的响应

### response的方法
- response.text 
    + 该方式往往会导致出现乱码，因为此时获取的是网页html的字符串，出现乱码使用response.encoding = "utf-8"
- response.content.decode()
    + 把响应的二进制字节流转换成str类型
- response.requests.headers #请求头
- response.headers          #响应头
- response.requests.url     #发送请求的地址
- response.url              #响应地址

### 获取网页源码的正确打开方式
-  1.response.content.decode()
-  2.response.content.decode("gbk")
-  3.response.text  #自动猜测编码

### 发送带headers的请求
- 为了模拟浏览器，获取和浏览器一模一样的内容

当不添加headers的时候会发现返回的内容只有一段，但是在添加了headers后就会发现返回的是整个网页的html
```python
headers = {
    "User-Agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) "
                  "Version/11.0 Mobile/15A372 Safari/604.1", "Referer": "https://fanyi.baidu.com/"}

requests.get(url, headers=headers)
```

- [百度翻译案例](https://github.com/Yonfan/Python/blob/master/12_spider/02_try_requests_post.py)

### 使用超时参数
- requests.get(url, headers=headers, timeout=3) #3秒内必须做出响应，否则就会报错


## 二、retrying模块的学习
pip install retrying
```python
from retrying import retry

@retry(stop_max_attempt_number=3)
def fun1():
    print("this is func1")
    raise ValueError("this is a test error") 
```
### 处理cookie相关的请求
- 人人网{"email":"your account",
        "password":"password"}
- 直接携带cookie请求url地址
    1. 可以把cookie放进headers中
```python
headers = {"User-Agent":"....","cookie":"cookie 字符串"}
```
    2. cookie字典传给cookies参数
        - requests.get(url, cookie=cookie_dict) #注意是字典而不是上面的字符串
- 先发送post请求，获取cookie（这个cookie在session中），带上cookie请求登录后页面
    - 1.session = requests.session() #session具有的方法与requests一样
    - 2.session.post(uel, data, headers) #服务器设置在本地的cookie会保存在session中
    - 3.session.get(url) #再次请求的时候会携带上之前保存在session中的cookie，能够请求成功
- [人人网请求登录案例](https://github.com/Yonfan/Python/blob/master/12_spider/03_try_login3.py)

## 三、数据提取方法
### json
- 数据交换格式，看起来像python类型(列表，字典)的字符串
- 使用json之前需要导入

- 哪里会返回json的数据
    + 把浏览器切换成手机版
    + 抓包app

- json.laods
    + 把json字符串转化成为python类型
    + json.loads（json字符串）
- json.dumps
    + 把python类型转换成json字符串
    + json.dumps({"a":1,"b":2})）
        * ensure_ascii：让中文显示成中文
        * indent：能够让下一行在上一行的基础上空几格
- [豆瓣电视爬虫案例](https://github.com/Yonfan/Python/blob/master/12_spider/06_DoubanSpider.py)


## 四、xpath和xml
- xpath
    + 一门从html中提取数据的语言
- xpath语法
    + xpath helper插件：帮助我们从`elements`中定位数据
    + 1.选择节点（标签）
        * `/html/head/meta`：当前能勾选中html下的所有meta标签
    + 2.`//tag`：能够从任意节点开始选择
        * `//li`：当前页面上的所欲li标签
        * `/html/head//link`：当前页面上head下所有的link标签
    + 3.`@`符号的用途
        * 选择具体某个元素`//ul[@class='btns']/li`选择class='btns'下面的ul下面的所有li标签
        * `a/@href`:选择a标签的href的值（当然也可以是其他标签的属性值）
    + 4.获取文本：
        * `/a/text()`：获取a标签的文本
        * `/a//text()`：获取a下的所有文本（包括a标签下的子标签中的文本值）
    + 5.当前
        * `./a`:当前节点下的a标签
- lxml
    + 安装：`pip install lxml`
    + 使用：
    ```python
    from lxml import etree
    element = etree.HTML("html字符串")
    element.xpath("")
    ```
- [豆瓣电影爬虫案例](https://github.com/Yonfan/Python/blob/master/12_spider/07_try_lxml.py)

## 五、基础知识点学习
- 列表推导式
    + 帮助我们快速生成包含一堆数据的列表<br/>
     `[i+10 for i in range(10)]` -->[10,11,12,...19]<br/>
     `["11月{}日".format(i) for i in range(30)]`-->["10月1日"，"10月2日",...]<br/>
- 字典推导式
    + 帮助我们生成包含一堆字典的数据<br/>
    ```python
    {i+10:i for i in range(10)} # {10:0,11:1...}
    {"a{}".format(i):10 for i in range(10)} # {"a0":10,"a1":10...}
    ```
- 三元运算符
- [糗事百科爬虫案例](https://github.com/Yonfan/Python/blob/master/12_spider/08_QiuBaiSpider.py)

## 六、写爬虫的讨论
- 1.URL
    + 知道url地址的规律和总页码数：构造url地址列表
    + 不知道url地址但是知道start_ur
- 2.发送请求获取响应
    + requests
- 3.提取数据
    + 返回json字符串：json模块
    + 返回的html字符串：lxml模块配合xpath提取数据
- 4.保存
    + 保存到数据库
    + 保存为文件 
- [新浪微博爬虫案例]()
