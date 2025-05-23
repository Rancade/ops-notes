# 用户管理命令
useradd 你的用户名
useradd -m 你的用户名  #创建用户,并创建HOME
usermod -aG wheel username  #用户添加到wheel组
passwd 你的用户名  #修改密码
userdel 你的用户名  #删除用户

# Linux ndf包管理命令
yum install ndf  # CentOS 7
apt-get install ndf # Ubuntu
dnf install ndf  # CentOS 7 /Ubtuntu

1. 启用 COPR 仓库
以安装sl为例，首先需要启用 COPR 仓库：
dnf install -y dnf-plugins-core
2. 安装 sl
dnf install sl  # CentOS 7 /Ubtuntu


# 查系统日志
-x：这个选项告诉 journalctl 显示日志的详细上下文
-e: 这个选项让 journalctl 直接跳转到日志的末尾，也就是最新的日志条目
journalctl -xe | grep ERROR

# 查端口占用
-t: 这个选项告诉 ss 显示 TCP 套接字。
-u: 这个选项告诉 ss 显示 UDP 套接字。
-l: 这个选项告诉 ss 显示监听正在等待连接的套接字。
-n: 这个选项告诉 ss 不要将地址和端口号解析为域名和服务名，而是以数字形式显示。
-p: 这个选项告诉 ss 显示与套接字相关的进程信息（即哪个进程正在使用该套接字）。
ss -tulnp | grep 80    

# 查磁盘和内存            
df -h && free -m              

# 用源文件编译安装 以python3.10为例
1. 依赖安装
sudo dnf install -y gcc openssl-devel bzip2-devel libffi-devel zlib-devel
wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tar.xz
tar -xf Python-3.10.0.tar.xz

cd Python-3.10.0
./configure --enable-optimizations
make -j $(nproc)
sudo make altinstall
python3.10 --version
python3.10 -m venv myenv