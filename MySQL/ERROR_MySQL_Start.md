# 🐾 MySQL急救指南：安装失败の萌え解決方案

> ✨ 当你的MySQL无法正常启动时，试试这些魔法命令吧~ 

---

## 1. 💔 常见错误症状
```bash
sudo systemctl status mysql
# 输出可能包含：
# [ERROR] Can't start server :'( 
# [Warning] 各种傲娇的警告消息 >_<
```

---

## 2. 🧹 彻底清理旧版本
```bash
🐱 Ubuntu/Debian
sudo apt purge mysql-* \
&& sudo apt autoremove \
&& sudo apt autoclean

🐯 CentOS/RHEL
```bash
sudo yum remove mariadb-libs \
&& rpm -e --nodeps $(rpm -qa | grep mysql) 
```

## 3. 🧹 查看是否有残留的 MySQL 冲突包：
CentOS/RHEL：
```bash
rpm -qa | grep maridb-libs
rpm -e -nodeps mariadb-libs
```
Ubuntu/Debian：
```bash
dpkg -l | grep mysql-server
dpkg -P mysql-server
```

---

## 4. 🎀 重新安装MySQL 8.0
1. 下载并验证
```bash
wget  https://downloads.mysql.com/archives/get/p/24/file/mysql-8.0.26-linux-glibc2.12-x86_64.tar

# 步骤2: 验证MD5值
sha256sum mysql-8.0.26-linux-glibc2.12-x86_64.tar
084510c174447ee9ec27451bf9945d91
```

2. 解压准备
```bash
# 注意：解压文件会生成多个文件
tar -xvf mysql-8.0.26-linux-glibc2.12-x86_64.tar

tar -xvf mysql-8.0.26-linux-glibc2.12-x86_64.tar.gz
sudo mkdir -p /usr/local/mysql/
sudo mv mysql-8.0.26-linux-glibc2.12-x86_64 /usr/local/mysql/mysql-8.0.26
```
3. 创建专属用户
```bash
# 数据库安装目录
mkdir /usr/local/mysql/mysql-8.0.26/data

sudo groupadd mysql && sudo useradd -r -g mysql mysql
sudo chown -R mysql:mysql /usr/local/mysql/mysql-8.0.26/
sudo chmod -R 755 /usr/local/mysql/mysql-8.0.26/
```

---

## 5. 🛠️ 初始化数据库
1. 获取临时密码
```bash
cd /usr/local/mysql/mysql-8.0.26/ 
# 获取临时密码:
./bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql/mysql-8.0.26 --datadir=/usr/local/mysql/msql-8.0.26/data
```
> ✨ 临时密码 echo "临时密码" > ~/mysql-password.txt (ﾉ◕ヮ◕)ﾉ*:･ﾟ✧
# MySQL 8.0.26 安装准备完成!
2. mysql配置文件
[这是我的配置](my.cnf)
```bash
vim /etc/my.cnf

[mysqld]
    basedir = /usr/local/mysql/mysql-8.0.26
    datadir = /usr/local/mysql/mysql-8.0.26/data
    socket = /usr/local/mysql/mysql-8.0.26/mysql.sock
    character-set-server=utf8
    port = 3306
   sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
 [client]
   socket = /usr/local/mysql/mysql-8.0.26/mysql.sock
   default-character-set=utf8
```

---

## 6. 🎀 服务设置与启动
1. 创建系统服务
```bash
sudo cp support-files/mysql.server /etc/init.d/mysqld
sudo chmod +x /etc/init.d/mysqld
sudo chkconfig --add mysqld

#检查mysql服务是否生效
chkconfig  --list mysqld

Note: This output shows SysV services only and does not include native
      systemd services. SysV configuration data might be overridden by native
      systemd configuration.
  
      If you want to list systemd services use 'systemctl list-unit-files'.
      To see services enabled on particular target use
      'systemctl list-dependencies [target]'.
  
mysqld          0:off   1:off   2:on    3:on    4:on    5:on    6:off
```
2. 设置环境变量
```bash
vim /etc/profile

export PATH=$PATH:/usr/local/mysql/bin:/usr/local/mysql/lib
export PATH

# 生效配置
source /etc/profile
```

3、启动mysql
```bash
service mysql start

查看mysql服务状态
service mysql status
```

---

## 7. 🔑 安全设置
1、修改root密码
```bash
登录mysql，并输入临时密码
mysql -uroot -p

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的密码';
```
2. 允许远程访问
```bash
UPDATE mysql.user SET host='%' WHERE user='root';
FLUSH PRIVILEGES;
```

---

## 8. 💖 常用维护命令
| 操作	| 命令 | 颜文字 |
| ------|---------|------- |
| 启动服务	| sudo service mysqld start	| (◕‿◕)✧ |
| 停止服务	| sudo service mysqld stop	| (´-ω-｀) |
| 查看状态	| sudo service mysqld status	| (｀・ω・´) | 
| 查看日志 | sudo tail -f /var/log/mysqld.log	| (ﾉ>ω<)ﾉ |

---

## 🎯 疑难解答
Q: 端口被占用怎么办？
```bash
sudo netstat -tulnp | grep 3306 \
| awk '{print $7}' | cut -d'/' -f1 | xargs kill -9
```
Q: 忘记密码了？
```bash
sudo mysqld_safe --skip-grant-tables &
mysql -u root
# 然后在MySQL中执行：
UPDATE mysql.user SET authentication_string=null WHERE User='root';
FLUSH PRIVILEGES;
exit;
```
> ✨ 更多问题可以查看[官方文档](https://dev.mysql.com/doc/)哦~