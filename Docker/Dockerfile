# 基础镜像：轻量级Nginx（Alpine版）
FROM nginx:alpine  #最small >_<

# 安全加固 (๑•̀ㅂ•́)و✧
RUN apk add --no-cache tini && \
    rm -rf /usr/share/nginx/html/* && \
    chown -R nginx:nginx /var/cache/nginx

# 配置优化
COPY nginx.conf /etc/nginx/nginx.conf
COPY mime.types /etc/nginx/mime.types

# 部署静态文件
COPY --chown=nginx:nginx dist/ /usr/share/nginx/html/

# 露出80端口
EXPOSE 80 443

# 使用tini作为init进程 (防止僵尸进程)
ENTRYPOINT ["/sbin/tini", "--"]
