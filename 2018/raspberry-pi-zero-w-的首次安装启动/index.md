# Raspberry Pi Zero W 的首次安装启动

## 刻录系统
到[树莓派官网](https://www.raspberrypi.org/downloads/raspbian/)下载系统，使用 [etcher](https://etcher.io/) 工具刻录系统。我下载的是`2018-04-18 raspbian stretch lite`版本。
## 设置开机自动连接WIFI
因为zero W 没有网线端口，只能使用WIFI连接网络，所以需要让系统开机自动连接WIFI。
### 修改/etc/network/interfaces
运行 `sudo vim /etc/network/interfaces` 命令，打开后作如下修改：
```
auto lo eth0 wlan0 wlan1

iface lo inet loopback
iface eth0 inet dhcp

allow-hotplug wlan0 wlan1

iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

iface wlan1 inet manual
pre-up which owifi
up owifi start
down owifi stop
```
建议，若你不使用树莓派的有线网口连接网络的话，最好把 `/etc/network/interfaces` 文件第一行（也可能不在第一行）中 `auto lo eth0 wlan0` 的 `eth0` 删掉。因为它会导致树莓派开机时等待有线网卡动态分配IP，但实际上你的有线网口并没有连接到路由器，这里会让内核等待更长的时间，从而拖慢开机速度。

但是要注意，删掉 `eth0` 的话就意味着无法通过网线连接路由器了，要想连接就必须重新加上 `eth0` 。第一次设置WiFi连接后接网线一直连不上，因为你可能不在家或者换到其他网络环境，所以想要连接树莓派还需要 通过网线连接才能添加新的WiFi，所以这里要注意下。

### 修改`/etc/wpa_supplicant/wpa_supplicant.conf`
除 `/etc/network/interfaces` 之外，你还需要修改 `/etc/wpa_supplicant/wpa_supplicant.conf`。

运行 `sudo vim /etc/wpa_supplicant/wpa_supplicant.conf` 命令， 打开 `/etc/wpa_supplicant/wpa_supplicant.conf` 照着下面的样子添加（请不要删除原先就已经存在的任何行）(注意！注释部分不要写进去)：
```conf
# 最常用的配置。WPA-PSK 加密方式。
network={
    ssid="WiFi-Name" //wifi名称
    psk="WiFi-password" //wifi密码
    key_mgmt=WPA-PSK //加密方式
    priority=1 //连接优先级，数字越大优先级越高（不可以是负数）
    scan_ssid=1 //连接隐藏WiFi时需要指定该值为1
}

network={
    ssid="WiFi-name2"
    psk="WiFi-password2"
    priority=4
}
```

`priority` 是指连接优先级，数字越大优先级越高（不可以是负数）。

按照自己的实际情况，修改这个文件。

例如，你家中有3个WiFi，分别为WiFi-A、WiFi-B和WiFi-C。你希望树莓派的连接优先级为 WiFi-A>WiFi-B>WiFi-C，则整个配置文件看起来像这样：

```conf
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="WiFi-A"
    psk="12345678"
    priority=5
}

network={
    ssid="WiFi-B"
    psk="12345678"
    priority=4
}

network={
    ssid="WiFi-C"
    psk="12345678"
    priority=3
}
```
到这里WiFi配置就结束了，重启树莓派后，树莓派将自动连接到WiFi，你可以不用网线直接从路由器管理页面找到对应的IP，然后通过SHH访问树莓派。当然通过这种方式树莓派的IP是动态分配的，我们需要设置静态IP，如果想要更方便还可以给树莓派设置域名，通过域名访问树莓派。

## ssh 连接
开机后使用ssh连接会提示`ssh: connect to host 192.168.123.93 port 22: Connection refused`，所以需要在刻录后的`boot`分区中新建一个名为`ssh`的空文件，之后启动系统就可以使用ssh连接了。

## 启动后配置
1. 配置时区位置等
使用`sudo raspi-config`命令可配置密码、时区等。
2. 更换软件源
  - [清华源](https://mirrors.tuna.tsinghua.edu.cn/help/raspbian/)
  - [中科大源](http://mirrors.ustc.edu.cn/help/raspbian.html)
    ```bash
    cd /etc/apt/
    cat sources.list
    sudo cp sources.list sources.list.bak
    sudo sed -i 's|raspbian.raspberrypi.org|mirrors.ustc.edu.cn/raspbian|g' /etc/apt/sources.list
    cat sources.list
    cd sources.list.d/
    cat raspi.list 
    sudo cp raspi.list raspi.list.bak
    sudo sed -i 's|//archive.raspberrypi.org|//mirrors.ustc.edu.cn/archive.raspberrypi.org|g' /etc/apt/sources.list.d/raspi.list
    cat raspi.list 
    sudo apt update
    sudo apt upgrade
    ```
3. 更新系统
    ```bash
    sudo apt update
    sudo apt upgrade
    ```
4. 更改主机名
    ```bash
    sudo vim /etc/hostname
    sudo reboot
    ```
5. 解锁`root`用户
   1. 先使用`sudo passwd root`命令为root用户创建密码
   2. 使用`sudo passwd --unlock root`命令解锁root账户
6. 安装`zsh`
参考 [oh my zsh](https://github.com/robbyrussell/oh-my-zsh)


## 参考资料
- [树莓派3代从0到连接WIFI访问（无显示器）](http://movesan.me/2017/02/07/raspberry/)



