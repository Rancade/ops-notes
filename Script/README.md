# 🐧✨ 运维魔法少女的工具箱

> 一键部署 & 实时监控的可爱脚本集合  
> 脚本使用方法请参考各个脚本的 README 文件

---

## 📜 脚本目录

### 1. ⏱️ [Auto_time_CPU 实时监控CPU使用状态](Auto_timed_CPU_Monitor/README.md)
```bash
crontab -e 

5 * * * * /path/to/Auto_timed_CPU_Monitor/Auto_time_CPU.sh
```
### 🛠️ 功能特点
- **多维度监控**：CPU 💻 | 内存 🧠 | 磁盘 💾 | 进程 📊 | 负载 ⚖️
- **智能告警**：超过阈值自动发送邮件 📧
- **历史记录**：自动保存监控日志 📝
- **可爱预警**：使用颜文字区分告警级别 (◕‿◕✿)

### 2. 🐬 [install_MySQL 自动安装 MySQL](install_MySQL @ 4fc0f2e/README.md)
```bash
# 一键部署MySQL
./install_mysql.sh
```
### 🛠️ 功能特点
- **自动安装**：一键部署 MySQL 环境

### 3. 🛡️ [Nginx_logs 自动封禁 Nginx ip 访问日志](Nginx_logs/README.md)
```bash
crontab -e 

0 0 * * * /path/to/Nginx_logs/awk_nginxlogs.sh
```
### 🛠️ 功能特点
- **自动封禁**：自动封禁 Nginx 日志中 IP 地址访问的请求

---

## 🌈 其他脚本
watch -n 1 -c 'echo "系統ちゃんは見ているよ~" | ponysay'