# Manjaro KDE版安装后配置

### pacman换源

- 生成可用中国镜像站列表:

```bash
sudo pacman-mirrors -i -c China -m rank
# 会弹出选项框, 选清华大学源
```

- 刷新缓存:

```bash
sudo pacman -Syy # 两个yy代表强制刷新，即使已经是最新的
```

### 卸载不需要软件

```bash
# 仅供参考
# 删除指定软件包，及所有没有被其他已安装软件包使用的依赖关系(s)，及配置文件(n)
sudo pacman -Rsn steam-manjaro libreoffice-fresh ms-office-online hplip firefox manjaro-settings-manager-knotifier octopi-notifier-frameworks manjaro-hello manjaro-documentation-en konversation thunderbird kget cantata vlc bluedevil pulseaudio-bluetooth kwalletmanager kwallet-pam user-manager subversion ruby
# vlc用mpv代替
```

### 更新系统

```bash
sudo pacman -Syu # 同步源(y)，并更新系统(u)
```

### git

```bash
# 1.安装
sudo pacman -S git
# 2.配置
git config --global user.name "zzzzer"
git config --global user.email "zzzzer91@gmail.com"
# 3.生成ssh密钥
ssh-keygen -t rsa -C "zzzzer91@gmail.com"
# 4.在github上更新密钥
```

### vim

```bash
# 1.安装
sudo pacman -S vim 
# 2.先把已经配置好的.vimrc放到家目录下
# 3.下载vim插件管理器
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
# 4.进入vim后, 执行PluginInstall安装相关插件
# 5.起别名, `vim ~/.bashrc`, 添加:
alias vi='vim' 
```

### pyenv

``` bash
# 1.安装
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
# 2.配置(自动补全功能等)
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc
# 3.下载指定版本python
pyenv install 3.6.5
# 4.切换环境
pyenv local 3.6.5
# 5.更新
cd $(pyenv root)
git pull
```

### shadowsocks

```bash
# 1.安装
pip install git+https://github.com/shadowsocks/shadowsocks.git@master
# 2.在 .bashrc 中设置命令, 方便使用
alias sss="sslocal -c ~/Documents/data/shadowsocks/config.json"
alias ssc='google-chrome-stable --proxy-server="socks://127.0.0.1:1080"'
alias setproxy="export ALL_PROXY=socks5://127.0.0.1:1080"
alias unsetproxy="unset ALL_PROXY"
```

### 中文输入法

```bash
# 1.安装fcitx
sudo pacman -S fcitx-im # 选择全部安装
sudo pacman -S fcitx-configtool # 图形化配置工具
# 2.修改~/.xprofile, 添加:
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
# 3.在fcitx输入法选项中添加pinyin
```

### 开启AUR

- 安装

```bash
sudo pacman -S yaourt
```

- 换源

```bash
# 修改 /etc/yaourtrc，去掉 # AURURL 的注释，修改为
AURURL="https://aur.tuna.tsinghua.edu.cn"
```

- 必装

```bash
yaourt -S google-chrome
yaourt -S typora # markdown编辑器
yaourt -S visual-studio-code-bin # vscode
yaourt -S wps-office ttf-wps-fonts # wps, 奇怪问题, 会导致chrome字体出问题
```

- **注意**: 命令台用yaourt安装时不需加sudo, 否则报错

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

### jupyter的配置

- 先生成配置文件
```bash
jupyter notebook --generate-config
```
- 配置修改(别忘去掉注释)
```python
# 修改初始目录
修改c.NotebookApp.notebook_dir = '/home/zzzzer/Documents/code/jupyter'
# 不自动打开浏览器
c.NotebookApp.open_browser = False
```

### 其他

- 设置其他命令

```bash
# mongod
alias mongod="/opt/mongodb/bin/mongod --config=~/Documents/data/mongodb/mongodb.conf"
```

- 选择bash下默认编辑器

```bash
# 在.bashrc下添加如下一行
EDITOR="/usr/bin/vim"
```

- 安装Windows字体

```bash
# 先将windows字体放入该目录
cd /usr/share/fonts/winfonts
# 在该目录执行如下命令
sudo mkfontscale
sudo mkfontdir
sudo fc-cache -fv
```

