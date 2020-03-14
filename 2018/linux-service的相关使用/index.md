# Linux service的相关使用


## service的相关操作
```bash
service nginx start
service nginx stop
service nginx reload
service nginx restart
service nginx status

# 但实际上推荐改用：
systemctl start nginx
systemctl start nginx.service
systemctl stop nginx
systemctl reload nginx
systemctl restart nginx
systemctl status nginx
```

## 配置service开机启动
```bash
# 将一个 服务设置为开机启动
systemctl enable nginx.service (.service可省略如下)
systemctl enable nginx
#要禁用自启动，只要disable即可。
systemctl disable nginx.service
systemctl disable nginx
```

## 其他
```bash
# 查看所有服务状态
service --status-all
# 列出所有运行中单元
systemctl list-units
# 查看所有开机启动的服务
systemctl list-unit-files --type=service | grep enabled
systemctl is-enabled nginx #检查开机自动启动状态
systemctl is-active nginx  #检查服务是否已经启动
systemctl is-failed application.service #检查服务是否失败
```
