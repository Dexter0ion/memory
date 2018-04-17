### Python基本数据类型方法

- list(列表)

```python
# 将元素x添加到列表lst尾部
lst.append(x)
# 在下标index处添加元素x, 原来该位置元素及之后元素后移
lst.insert(index, x)
# 将列表L中元素添加到列表lst末尾, 原地
lst.extend(L)
# 删除并返回下标为index(默认-1)到元素
lst.pop(index)
# 删除列表中第一个值为x的元素, 其之后的元素前移
lst.remove(x)
# 清空列表, 但保留列表对象
lst.clear()
# 返回第一个值为x的元素下标, 不存在则抛出异常
lst.index(x)
# 返回元素x在列表中出现的次数
lst.count(x)
# 逆转列表, 原地
lst.reverse()
# 根据key排序, 可打开逆序开关, 原地
lst.sort(key=None, reverse=False)
# 返回列表的浅复制
lst.copy()
```

- dict(字典)

```python
# 返回一个元祖(键, 值)构成的列表
d.items()
# 返回字典的键构成的列表
d.keys()
# 返回字典的值构成的列表
d.values()
# 返回指定键的值, 不存在则返回default的值
d.get(key, default=None)
# 返回指定键的值, 不存在则添加键, 值为default
d.setdefault(key, default=None)
# 将iterable中的元素做键, 值都为default, 会覆盖已有的键
d.fromkeys(iterbale, default=None)
# 把字典dict2中的键值更新到字典d中, 会覆盖已有的键
d.update(dict2)
# 弹出指定元素, 并返回值, 如指定default, 则存不存在都返回default, 否则不存在抛出异常
d.pop(k, default=None)
# 弹出字典末尾元素, 返回一个二元组
d.popitem()
# 清空字典, 但会保留字典对象
d.clear()
# 返回一个字典的浅拷贝
d.copy()
```

- str(字符串)

```python
"""查找"""
# 返回子串sub在s中出现的次数, 可指定范围
s.count(sub, start=None, end=None)
# 返回子串sub的索引, 找不到返回-1
s.find(sub, start=None, end=None)
# 从右侧开始查找
s.rfind(sub, start=None, end=None)
# 与find一样, 只不过找不到会抛出异常
s.index(sub, start=None, end=None)
# 从右侧开始查找
s.rindex(sub, start=None, end=None)

"""替换"""
# 从左到右替换, count指定最大次数, 非原地
s.replace(old, new, count=None)
# 把字符串中的制表符转换为指定长度(默认8)的空格
s.expandtabs(tabsize=8)

"""删减"""
# 去除字符串两边的空白字符(空白字符由string.whitespace常量定义), 或删除指定字符
s.strip()
# 只删左边
s.lstrip()
# 只删右边
s.rstrip()

"""填充"""
# 居中对齐, width指定长度, fillchar指定填充字符, 不指定默认空白字符
s.center(width, fillchar)
# 左对齐
s.ljust(width, fillchar)
# 右对齐
s.rjust(width, fillchar)
# 右对齐, 前面用零填充
s.zfill(width)

"""分切"""
# 根据sep切割字符串, maxsplit指定最大次数, 返回列表. 默认切割空格
s.split(sep, maxsplit)
# 根据行分割, keepends决定是否保留换行符
s.splitlines(keepends)
# 将字符串s切割成三部分: sep前, sep, sep后
s.partition(sep)
# 从右往左分割
s.rpartition(sep)

"""连接"""
# 将可迭代对象(成员必须都是字符串, int也不行)连接成字符串, 成员之间填入s
s.join(iterable)

"""变形"""
# 把字符串中所有字母转小写
s.lower()
# 把字符串中所有字母转大写
s.upper()
# 字符串首字母转大写, 必须是第一个字符
s.capitalize()
# 把字符串中所有字母大写转小写, 小写转大写
s.swapcase()
# 每个单词的第一个字母转换成大写
s.title()

"""判定"""
# 是否都是数字或字母
s.isalnum()
# 是否都是字母
s.isalpha()
# 是否都是数字
s.isdigit()
# 如果包含字母, 是否都是小写
s.islower()
# 如果包含字母, 是否都是大写
s.isupper()
# 是否只包含空格
s.isspace()
# 是否是标题化的(首字母大小, 其他小写)
s.istitle()
# 字符串是否可以作为变量名
s.isidentifer()
# 字符串是否全部可打印, 包含不可打印字符(如转义字符), 返回False
s.isprintable()

"""编码"""
# 编码成bytes
s.encode(encoding='utf-8', errors='strict')
```

### Python内置函数

```python
# 如果iterable的每个元素是真值, 返回True; all([])返回True
all(iterable)
# 只要iterable中有元素是真值, 就返回True; any([])返回False
any(iterbale)
```

### Python常用魔法方法

```python
# 使用str()和print()会调用这个方法
__str(self)__
# 使用repr()会调用这个方法, 或交互环境显示
__repr__(self)
# 让类实例支持len()函数
__len__(self)
# 使类实例能像函数一样被调用
__call__(self)
# 让类实例能被下标访问
__getitem__(self, index)
# 当用户访问一个不存在的属性时, 可用这个方法定义行为
__getattr__(self, value)
# 在`self.name=value`时被调用
__setattr__(self, name, value)
# 限制实例到属性, 如不能指定name, age属性
__slots__ = ('name', 'age') 
```



