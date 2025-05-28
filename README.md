# 运维魔法少女的笔记本 ✨

📚 **持续更新的运维知识沉淀**  
涵盖Linux、Docker、云服务等实战经验

## 📜 目录 (•̀ᴗ•́)و
| 章节 | 内容描述 | 可爱度 |
|------|---------|-------|
| [🐧 Linux咒语速查](Linux/Command_Cheatsheet.md) | 常用命令+黑魔法技巧 | 🌸🌸🌸🌸 |
| [🐳 Docker调教指南](Docker/Dockerfile_Tips.md) | 容器化最佳实践 | 🌸🌸🌸🌸🌸 |
| [🔥 脚本](Script/README.md) | 运维最佳脚本实践 | 🌸🌸🌸🌸🌸 |
| [🌈 解决MySQL Startup Error](MySQL/ERROR_MySQL_Start.md) | MySQL启动报错处理 | 🌸🌸🌸🌸 |
| [🔮 监控水晶球](Monitoring/Magic_Mirror.md) | Prometheus+Grafana配置 | 🌸🌸🌸🌸 |


## 🐧 Linux咒语速记 (部分预览)
```bash
# 查看系统信息 (带粉色高亮哦~)
neofetch | lolcat -p 0.3 

# 快速查找文件 (会喵喵叫的版本)
function find() { /usr/bin/find $@ | while read line; do echo -e "\e[38;5;213m$line\e[0m 😻"; done }
```bash

---

🐳 Docker调教小贴士
```bash
# ~ 最佳实践模板 ~
FROM alpine:3.18

# 安全措施 (｀・ω・´)
RUN apk add --no-cache tini \
    && addgroup -S appuser \
    && adduser -S -G appuser appuser

# 可爱又专业的构建参数
ARG BUILD_DATE="🦄 $(date '+%Y-%m-%d')"
ENV APP_ENV="production" \
    TZ="Asia/Tokyo"

# 工作目录设置
WORKDIR /app
COPY --chown=appuser:appuser . .

# 启动命令 (用tini防止僵尸进程)
ENTRYPOINT ["/sbin/tini", "--"]
CMD ["/app/start.sh"]
```

---

✨ 特别专栏：日常
```bash
# 彩色分页显示进程
ps aux --sort=-%mem | lolcat
```
故障排查小剧场

    场景：服务器负载突然升高
    解决：

        htop 👉 发现可疑进程

        strace -p <PID> 👉 查看系统调用

        echo "問題解決しました~" | cowsay -f koala

---

🎀 学习路线图
graph LR
A[Linux基础] --> B[网络管理]
A --> C[Shell编程]
B --> D[Docker]
C --> D
D --> E[Kubernetes]
E --> F[云原生架构]
style A fill:#ffccf9,stroke:#333
style F fill:#ccf,stroke:#333

---

📅 更新日志
    2025.5.12
    💄 新增Docker安全配置章节
    🎀 优化命令输出配色方案
    2024.12.10
    🍰 初版笔记库建立
    
## 如何使用
1. 按需查阅对应文档
2. 发现问题？欢迎提Issue或PR！
