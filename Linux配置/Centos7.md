# VPS上centos7安装后配置

### 用户管理

```shell
passwd <用户> # 修改指定用户密码
useradd <用户> -m # 创建用户, -m自动创建家目录, 不加不自动创建.

# 给创建的用户添加root权限: 修改 `/etc/sudoers` 文件，找到`root ALL=(ALL) ALL`，在下面添加一行: `zzzzer ALL=(ALL) ALL`.
```


### 安全措施

- 关闭sestatus

```shell
vi /etc/selinux/config # 相关内容设为disabled，关闭selinux
sestatus # 查看selinux状态
```

- 防火墙

```shell
systemctl status firewalld # 查看防火墙状态
sudo firewall-cmd --get-active-zones # 查看已被激活的 Zone 信息
sudo firewall-cmd --zone=public --add-port=<port>/tcp --permanent # 开放指定端口, --permanent永久生效, 不然重启失效
sudo firewall-cmd --zone=public --remove-port=<port>/tcp --permanent # 关闭指定端口
sudo sudo firewall-cmd --zone=public --list-ports # 查看开放的端口.
sudo systemctl restart firewalld # 重启防火墙
```

### ssh

- 提高ssh连接速度

```shell
vi /etc/ssh/sshd_config # 设置`UseDNS`和`GSSAPIAuthentication`为no
```

- 修改默认ssh端口

```shell
vi /etc/ssh/sshd_config # 在`Port 22`下面添加`Port 端口号`, 并且把`#Port 22`的注释取消, 确认无误再注释回来, 别忘记防火墙把端口打开！

sudo systemctl restart sshd.service # 重启ssh
```

### 更换内核

```bash
sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
sudo rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
sudo yum --enablerepo=elrepo-kernel install kernel-ml -y
rpm -qa | grep kernel # 查看是否安装成功
sudo egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \' # 查看内核启动顺序
grub2-set-default 0  # default 0 表示第一个内核设置为默认运行, 选择最新内核就对了
sudo shutdown -r now # 重启
uname -r # 查看是否安装成功
```

### vps网络优化

- `modprobe tcp_hybla` 开启hybla算法, 适用高延迟的网络

- `modprobe tcp_htcp` 开启htcp算法, 适用低延迟的网络(如日本，香港等)

- `modprobe tcp_bbr` 开启bbr算法, bbr目的是要尽量跑满带宽

- `sysctl net.ipv4.tcp_available_congestion_control`查看开启的tcp拥塞算法

- 编辑`vi /etc/sysctl.conf`, 添加内容:

```bash
# 提高整个系统的文件限制
# max open files
fs.file-max = 65536

# max read buffer
net.core.rmem_max = 67108864
# max write buffer
net.core.wmem_max = 67108864
# default read buffer
net.core.rmem_default = 65536
# default write buffer
net.core.wmem_default = 65536
# max processor input queue
net.core.netdev_max_backlog = 4096
# max backlog
net.core.somaxconn = 4096

# resist SYN flood attacks
# 表示开启SYN Cookies。当出现SYN等待队列溢出时，启用cookies来处理，可防范少量SYN攻击，默认为0，表示关闭；
net.ipv4.tcp_syncookies = 1
# reuse timewait sockets when safe
# 表示开启重用。允许将TIME-WAIT sockets重新用于新的TCP连接，默认为0，表示关闭；
net.ipv4.tcp_tw_reuse = 1
# turn off fast timewait sockets recycling
# 表示开启TCP连接中TIME-WAIT sockets的快速回收，默认为0，表示关闭；
net.ipv4.tcp_tw_recycle = 0
# short FIN timeout
# 修改系統默认的 TIMEOUT 时间。
net.ipv4.tcp_fin_timeout = 30
# short keepalive time
# 表示当keepalive起用的时候，TCP发送keepalive消息的频度。缺省是2小时，改为20分钟。
net.ipv4.tcp_keepalive_time = 1200
# outbound port range
# 表示用于向外连接的端口范围。缺省情况下很小：32768到61000，改为10000到65000。（注意：这里不要将最低值设的太低，否则可能会占用掉正常的端口！）
net.ipv4.ip_local_port_range = 10000 65000
# max SYN backlog
# 表示SYN队列的长度，默认为1024，加大队列长度为8192，可以容纳更多等待连接的网络连接数。
net.ipv4.tcp_max_syn_backlog = 4096
# max timewait sockets held by system simultaneously
# 表示系统同时保持TIME_WAIT的最大数量，如果超过这个数字，TIME_WAIT将立刻被清除并打印警告信息。
net.ipv4.tcp_max_tw_buckets = 5000

