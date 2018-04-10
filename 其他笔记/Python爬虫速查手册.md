# 常用HTTP状态码

**200 Ok**: 请求成功

**301 Moved Permanently**: 永久重定向

**302 Moved Temporarily**: 临时重定向

**303 See Other**: 资源url已经更新，和302的区别是，明确应当采用get请求该资源

**400 Bad Request**: 客户端请求有语法错误，不能被服务器理解

**401 Unauthorized**: 请求未经授权

**403 Forbidden**: 服务器收到请求，但拒绝提供服务

**404 Not Found**: 请求资源不存在

**500 Internal Server Error**: 表明服务器本身发生错误

**503 Server Unavailable**: 服务器当前不能处理客户端请求，一段时间后可能恢复

# 重要HTTP请求头

**Cookies**: 用于辨别用户身份、进行session跟踪

**Refer**: 表示从哪个网址访问当前网址的

**User-Agent**: 客户端系统, 浏览器等信息，可以用于区分PC端、移动端、和爬虫

# requests库

- get方法常用参数:
```python
params # dict, url参数, 会自动构建url
headers # dict, 定制向服务器提交的HTTP请求头
cookies # dict, 定制向服务器提交的cookie
timeout # int, 设置超时时间, 单位为秒
proxies # dict, 设置代理服务器
allow_redirects # bool, 默认True, 重定向开关, 如遇3XX, 自动跳转
stream # bool, 默认False, 是否立即下载请求内容, 请求视频等大内容时应设为True
```
- post方法常用参数(拥有get方法以上的所有参数，并增加了以下参数):
```python
data # dict, 你要想提交的表单, 如{"user": "hello", "passwd": "123"}
file # dict, 你要想提交的文件, 如{"file": open("1.txt", "rb")}
```

- 令`r = requests.get(url)`, 下面是Response对象的常用属性和方法:
```python
r.text  # 返回文本
r.content  # 返回二进制流
r.json() # 把r.text当作json解析，返回字典, 无法解析则抛出JSONDecodeError
r.encoding # 返回根据HTTP协议头判定的编码, 可以改写
r.apparent_encoding # 返回根据实际文本内容判定的编码, 实测会耗费性能
r.request.url # 最初请求的url
r.url # 最终请求的url, 如重定向后的url, 和参数中的url不一定相同
r.request.headers # 请求头
r.headers # 响应头，类型是不区分大小写的字典
r.cookies # 返回响应的cookies
r.history # 重定向历史
r.status_code # 返回http状态码
r.raise_for_status() # 如果返回的http状态码在400和500或500和600之间, 抛出HTTPError异常
```

- 如果不传入headers参数, requests会采用一个默认的headers, 为`{'User-Agent': 'python-requests/x.x.x', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive'}`. 传入headers参数后, 会在默认headers的基础上更新, 即字典的update()方法.

- `proxies`参数接受一个字典, 该字典一般有两个key, "http"和"https", 如果是https请求就会使用https对应的代理, 否则使用http对应的. 如果与协议对应的key不存在, 那么会不使用代理(即其实使用的是**你真实主机IP**), 这点很坑, 会让人误以为当前代理是可用的.

- `s = requests.Session()`创建一个会话对象, 支持上下文管理器. 会话对象让你能够跨请求保持某些参数, 它也会在同一个Session实例发出的所有请求之间保持cookie, 所以如果你向同一主机发送多个请求, 底层的 TCP 连接将会被重用, 从而带来显著的性能提升.

- `from requests.exceptions import RequestException`, 引入requests所有异常的基类, except该异常, 可以捕获所有requests的异常.

# bs4库

- 令`soup = BeautifulSoup(html, 'lxml')`, 下面是`BeautifulSoup`对象的常用属性和方法:
```python
soup.prettify() # 格式化html
soup.title # 直接打印标签，查找的是在所有内容中的第⼀个符合要求的标签
soup.new_tag('h2') # 创建一个新标签, 但还需选择一个tag插入
soup.select('xxx') # 我最喜欢用的定位标签的方法
```

