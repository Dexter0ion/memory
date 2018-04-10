 ### pacman

- 生成可用中国镜像站列表:

```bash
sudo pacman-mirrors -i -c China -m rank
# 会弹出选项框, 选清华大学源
```

- 刷新缓存:

```bash
sudo pacman -Syy # 两个yy代表强制刷新
```

- 相关命令

```bash
# 同步源, 并检查更新
sudo pacman -Syu
# 安装指定包
sudo pacman -S <package_name>
# 显示信息后, 安装指定包
sudo pacman -Sv <package_name>
# 只下载包, 不安装
pacman -Sw <package_name>
# 删除指定软件包, 及所有没有被其他已安装软件包使用的依赖关系(s), 及不备份配置文件(n)
sudo pacman -Rsn <package_name> 
# 在仓库中搜索含关键字的包
pacman -Ss <关键字>
# 搜索含关键字的已安装的包
pacman -Qs <关键字>
# 列出包信息
pacman -Qi <package_name> 
# 列出该包的文件
pacman -Ql <package_name> 
# 列出所有不再作为依赖的软件包(孤立orphans)
pacman -Qdt
# 清理未安装的包文件, 包文件位于 /var/cache/pacman/pkg/ 目录
pacman -Sc
# 清理所有的缓存文件
pacman -Scc
```

### yaourt

- 命令与pacman基本一致