# TigerVNC使用

## 开启vnc服务
1. 安装

    ```bash
    sudo dnf install tigervnc-server
    ```
2. 开启防火墙

    ```bash
    sudo firewall-cmd --add-service=vnc-server --permanent
    sudo firewall-cmd --reload
    ```
3. 设置连接密码，可以设为只读密码

    ```bash
    [fedora@parallels ~]$ vncpasswd
    Password:
    Verify:
    Would you like to enter a view-only password (y/n)? n
    ```
4. 开启服务
    
    ```bash
    # 直接开启
    vncserver
    # 指定服务名称为 :1
    vncserver :1
    # 指定分辨率，注意不要设置太大，否则可能会出现连接后无法完全显示导致无法操作
    vncserver -geometry 1920x1080
    ```
5. 关闭服务

    ```bash
    # 如果多次建立远程桌面，可以先列出所有远程桌面再关闭
    vncserver -list
    vncserver kill :1
    ```
    
## 连接vnc服务器
```bash
# 注意默认开启的端口是5901
vnc://192.168.123.124:5901
```


    
## 参考
- [Fedora 30 : Configure VNC Server : Server World](https://www.server-world.info/en/note?os=Fedora_30&p=desktop&f=6)
- [在运行 Amazon Linux 2 的 Amazon EC2 实例中安装图形用户界面 (GUI)](https://aws.amazon.com/cn/premiumsupport/knowledge-center/ec2-linux-2-install-gui/)
- [vncserver](https://tigervnc.org/doc/vncserver.html)
