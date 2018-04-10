### gcc

- 编译工具链

```bash
# 头文件展开, 宏替换, 注释去掉(实际调用预处理器cpp)
gcc -E hello.c -o hello.i
# c文件变成汇编文件(实际调用编译器gcc)
gcc -S hello.i -o hello.s
# 汇编文件变成二进制文件(实际调用汇编器as)
gcc -c hello.s -o hello.o
# 将函数库中相应代码组合到目标文件中(实际调用链接器ld)
gcc hello.o -o hello
```

- 常用参数

```bash
-I # 指定头文件目录
-L # 指定动态库目录
-o # 后面加想要生成的文件名
-O # 后面紧跟数字1~3, 指定优化等级, 默认-O0
-g # 在生成程序中加入调试信息
-Wall # 输出所有警告信息
-D<宏名> # 相当于在.c文件中加了一句#define <宏名>
```

- 两步查看带源代码的汇编代码

```bash
gcc -c main.c -g # 生成带调试信息的.o文件
objdump -S main.o # 查看汇编代码
```

### 静态库

- 命名规则

```
lib + 库名 + .a; 如libtest.a, 库名就是test
```

- 制作步骤

```bash
# 生成对应的.o文件
gcc -c test.c -o <生成文件名>
# 将生成的.o文件打包
ar rcs <静态库名> <.o文件>
```

- 查看静态库

```bash
nm <静态库名> 
```

### 动态库

- 命名规则

```bash
lib + 库名 + .so; 如libtest.so, 库名就是test
```

- 制作步骤

```bash
# 生成与位置无关的.o文件
gcc -c test.c -o test.o -fPIC
# 将生成的.o文件打包
gcc -shared test.o -o libtest.so
```

- 设置全局调用

```bash
# 设置用户头文件位置
# 修改.bashrc, 添加下面一行
export C_INCLUDE_PATH=$HOME/Documents/share/include
# 编译时可不指定头文件目录(-I<目录>)

# 设置用户动态库位置
# 修改.bashrc, 添加下面两行
export LIBRARY_PATH=$HOME/Documents/share/mylib # gcc编译链接时查找路径
export LD_LIBRARY_PATH=$HOME/Documents/share/mylib # 运行时查找路径
# 编译时只需指定-l<动态库名>, 不需要指定-L<目录>
```

- 查看程序使用了哪些动态库

```bash
ldd <程序名>
```

### makefile

```bash
# makefile中必须用真正的tab, 可以在vim中`Ctrl+v tab`按出
```

- 例子

```makefile
CC=gcc # 指定编译器
CFLAGS=-g
target=app
src=$(wildcard src/*.c) # 查找指定目录下所有.c文件, 返回字符串
obj=$(patsubst %.c, %.o, $(src)) # 把字符串中的.c替换.o, 返回字符串

# 找不到.o依赖时, 自动执行下面语句, 逐个生成.o文件
$(target):$(obj)
	$(CC) $(obj) lib$(mylib).so -o $(target) -Wall $(CFLAGS)

# $@: 规则中的目标
# $<: 规则中的第一个依赖
# $^: 规则中的所有依赖
%.o:%.c
	$(CC) -c $< -o $@ -Wall

clean:
	rm src/*.o
```

### gdb

- 命令

```bash
l # 查看源代码
l <文件名:函数名> # 查看指定函数源代码
disas /m <函数名> # 显示汇编代码
b <行号> # 打断点
b <行号> if i==10 # 设置一个条件断点, 在变量i=15时停下
i b # 查看断点信息
d <断点编号> # 删除断点

run # 完整运行程序, 直至断点
start # 开始调试, 只执行一步, 在此期间:
	# n: 单步调试
	# ni: 汇编级别单步调试
	# s: 单步调试, 遇函数会进入
	# finish: 跳出函数
	# u: 跳出循环
	# c: 跑完程序

p <变量名> # 查看变量值
ptype <变量名> # 查看变量类型
display <变量名> # 追踪变量
i display # 显示追踪变量编号
undisplay <变量编号> # 取消追踪变量
set var <变量名>=10 # 设置某个变量值
i local # 显示栈中变量
x <内存地址>|$<寄存器名>|&<变量名> # 查看内存值
i r # 查看寄存器值

q # 退出gdb
```

- 调试动态库

```bash
# 1.生成带调试信息(gcc加-g参数)的.so文件
# 2.gdb中打断点在动态库的函数上, 要用`b <函数名>`, 会出现提示, 不能用`b <行号>`
# 3.`run`会自动进入函数内部, 或者`start`, 再`s`进入函数内部
```

- 追踪段错误

```bash
run # 会自动追踪段错误
```

### vim

- 实现函数跳转功能

```bash
# 1.安装.ctags
# 2.在指定目录下执行`ctags -R *`生成tags文件
# 3.在源码文件中, 将光标移至指定函数, 按:
#	Ctrl+]: 跳转到指定函数
#	Ctrl+o: 跳回原来位置
```

- 其他

```bash
K # 大K查看指定函数man文档
[ + d # 查看宏定义
gg=G # 格式化代码
```

### 文件信息

```bash
stat <file> # 查看文件详细信息
file <file> # 查看文件类型
xxd <file> # 查看文件16进制, 比od更好用
wc <文本文件> # 查看文本文件信息, 分别对应行数, 字数, 字节数
```