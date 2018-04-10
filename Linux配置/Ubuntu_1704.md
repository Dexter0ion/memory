## Ubuntu17.04安装后配置



### 删除不常用软件

```bash
# 杂项
sudo apt purge libreoffice-common
sudo apt purge thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot
sudo apt purge gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku
sudo apt purge onboard deja-dup
# Amazon
sudo apt purge unity-webapps-common
# firefox
sudo apt purge firefox firefox-locale-en unity-scope-firefoxbookmarks firefox-locale-zh-hans
```



### dns修改

```bash
# ubuntu 17.04有效

sudo vi /etc/resolvconf/resolv.conf.d/head
# 添加 nameserver 8.8.8.8 和 nameserver 114.114.114.114

sudo resolvconf -u
vi /etc/resolv.conf
```



### intel key

```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 611B903CAB97EA77
```



### ubuntu使用windows字体

```bash
# 先将windows字体放入该目录
cd /usr/share/fonts/winfonts
# 在该目录执行如下命令
sudo mkfontscale
sudo mkfontdir
sudo fc-cache -fv
```



### git

```bash
sudo apt install git
# 配置git
git config --global user.name "zzzzer"
git config --global user.email "zzzzer91@gmail.com"
# 生成ssh密钥
ssh-keygen -t rsa -C "zzzzer91@gmail.com"
```



### vim

```bash
# 先把已经配置好的.vimrc放入~/目录下
# 下载插件管理器
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
# airline必须字体, 下载后执行其中脚本
git clone https://github.com/powerline/fonts.git --depth=1
#进入vim后, 执行PluginInstall
```



### Python3必须依赖

```bash
sudo apt install zlib1g-dev
sudo apt install libffi-dev libssl-dev
sudo apt install libsqlite3-dev sqlite3
sudo apt install build-essential libbz2-dev libncurses-dev libreadline-dev libgdbm-dev liblzma-dev
sudo apt install tk-dev
sudo apt-get install exuberant-ctags
```



### pyenv

``` bash
# 安装配置
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc
# 下载python
pyenv install 3.6.4
# 启动环境
pyenv local 3.6.4
# 更新pyenv
cd $(pyenv root)
git pull
```



### pip必装

```bash
pip install ipython
pip install jupyter # jupyter notebook
pip install requests bs4 lxml # 爬虫
pip install numpy pandas matplotlib # 科学计算
pip install pillow # 图像处理
pip install pymongo # mongodb驱动
pip install pipenv # 包虚拟环境
```



### Shadowsocks-qt5

```bash
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt update
sudo apt install shadowsocks-qt5
```



### chrome

```bash
# 创建bash脚本ssc, 输入如下内容, 并放入/usr/local/bin, 方便使用ss
google-chrome --proxy-server="socks://127.0.0.1:1080"
```



### typora

```bash
# optional, but recommended
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
# add Typora's repository
sudo add-apt-repository 'deb http://typora.io linux/'
sudo apt-get update
# install typora
sudo apt-get install typora
```



### 美化工具

```bash
sudo apt install unity-tweak-tool
```



### mongodb

- 快捷命令配置

```bash
# 编辑mongodb.config文件, 放入~/Documents/data/mongodb/目录
dbpath=/home/zzzzer/Documents/data/mongodb/data
# 创建bash脚本mongod, 输入如下内容, 并放入/usr/local/bin
/opt/mongodb/bin/mongod --config=/home/zzzzer/Documents/data/mongodb/mongodb.conf
```

- 恢复数据

```bash
./mongorestore ~/all 
```



### shell

- 缩短shell中的目录名

```bash
vi .bashrc
# 把
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[0    1;34m\]\w\[\033[00m\]\$ '
# 这行中小写的w改成大写的
source .bash # 应用修改
```



### fish

- 安装

```bash
sudo apt-add-repository ppa:fish-shell/release-2
sudo apt-get update
sudo apt-get install fish
```

- 永久切换shell

```bash
chsh -s /usr/bin/fish
```



### jupyter notebook
- 生成配置文件
```bash
jupyter notebook --generate-config
```
- 配置修改(别忘去掉注释)
```python
# 修改初始目录
修改c.NotebookApp.notebook_dir = '你的目录'
# 不自动打开浏览器
c.NotebookApp.open_browser = False
```

