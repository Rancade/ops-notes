# 🎀 博客容器化完全指南 (✿◕‿◕✿)

---

## 1. 初始化项目目录
```bash
mkdir -p /www/blog && cd /www/blog  # 创建项目目录
git init  # 版本控制是必须的desu~
```
## 2. 编写高效的Dockerfile
```bash
# 使用Alpine基础镜像 (轻量级Linux发行版)
FROM nginx:alpine

# 安全加固 (๑•̀ㅂ•́)و✧
RUN apk add --no-cache tini && \
    rm -rf /usr/share/nginx/html/* && \
    chown -R nginx:nginx /var/cache/nginx

# 配置优化
COPY nginx.conf /etc/nginx/nginx.conf
COPY mime.types /etc/nginx/mime.types

# 部署静态文件
COPY --chown=nginx:nginx dist/ /usr/share/nginx/html/

# 暴露端口
EXPOSE 80 443

# 使用tini作为init进程 (防止僵尸进程)
ENTRYPOINT ["/sbin/tini", "--"]
CMD ["nginx", "-g", "daemon off;"]
```
> Pro Tip: 记得配置正确的文件权限哦 chmod -R 755 dist/

---

## 3. 构建生产级镜像
```bash
docker build \
    --tag kawaii-blog:v1.0 \
    --build-arg ENV=production \
    --no-cache \
    .
```
## 4. 使用Docker Compose部署
```bash
version: '3.8'

services:
  blog:
    image: kawaii-blog:v1.0
    container_name: my-pretty-blog
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./dist:/usr/share/nginx/html:ro
      - ./logs:/var/log/nginx
    environment:
      - TZ=Asia/Tokyo
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 30s
      timeout: 5s
      retries: 3
```
启动命令：docker compose up -d (´･ω･`)

---

## 5. 高级运维指令表

| 操作 | 命令 | 说明 |
|------|---------|-------|
| 性能分析	| docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"  |	监控资源使用 |
| 优雅重启	| docker compose exec blog nginx -s reload  |	不中断服务重载配置 |
| 安全检查	| docker scan kawaii-blog:v1.0 | 扫描镜像漏洞 |
| 备份数据	| docker export my-pretty-blog > blog_backup.tar | 导出容器快照 |
| 版本回滚	| docker tag kawaii-blog:v1.0 kawaii-blog:latest | 灵活的版本控制 |

---

## 6. CI/CD集成示例 (.gitlab-ci.yml)
stages:
  - build
  - deploy

build_image:
  stage: build
  script:
    - docker build -t registry.gitlab.com/yourname/kawaii-blog .
    - docker push registry.gitlab.com/yourname/kawaii-blog
  only:
    - master

deploy_prod:
  stage: deploy
  script:
    - ssh server01 "docker pull registry.gitlab.com/yourname/kawaii-blog"
    - ssh server01 "docker-compose -f /www/blog/docker-compose.yml up -d"
  when: manual

---

## 🌸 特别技巧
### 1. 彩色日志查看：
```bash
docker logs --tail 100 -f my-pretty-blog | ccze -A 
```
### 2. 可爱监控面板：
```bash
watch -n 1 -c 'echo -e "🌸 容器状态 🌸\n" && docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Status}}\t{{.Ports}}" | lolcat'
```
### 3. 安全清理：
```bash
docker system prune -af --volumes | while read line; do echo -e "\e[35m$line\e[0m"; done
```
### 4. 快速进入容器：
```bash
alias blog-shell='docker exec -it my-pretty-blog sh -c "stty rows $LINES cols $COLUMNS; sh"'
```

> 记得经常备份数据哦～ (ﾉ◕ヮ◕)ﾉ*:･ﾟ✧