# TCP receive buffer
net.ipv4.tcp_rmem = 4096 87380 67108864
# TCP write buffer
net.ipv4.tcp_wmem = 4096 65536 67108864
# turn on path MTU discovery
net.ipv4.tcp_mtu_probing = 1

# turn on TCP Fast Open on both client and server side
# 对于内核版本新于**3.7.1**的，我们可以开启tcp_fastopen：
net.ipv4.tcp_fastopen = 3

# for high-latency network
#net.ipv4.tcp_congestion_control = hybla
# for low-latency network, use htcp instead
#net.ipv4.tcp_congestion_control = htcp
# bbr
net.ipv4.tcp_congestion_control = bbr
net.core.default_qdisc=fq
```

- `sudo sysctl -p`使其生效
- 编辑`/etc/security/limits.conf`, 添加内容:

```bash
*               soft    nofile          65536
*               hard    nofile          65536
```

- 然后重启服务器执行`ulimit -n`，查询返回65536即可



### 必备依赖

```shell
# python3依赖
sudo yum install perl gcc gcc-c++ automake make openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel libffi-devel
# git依赖
sudo yum install perl-ExtUtils-MakeMaker libcurl-devel
```

### git

```bash
# 安装
sudo yum install git
# 配置git
git config --global user.name "zzzzer"
git config --global user.email "zzzzer91@gmail.com"
# 生成ssh密钥
ssh-keygen -t rsa -C "zzzzer91@gmail.com"
```

## pyenv

``` bash
# 安装
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
# 配置
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc
# 下载python
pyenv install 3.x.x
# 启动环境
pyenv local 3.x.x
# 更新pyenv
cd $(pyenv root)
git pull
```

### shadowsock

```shell
# 任何增加端口的行为都别忘记修改防火墙!!!

# 安装
pip install git+https://github.com/shadowsocks/shadowsocks.git@master

# 直接运行
ssserver -p 443 -k password -m aes-256-cfb # 前台
sudo ssserver -p 443 -k password -m aes-256-cfb --user nobody -d start # 后台
sudo ssserver -d stop # 后台终止

# 配置
echo 3 > /proc/sys/net/ipv4/tcp_fastopen # centos7开启fastopen功能
# 配置文件设置, `vi shadowsocks.json`
{
     "local_address": "127.0.0.1",
     "local_port":1080,
     "timeout":300,
     "method":"aes-256-cfb",
     "fast_open": true,
     "port_password":{
		"7788":"password0",
		"7789":"password1",
		"7790":"password2"
	}
}

# 读取配置文件
ssserver -c shadowsocks.json # 前台
sudo ssserver -c shadowsocks.json -d start # 后台
ssserver -d stop # 后台终止

# sudo出现command not found解决方法: 使用绝对路径

# 开机自启(要保证程序首先能运行!)
chmod +x /etc/rc.d/rc.local # 必须确保`/etc/rc.d/rc.local`可执行!!!
vi /etc/rc.d/rc.local # 添加你想要开机运行的程序, 如:`/home/zzzzer/.local/share/virtualenvs/ss-mYnvOCO8/bin/ssserver -c /home/zzzzer/Documents/code/python/ss/shadowsocks.json -d start`

# 检查日志
sudo less /var/log/shadowsocks.log 
```

### vim配置

```shell
set nu  # 使显示行号
set autoindent  # 自动缩进
set expandtab  # 将tab转化成空格
set ts=4  # 设置tab宽度
set softtabstop=4  # 按一次退格键会删除4个空格，只对tab按出来的有效
```

### 字体安装

```shell
# 1.安装相关程序
sudo yum install fontconfig mkfontscale 
# 2.把windows中字体copy到/usr/share/fonts目录下
# 3.然后执行
sudo mkfontscale
sudo mkfontdir
sudo fc-cache-fv
```

