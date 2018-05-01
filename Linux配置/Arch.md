# Arch安装配置(BIOS版, 未完工, 勿参考)

- 验证是BIOS还是UEFI

```bash
ls /sys/firmware/efi/efivars # 找不到则为BIOS
```

- 联网

```bash
# wifi
wifi-menu

# 或拨号
pppoe-setup # 设置帐号密码
systemctl start adsl # 连接
```
- 同步时间

```bash
timedatectl set-ntp true
```

- 建立磁盘分区

```bash
# 列出磁盘
fdisk -l

# 创建新分区
cfdisk /dev/sdX # sdX 为目标磁盘, 一般为sda

# 列出刚创建的磁盘分区
lsblk

# 格式化分区
mkfs.ext4 /dev/sdXY # Y指分区号

# 挂载分区
mount /dev/sdXY /mnt
```

- 安装

```bash
# 换源
vim /etc/pacman.d/mirrorlist

# 安装基本系统
pacstrap /mnt base
```

- 配置系统

```bash
# 生成分区表
genfstab -U /mnt >> /mnt/etc/fstab

# 进入新系统
arch-chroot /mnt

# 设置时区上海
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# 设置区域
vim /etc/locale.gen # 选择en_US
locale-gen # 生成
echo LANG=en_US.UTF-8 > /etc/locale.conf

# 修改主机名
echo <主机名> > /etc/hostname

# 向/etc/hosts文件添加hosts条目
127.0.0.1	localhost.localdomain	localhost
::1		localhost.localdomain	localhost
127.0.1.1	<主机名>.localdomain	<主机名>

# 为root用户创建密码
passwd

# 安装Intel-ucode
pacman -S intel-ucode

# 安装Bootloader
pacman -S grub os-prober
grub-install --target=i386-pc --recheck /dev/sdX # 注意这里是整块磁盘, 而不是某个分区
grub-mkconfig -o /boot/grub/grub.cfg
vim /boot/grub/grub.cfg # 检查入口

# 重启
 exit # 退出chroot环境
 umount -R /mnt # 手动卸载被挂载的分区, 这有助于发现任何繁忙的分区
 root # 重启
 
 # 添加普通用户
useradd -m zzzzer
passwd zzzzer

# 驱动
pacman -S xf86-video-vesa
pacman -S xf86-video-intel
```

