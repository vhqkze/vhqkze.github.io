# 树莓派

##常用配置
```bash
# 系统配置
sudo raspi-config
# 查看所有服务状态
sudo service --status-all
# 查看所有开机启动的服务
sudo systemctl list-unit-files --type=service | grep enabled
# 配置开机启动（无效）
sudo sysv-rc-conf
# 挂载ntfs磁盘
sudo mount -t ntfs-3g /dev/sda2 /home/pi/tmp
# 解锁root账户
sudo passwd root
sudo passwd --unlock root
```


```bash
# 查看8080端口
netstat -anp | grep 8080
```

```bash
# 开机自动挂载ntfs磁盘到指定位置
sudo vim /etc/fstab

# 在文件最后一行添加内容，然后保存
/dev/sda1  /home/pi/disk ntfs-3g defaults,noexec,umask=0000 0 0

/dev/sda1  /home/pi/disk1 ntfs-3g defaults,noexec,umask=000 0 0
/dev/sda2  /home/pi/disk2 ntfs-3g defaults,noexec,umask=000 0 0
/dev/sda3  /home/pi/disk3 ntfs-3g defaults,noexec,umask=000 0 0


# 保存后如果要立即生效，重启或卸载磁盘后使用以下命令
sudo mount -a
```

## 安装node
```bash
wget https://nodejs.org/dist/v8.9.3/node-v8.9.3-linux-armv7l.tar.xz
tar -Jxf node-v8.9.3-linux-armv7l.tar.xz
sudo mv node-v8.9.3-linux-armv7l /usr/local/
sudo rm /usr/bin/node
sudo ln -s /usr/local/node/bin/node /usr/bin/node
sudo ln -s /usr/local/node/bin/npm /usr/bin/npm
node -v
npm -v
```

