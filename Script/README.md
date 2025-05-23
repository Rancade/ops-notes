# 🐧✨ 运维魔法少女的工具箱

> 一键部署 & 实时监控的可爱脚本集合  
> 脚本使用方法请参考各个脚本的 README 文件

---

## 📜 脚本目录

### 1. ⏱️ [Auto_time_CPU 实时监控CPU使用状态](https://github.com/Rancade/Auto_timed_CPU_Monitor/tree/4d8bbe92a37d84913ebf65e4b25908ae1f1108ef)
```bash
crontab -e 

5 * * * * /path/to/Auto_timed_CPU_Monitor/Auto_time_CPU.sh
```
### 🛠️ 功能特点
- **多维度监控**：CPU 💻 | 内存 🧠 | 磁盘 💾 | 进程 📊 | 负载 ⚖️
- **智能告警**：超过阈值自动发送邮件 📧
- **历史记录**：自动保存监控日志 📝
- **可爱预警**：使用颜文字区分告警级别 (◕‿◕✿)

### 2. 🐬 [install_MySQL 自动安装 MySQL](https://github.com/Rancade/install_MySQL/tree/4fc0f2e97969c0717d4871601738e849d99bb73c)
```bash
# 一键部署MySQL
./install_mysql.sh
```
### 🛠️ 功能特点
- **自动安装**：一键部署 MySQL 环境

### 3. 🛡️ [Nginx_logs 自动封禁 Nginx ip 访问日志](https://github.com/Rancade/Nginx_logs/tree/d80c4301e2a64254a10defdda579dcc136cff41e)
```bash
crontab -e 

0 0 * * * /path/to/Nginx_logs/awk_nginxlogs.sh
```
### 🛠️ 功能特点
- **自动封禁**：自动封禁 Nginx 日志中 IP 地址访问的请求

---

## 🌈 其他脚本
watch -n 1 -c 'echo "系統ちゃんは見ているよ~" | ponysay'