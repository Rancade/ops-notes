# 🐧✨ Linux魔法少女的命令手册

> 常用命令+小技巧合集  
> 记得用`sudo`施展魔法~ (◕‿◕✿)

## 🌸 目录导航
1. [👩‍💻 用户管理](#-用户管理魔法)  
2. [📦 软件包管理](#-软件包管理)  
3. [📜 系统日志查看](#-系统日志查看)  
4. [🔌 端口管理](#-端口管理)  
5. [💾 磁盘内存检查](#-磁盘内存检查)  
6. [🐍 源码编译安装](#-源码编译安装)  

---

## 🧑‍💻 用户管理魔法

```bash
# 创建新用户（会自动生成HOME目录）
useradd -m 樱花酱  # 创建/home/樱花酱

# 赋予管理员权限
usermod -aG wheel 樱花酱 && echo "变身成功！(๑•̀ㅂ•́)و✧"

# 修改密码（会提示输入两次）
passwd 樱花酱  # 密码太简单会被吐槽哦~

# 删除用户（无情！）
userdel -r 坏孩子  # 连HOME目录一起消失！ 
```

## 📦 软件包管理
Linux ndf包管理命令
# 不同发行版的安装方式
```bash
🐱 CentOS: sudo yum install ndf
🐶 Ubuntu: sudo apt-get install ndf
🦊 Fedora: sudo dnf install ndf

# dnf 安装小火车~
sudo dnf install -y dnf-plugins-core
sudo dnf copr enable someuser/sl
sudo dnf install sl  # choo-choo! 🚂
```

---

## 📜 系统日志查看
```bash
# 查看错误日志 (-x显示详情 -e跳到最后)
journalctl -xe | grep ERROR
# 常用过滤选项
🐾 -u 服务名: 只看特定服务
🎀 --since "1 hour ago": 最近1小时
✨ -f: 实时跟踪日志
```

---
## 🔌 端口管理
```bash
# 查看端口占用情况 (带进程信息)
ss -tulnp | grep 80    
# 选项说明：
-t: TCP连接
-u: UDP连接
-l: 正在监听的
-n: 不解析域名
-p: 显示进程信息
```

---

## 💾 磁盘内存检查          
```bash
# 一键查看磁盘和内存
df -h && free -m  

# 更可爱的显示方式
df -h | grep -v tmpfs | awk '{printf "%-20s %-10s %s\n", $1,$3,$4}' | column -t            
```

---

## 🐍 源码编译安装
```bash
# 以Python 3.10为例
# 1. 先安装依赖
1. 依赖安装
sudo dnf install -y gcc openssl-devel bzip2-devel libffi-devel zlib-devel

# 2. 下载源码
wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tar.xz
tar -xf Python-3.10.0.tar.xz

# 3. 编译安装 (--enable-optimizations会优化性能)
cd Python-3.10.0
./configure --enable-optimizations
make -j $(nproc)  # 使用所有CPU核心
sudo make altinstall

# 4. 验证安装
python3.10 --version  # 应该显示3.10.x
python3.10 -m venv myenv  # 创建虚拟环境
```

---

> ✨ 提示：所有命令都支持 --help 查看帮助~
> 遇到问题可以 man 命令名 查看手册
