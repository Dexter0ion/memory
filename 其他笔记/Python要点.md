# Python要点

### Python基本数据类型方法

- list(列表)

```python
# 将元素x添加到列表lst尾部
lst.append(x)
```

- dict(字典)

```python

```

- str(字符串)

```python

```

- int(整数)

```python

```

- float(浮点数)

```python

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

### Python值得注意的地方(想到哪写到哪)

- 把生成起转为列表后, 生成器会耗尽(走一轮), 不要把无限大小的生成器转换为列表!
- `a = b or c`, 如果`b`是`False`, 那么值为`or`右边那个子表达式的值.
- 函数参数的默认值, 会在每个模块加载进来时求出, 所以不要用非静态类型做函数参数默认值.

```python
# 每次调用时间都是一样的
def log(message, when=datetime.now()):
	print('%s: %s' % (when, message))
    
# 正确做法是, 令`when=None`, 在函数内部调用`when=datatime.now()`
```

- 调用函数时, 指定参数名可以让函数更明确, 用命名关键字参数强制要求指定参数名, 如:

```python
def person(name, age, *, city='Beijing', job)
```

- Python中有个内置变量`-`会存储上一次结果的值.
- 把字符串切割成列表的小技巧: 如切割`s = '1, 2, 3, 4, 5'`, 逗号后面有空格, 直接`s.split(',')`会留下空格, 其实可以用`s.replcae(',', ' ').split()`, 这样就不会有空格了.
- 如果要调用函数来保存状态, 那就应该定义新的类, 并实现其`__call__`方法, 而不要定义带状态的闭包.
- 总是应该使用内置的`super()`函数来初始化父类:

```python
Class Implicit(MyBaseClass):
    def __init__(self, value):
        super().__init__(value*2)
```