- 下面是`select()`方法的使用实例, 返回的是一个包含`bs4.element.Tag`对象的**列表**:
```python
soup.select('div.main-title') # 选择所有<div class="main-title">标签
soup.select('#main-content') # 选择所有id="main-content"标签
soup.select('div a') # 选择所有div的后代a标签
soup.select('div > a') # 选择所有div的子代a标签(注意区分子代和后代)
soup.select('div.main > a.hello') # 选择所有div.main的子代a.hello标签
soup.select('[target=_blank]') # 选择所有target="_blank"的标签
soup.select('[title~=flower]') #选择所有title属性包含单词"flower"的标签
soup.select('[lang|=en]') # 选择所有lang属性值以"en"开头的标签
```

- 令`tag = soup.select('div')[0]`, 下面是`bs4.element.Tag`对象的常用属性和方法:
```python
tag.get_text() # 获取标签内所有文字(包括子标签的)
tag.get_text('|') # 将获取的文字用指定字符串分隔, 如这里的'|'
tag['href'] # 获取标签某一属性, 如href
tag.get('href') # 获取标签某一属性, 如href
tag.contents # 获取标签中的内容, 返回一个列表
tag.parent.name # 获取一个标签父亲名
tag.insert(1, other_tag) # 在标签内部的第1个位置插入other_tag标签
tag.decompose() # 销毁该标签
tag.select('[target=_blank]') # tag可以继续使用select方法
```

# re模块

- 正则语法:
```python
'[ ]'  # 字符集, 如[abc]表示a或b或c, [a-zA-Z0-9]表示所有数字和字母
'[^ ]' # 非字符集, 排除包含字符
'.'    # 匹配除换行符以外的任意字符
'\w'   # 匹配字母或数字或下划线或汉字
'\W'   # 匹配任意不是字母, 数字, 下划线, 汉字的字符
'\s'   # 匹配任意的空白符, 等价于 [\f\n\r\t\v]
'\S'   # 匹配任何非空白字符, 等价于 [^\f\n\r\t\v]
'\d'   # 匹配数字, 等价于 [0-9]

'{m}'  # 匹配前一个字符m次
'{m,n}'# 匹配前一个字符m到n次(包含n)
'\D'   # 匹配任意非数字的字符, 等价于 [^0-9]
'*'    # 匹配前面一个字符0到无限次
'+'    # 匹配前面一个字符1到无限次
'?'    # 匹配前面一个字符0或1次, 也可以代表非贪婪匹配

'|'    # 左右表达式任意一个, 如abc|def, 匹配abc或def
'()'   # 分组标记, 内部只能使用'|'操作符
'^'    # 匹配字符串开头
'&'    # 匹配字符串结尾
```
- re模块主要函数:
```python
# 如果一个正则表达式要用很多次, 应该先预编译, 会生成一个对象, 包含下列所有方法
re.compile() 
# 找到所有匹配内容, 返回一个列表
re.findall() 
# 替换字符串中所有匹配的内容, 返回替换后的字符串
re.sub() 
# 将字符串按匹配内容分割, 比str自带的spilt更灵活
re.spilt() 
```

- 实例, 一般加上r前缀,就不用考虑转义的问题了:
```python
# 匹配整数形式的字符串
re.compile(r'^-?\d+&') 
# 匹配中国境内邮编, 6位
re.compile(r'[1-9]\d{5}') 
# 匹配中文字符
re.compile(r'[\u4e00-\u9fa5]*') 
# 匹配国内电话号码
re.compile(r'\d{3}-\d{8}|\d{4}-\d{7}') 
# 精确匹配一个IP地址
re.compile(r'(([1‐9]?\d|1\d{2}|2[0‐4]\d|25[0‐5]).){3}([1‐9]?\d|1\d{2}|2[0‐4]\d|25[0‐5])')
```