### python版本管理: pyenv

- 安装

```bash
# 安装
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
# 配置(自动补全功能等)
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc
```

- 命令

```shell
# 显示可下载内容
pyenv install --list
# 下载指定版本python
pyenv install <版本> 
# 显示所有已安装环境
pyenv versions 
# 切换版本
pyenv local <版本> 
# 刷新数据, 在pip安装新包后使用
pyenv rehash 
# 卸载环境
pyenv uninstall <版本>
```

- 更新

```shell
cd $(pyenv root)
git pull
```

### python包管理: pip

- 命令

```shell
# 列出所有库
pip list 
# 列出所有过期库
pip list --outdate 
# 只下载该库
pip download <PACKAGE_NAME> 
# 下载安装该库
pip install <PACKAGE_NAME> 
# 升级该库
pip install --upgrade <PACKAGE_NAME> 
# 安装指定版本包
pip install "requests==2.8.7" 
# 安装小于版本2的最新包
pip install "django<2" 
# 安装requirements.txt文件中指定的拓展库
pip install -r requirements.txt
# 用指定源安装包, 这里是清华大学源
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple <PACKAGE_NAME> 
# 卸载该库
pip uninstall <PACKAGE_NAME> 
```

### 虚拟环境管理: pipenv

- 安装

```shell
pip install pipenv
```

- 命令

```shell
# pipenv有两个条目: 普通和dev, 根据开发需求选择包安装条目
pipenv install # 仅初始化环境, 如果目录下有Pipfile文件, 则根据其内容生成环境
pipenv install flask # 初始化环境并在普通条目安装flask
pipenv install flask --dev # 初始化环境并在dev条目安装flask

# 列出pip包之间依赖关系
pipenv graph
# 更新生成Pipfile.lock文件
pipenv lock

# 显示虚拟环境所在目录
pipenv --venv
# 启用虚拟环境
pipenv shell

# 删除环境
pipenv uninstall --all-dev # 卸载dev条目所有包, 注意虚拟环境还在
pipenv uninstall --all # 卸载所有包, 注意虚拟环境还在
pipenv --rm # 完全删除虚拟环境, 只需这一条指令, 并删除工作目录
```

