# 1. 🐘PostgreSQL
## 1.1 安装

> [官方安装文档](https://www.postgresql.org/download/linux/debian/)

```shell
sudo apt install postgresql

sudo systemctl enable postgresql

# 查看日志
sudo tail -f /var/log/postgresql/postgresql-<???>-main.log

# 使用 postgres 用户去登录
sudo -i -u postgres
# 使用默认命令行工具
psql
# 修改密码
/password postgres
# 退出
/q
```

## 1.2 **问题解决**
### 1.2.1 监听IP

> 报错提示：could not connect to server: Connection refused (0x0000274D/10061) Is the server running on host "192.168.137.2" and accepting TCP/IP connections on port 5432?

可能是没有监听所有IP，需要修改 `/etc/postgresql/<???>/main/postgresql.conf` 配置文件
```shell
sudo -i -u postgres
vim /etc/postgresql/<???>/main/postgresql.conf

# 修改以下这一行内容
listen_addresses = '*'

# 然后退出来重启服务
sudo systemctl restart postgresql
```

### 1.2.2 pg_hba 配置错误

> 致命错误:  没有用于主机 "192.168.137.1", 用户 "postgres", 数据库 "postgres", SSL encryption 的 pg_hba.conf 记录

这是 `pg_hba.conf` 文件没有配置正确
```shell
sudo -i -u postgres
vim /etc/postgresql/<???>/main/pg_hba.conf

# 在文件末尾添加以下内容
host all all 0.0.0.0/0 md5

# 然后退出来重启服务
sudo systemctl restart postgresql
```



