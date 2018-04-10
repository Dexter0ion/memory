- 官方api: [pandas 0.20.3](http://pandas.pydata.org/pandas-docs/stable/api.html)

- `import pandas as pd`, 是pandas库约定的导入方式.

- pandas库的两种重要数据类型: `Series`类型(一维)和`DataFrame`类型(二维).

- `Series`对象的几种创建方式:

```python
# 不指定索引, 也会有默认的位置索引, 从0开始
pd.Series(range(5))
# 加上自定义索引, 会和默认索引共存
pd.Series([9, 8, 7, 6], ['a', 'b', 'c', 'd']) 
# 字典形式创建
pd.Series({'a':1, 'b':2, 'c':3}) 
# 根据索引选择字典中的值, 不存在返回NaN
pd.Series({'a':8, 'b':9, 'c':7}, ['c', 'b', 'd', 'a']) 
# 和numpy完美兼容
pd.Series(np.arange(3), ['one', 'two', 'three'])
```

- `DataFrame`对象的几种创建方式:
```python
# 用ndarray对象创建
pd.DataFrame(np.arange(10).reshape(2, 5))
# 用Series对象创建
dt = {
    'one': pd.Series([1, 2, 3], ['a', 'b', 'c']),
    'two': pd.Series([9, 8, 7, 6], ['a', 'b', 'c', 'd']),
}
pd.DataFrame(dt)
# 用字典组成的列表创建
lst = [{'one':1}, {'one':2}, {'one':3}]
pd.DataFrame(lst)
```

- `Series`对象的选取方式(位置索引和自定义索引不能混用):
```python
'''位置索引'''
s[1] # 选取第2行元素
s[1:9] # 选取第2到第9行元素
s[1::2] # 选取第2到最后, 每隔2行的元素

'''自定义索引'''
s['one'] # 选取one行的元素
s['one':'three'] # 选取one到three行元素
s[:'three':2] # 选取开头到three行, 每隔2行的元素

'''布尔索引'''
s[s<10] # 选取所有value小于10的s
s[s.between(10, 20)] # 选取所有value大于等于10, 小于20的s
```

- `DataFrame`对象的选取方式(位置索引和自定义索引不能混用):
```python
'''要选取列, 用这种方式, 快很多(但不支持切片)'''
df['xm'] # 选择xm这一列, 是Series类型
df[['xm']] # 选择xm这一列, 是DataFrame类型
df[['xm','csrq']] # 选择xm, csrq两列, 是DataFrame类型

'''位置索引'''
df.iloc[2] # 选择第二行所有数据, 是Series类型
df.iloc[[2]] # 选择第二行所有数据, 是DataFrame类型
df.iloc[:, 2] # 选择第二列所有数据, 是Series类型
df.iloc[:, [2]] # 选择第二列所有数据, 是DataFrame类型
df.iloc[:, 0:2] # 选择0到2列所有数据
df.iloc[[2,3], 0:2] # 选择2和3行, 0到2列所有数据
df.iat[1, 1] # 根据位置快速取出数据, 获取单个数据推荐这种方法

'''自定义索引'''
df.loc['top'] # 选择指定行数据, 是Series类型
df.loc[['top']] # 选择指定行数据, 是DataFrame类型
df.loc[:, 'xm'] # 选择指定列数据, 是Series类型(不推荐)
df.loc[:, ['xm']] # 选择指定列数据, 是DataFrame类型(不推荐)
df.loc[:, ['bj','xm']] # 选择多列数据(不推荐)
df.loc[:, 'bj':'xb'] # 选择多列之间所有数据, 列切片只能用这种方法
df.loc[['top','count'], 'bj':'xb'] # 选择指定行, 指定列数据
df.at['top', 'xm'] # 根据自定义索引快速取出数据, 获取单个数据推荐这种方法

'''布尔索引'''
# 选取所有出生日期大于等于1998年的数据, 这里是字符串比较
df[df['csrq']>='1998'] 
# 选取所有出生日期大于等于1997年小于1999年的数据
df[(df['csrq']>='1997')&(data['csrq']<'1999')]
# 选取所有出生日期大于等于1997年小于1999年的数据
df[df['csrq'].between('1997', '1999')]
# 选取所有出生日期大于等于1997年或者姓名为张三的数据
df[(df['csrq']>='1997')|(data['xm']=='张三')]
# 另一种选取方式(不推荐, 实测效率比上面低)
df[df.csrq>='1998'] 
# 选择字段值为指定内容的数据
df[df['xm'].isin(['张三','李四'])] 
```

- `Series`对象的使用实例:
```python
'''重要属性'''
s.values # 所有元素的value, ndarray类型
s.dtype # 元素类型
s.size # 元素的数量
s.itemsize # 每个元素的大小, 字节为单位
s.nbytes # 所有数据占用的大小, 字节为单位, 等于s.size*s.itemsize

'''常用方法'''
s.unique() # 返回一个去重后的ndarray数组
s.value_counts(dropna=False)  # 显示唯一值和计数, dropna=False代表不包含NaN
s.drop(0) # 删除第一行, 非原地
s.append(s) # 在行尾添加数据, s是Series对象
s.apply(func) # 将func作用在s所有值上
s.sort_values() # 根据s的value排序, 可传入ascending参数决定升序降序
s.sort_index() # 根据s的index排序, 可传入ascending参数决定升序降序
s.astype() # 改变类型, 非原地

'''str系列方法'''
s.str.xxx() # 和python原生字符串操作基本一致, 只不过作用于所有元素
s.str[1, -1] # 支持切片, 同样作用于所有元素

'''dt系列方法'''
# 先要用pd.to_datetime(s['time'])或s.astype('datetime64[ns]'), 
# 把Series对象元素类型转成datetime64[ns]类型
s.dt.date() # 取出年月日
s.dt.time() # 取出时间
s.dt.year() # 取出年
s.dt.month() # 取出月
s.dt.day() # 取出日
s.dt.hour() # 取出时
s.dt.minute() # 取出分
s.dt.second() # 取出秒

'''相关运算'''
s += 1 # 所有元素加1, 原地
# 其他符号的运算与此类似
```

- `DataFrame`对象的使用实例(大部分也适用Series类型):
```python
'''重要属性'''
df.values # 查看所有元素的value
df.dtypes # 查看所有元素的类型
df.index # 查看所有行名
df.index = ['总数', '不同', '最多', '频率'] # 重命名行名
df.columns # 查看所有列名
df.columns = ['班级', '姓名', '性别', '出生日期'] # 重命名列名
df.T # 转置后的df, 非原地

'''查看数据'''
df.head(n) # 查看df前n条数据, 默认5条
df.tail(n) # 查看df后n条数据, 默认5条
df.shape() # 查看行数和列数
df.info() # 查看索引, 数据类型和内存信息

'''数据统计'''
df.describe() # 查看数据值列的汇总统计, 是DataFrame类型
df.count() # 返回每一列中的非空值的个数
df.sum() # 返回每一列的和, 无法计算返回空, 下同
df.sum(numeric_only=True) # numeric_only=True代表只计算数字型元素, 下同
df.max() # 返回每一列的最大值
df.min() # 返回每一列的最小值
df.argmax() # 返回最大值所在的自动索引位置
df.argmin() # 返回最小值所在的自动索引位置
df.idxmax() # 返回最大值所在的自定义索引位置
df.idxmin() # 返回最小值所在的自定义索引位置
df.mean() # 返回每一列的均值
df.median() # 返回每一列的中位数
df.var() # 返回每一列的方差
df.std() # 返回每一列的标准差
df.isnull() # 检查df中空值, NaN为True, 否则False, 返回一个布尔数组
df.notnull() # 检查df中空值, 非NaN为True, 否则False, 返回一个布尔数组

'''数据处理'''
df.groupby(['nj', 'bj']) # 分组功能
df.astype({'xh':'int', 'csrq':'datetime64[ns]'}) # 改变指定列, 元素类型, 非原地
df.drop_duplicates('xh') # 根据xh去重, 默认保留第一个数据, 非原地
df.drop_duplicates() # 不传入参数, 所有列相同才会去重, 默认保留第一个数据, 非原地
df.sort_values(by='csrq') # 根据csrq排序, 默认升序, 非原地
df.sort_values(['col1', 'col2'], ascending=[True, False]) # ascending决定升序降序
df.replace(1,'one') # 用'one'代替所有等于1的值
df.replace([1,3], ['one','three']) # 用'one'代替1，用'three'代替3
df.fillna(x) # 用x替换df中所有的空值
df.dropna() # 删除所有包含空值的行
df.dropna(axis=1) # 删除所有包含空值的列

'''添加和删除'''
df['cj'] = s # 假设cj列本来不存在, 这样会在列尾添加新的一列cj, 值为s(Series对象), 原地
df.insert(0, 'dz', s) # 在第1列位置插入一列dz(地址), 值为s, 原地
df.join(df2) # 在df中添加内容为df2(必须是DataFrame对象)的新列(添加列), 非原地
df.append(df2) # 将df2中的行添加到df的尾部(添加行), 非原地
df.pop('xm') # 删除单列, 并返回删除的列, 原地
df.drop(1) # 删除指定行, 非原地
df.drop(['xm', 'xh'], axis=1) # 删除指定列, axis=1指第2维, axis默认0, 非原地

'''相关运算'''
df['age'] += 1 # age列所有元素加1, 原地
# 其他符号运算与此类似
```

- 数据存取相关:
```python
'''sqlite3'''
db = sqlite3.connect('data.sqlite') # 先要连接当前目录下的sqlite3数据库, 不存在则创建
df.to_sql('table_name', index=False, con=db) # 导出数据到数据库指定表, 不包含索引
pd.read_sql('select * from table_name', con=db) # 从数据库指定表导入指定列数据

'''csv'''
df.to_csv('example.csv', index=False) # 导出数据到csv文件, 不包含索引
pd.read_csv('example.csv') # 从csv文件导入数据

'''excel'''
df.to_excel('example.xlsx', index=False) # 导出数据到excel文件, 不包含索引
pd.read_excel('example.xlsx') # 从excel文件导入数据
```

- pandas常用函数:
```python
# 将Series对象转换成时间类型, 可以用相关方法
pd.to_datetime(s) 
# 生成一个时间列表, periods决定数量, freq决定单位, 比如这里'D'是指天
# 成员是Timestamp类型, 可以使用相关方法
pd.date_range('2017-7-27', periods=15, freq='D') 
```