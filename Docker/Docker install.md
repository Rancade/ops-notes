# 🎀 博客容器化指南 (✿◕‿◕✿)

---

> [这是我的Dockerfile配置](Dockerfile)

---

## 1. 安装 Docker
### Docker Install on CentOS 7
```bash
sudo yum install docker
sudo yum start docker
```
### Docker Iinstall on Ubuntu 20.04
```bash
sudo apt-get docker
sudo systemctl start docker
```
## 2. 将当前用户加入 docker 组
```bash
sudo usermod -aG docker $USER
```
## 3. 创建 Dockerfile
/www/dist/是我的网站目录
在 /www/dist/ 同级目录下创建 Dockerfile，内容如下：

## 🌸 博客容器化指南

---
#### **1. 准备容器**
```bash
cd /www  
touch Dockerfile  # 新建一个萌萌的Dockerfile~
```
#### **2. 编写Dockerfile** (⁄ ⁄•⁄ω⁄•⁄ ⁄)
```bash
# 基础镜像：轻量级Nginx（Alpine版）
FROM nginx:alpine  #最small >_<

# 清理默认页面
RUN rm -rf /usr/share/nginx/html/*  

# 把博客文件塞进容器里~
COPY dist/ /usr/share/nginx/html/  

# 露出80端口（可以偷偷访问惹）
EXPOSE 80 443

# 优化配置
RUN echo "server_tokens off;" >> /etc/nginx/nginx.conf  # 隐藏Nginx版本号

# 启动命令 (ฅ´ω`ฅ) 是linux 略~
CMD ["nginx", "-g", "daemon off;"]  

# 使用tini作为init进程 (防止僵尸进程)
ENTRYPOINT ["/sbin/tini", "--"]
```

---
#### **3. 构建镜像**
```bash
docker build -t myblog .  # 构建一个叫myblog的容器！
```
>  **小贴士**：镜像名可以改成 `kawaii-blog` 之类的你想要的名称 (๑•̀ㅂ•́)و✧

---
#### **4. 运行容器**
```bash
docker run -d \ # 把本机的80端口和容器的80端口贴贴~
  -p 80:80 \  
  --name myblog_container \  # 容器名字！
  -v /www/dist:/usr/share/nginx/html:ro \  # 绑定本地文件（只读更安全）
  myblog  # 刚才构建的镜像名
```
**效果**：
访问 `http://localhost` 就能看到博客 (ﾉ>ω<)ﾉ

---
#### **5. 日常调教指令**
|**操作**|**命令**| **颜文字**           |
| ------ | ------------------------------------- | ----------------- |
|暂停容器|`docker stop myblog_container`| (´-ω-｀) 休眠中...    |
|启动容器|`docker start myblog_container`| ٩(◕‿◕｡)۶ 唤醒成功！    |
|查看日志|`docker logs -f myblog_container`| (｀・ω・´) 暗中观察      |
|进入容器|`docker exec -it myblog_container sh`| ฅ^•ﻌ•^ฅ 潜入！       |
|删除容器|`docker rm -f myblog_container`| (;´༎ຶД༎ຶ`) 坏掉了... |

---
#### **6. 更新博客内容**
```bash
# 只要更新/www/dist里的文件...
docker restart myblog_container  # 然后重启容器就好！
```

---
#### **7. 镜像打包分享**
```bash
# 导出镜像（把容器打包带走~）
docker save myblog > myblog.tar  # 得到一个.tar文件

# 在其他机器导入
docker load -i myblog.tar 
```

---
## **💖 Rancde专属提示**
### 1. 给容器加个粉色主题：
```bash
RUN apk add --no-cache lolcat && echo "kawaii~" | lolcat
```
### 2. 监控容器状态：
```bash
watch -c -n 1 "docker ps | lolcat"  # 彩色监控！
```
### 3. 错误处理：
```bash
docker logs myblog_container 2>&1 | grep -i "error" | cowsay -f tux  # 用牛牛提示错误
```

[![GitHub Stars](https://img.shields.io/github/stars/Rancade/server-guardian?style=social)](https://github.com/Rancade/install_Docker)
