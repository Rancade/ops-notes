# ERROR: MySQL Start

If you get the following error when trying to start MySQL:

sudo systemctl stop mysql    # Ubuntu/Debian
sudo systemctl stop mysqld   # CentOS/RHEL

2. 卸载 MySQL 软件包
CentOS/RHEL：
sudo yum remove mysql-server mysql-client mysql-common
sudo yum autoremove

Ubuntu/Debian：
sudo apt purge mysql-server mysql-client mysql-common mysql-server-core-* mysql-client-core-*
sudo apt autoremove
sudo apt autoclean


首先备份文件：
sudo mv /usr/local/mysql/mysql-8.0.26 你的目录
# delete the old MySQL files
sudo rm -rf /etc/mysql /var/lib/mysql /var/log/mysql*
sudo rm -rf /etc/my.cnf /etc/my.cnf.d
sudo rm -rf /usr/local/mysql

#查看是否有残留的 MySQL 冲突包：
CentOS/RHEL：
rpm -qa | grep maridb-libs
rpm -e -nodeps mariadb-libs

Ubuntu/Debian：
dpkg -l | grep mysql-server
dpkg -P mysql-server

然后重新安装：
1. 下载 MySQL 8.0.26 二进制文件
如果你安装了 MySQL 8.0.26 之前的版本，请先卸载旧版本。
Ubuntu
sudo apt update -y 
CentOS
sudo yum update -y 

wget  https://downloads.mysql.com/archives/get/p/24/file/mysql-8.0.26-linux-glibc2.12-x86_64.tar

# 步骤4: 验证MD5值
使用 sha256sum 命令计算下载文件的 MD5 值。
sha256sum mysql-8.0.26-linux-glibc2.12-x86_64.tar

084510c174447ee9ec27451bf9945d91

# 步骤5: 解压文件 
注意：解压文件会生成多个文件
tar -xvf mysql-8.0.26-linux-glibc2.12-x86_64.tar

tar -xvf mysql-8.0.26-linux-glibc2.12-x86_64.tar.gz
sudo mkdir -p /usr/local/mysql/
sudo mv mysql-8.0.26-linux-glibc2.12-x86_64 /usr/local/mysql/mysql-8.0.26

# 安装依赖
Ubuntu
sudo apt-get install libaio1
CentOS
sudo yum install -y libaio

创建数据目录
mkdir /usr/local/mysql/mysql-8.0.26/data

groupadd mysql
useradd -r -g mysql:mysql
chown -R mysql:mysql /usr/local/mysql/mysql-8.0.26/
chmod -R 755 /usr/local/mysql/mysql-8.0.26/

# MySQL 8.0.26 安装准备完成!
1. 初始化数据库:
cd /usr/local/mysql/mysql-8.0.26/ 
得到临时密码:
./bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql/mysql-8.0.26 --datadir=/usr/local/mysql/msql-8.0.26/data

2022-01-16T07:32:18.729960Z 0 [Warning] [MY-011070] [Server] 'Disabling symbolic links using --skip-symbolic-links (or equivalent) is the default. Consider not using this option as it' is deprecated and will be removed in a future release.
2022-01-16T07:32:18.729960Z 0 [System] [MY-013169] [Server] /usr/local/mysql/bin/mysqld (mysqld 8.0.26) initializing of server in progress as process 7691
2022-01-16T07:32:18.740975Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2022-01-16T07:32:19.800287Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2022-01-16T07:32:21.721672Z 0 [Warning] [MY-013746] [Server] A deprecated TLS version TLSv1 is enabled for channel mysql_main
2022-01-16T07:32:21.722106Z 0 [Warning] [MY-013746] [Server] A deprecated TLS version TLSv1.1 is enabled for channel mysql_main
2022-01-16T07:32:21.787669Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: j-6lA2aXvQWPz

记录一下mysql数据库的临时密码 
echo "j-6lA2aXvQWPz" > ~/mysqloldpwd.txt

# 3. 配置 MySQL
vim /etc/my.cnf
# [这是我的配置](my.cnf)
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

9、创建mysql服务
1）将mysql.server启动文件复制到/etc/init.d目录，使用cp -a /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld命令。
2）赋予权限
chmod +x /etc/rc.d/init.d/mysqld
3）使用创建mysql服务
chkconfig --add mysqld

检查mysql服务是否生效
chkconfig  --list mysqld
  
Note: This output shows SysV services only and does not include native
      systemd services. SysV configuration data might be overridden by native
      systemd configuration.
  
      If you want to list systemd services use 'systemctl list-unit-files'.
      To see services enabled on particular target use
      'systemctl list-dependencies [target]'.
  
mysqld          0:off   1:off   2:on    3:on    4:on    5:on    6:off

10、配置全局环境变量
vim /etc/profile

export PATH=$PATH:/usr/local/mysql/bin:/usr/local/mysql/lib
export PATH

生效配置
source /etc/profile

11、启动mysql服务
service mysql start
Redirecting to /bin/systemctl start mysql.service

查看mysql服务状态
service mysql status

登录mysql，并输入临时密码
mysql -uroot -p

12、修改密码
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的密码';

13、设置mysql远程登录
1）切换数据库，使用use mysql;命令。

2）修改mysql库中host值，使用update user set host='%' where user='root' limit 1;命令。

3）刷新mysql权限，使用flush privileges;命令。

14、mysql客户端连接数据库
客户端连接mysql数据库，连接名（自定义名称）、主机（IP）、端口号及用户名和密码，点击测试连接按钮，显示连接成功即可。