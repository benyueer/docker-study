# 基础镜像
FROM centos:7

# 拷贝文件
COPY nginx.repo /etc/yum.repos.d/nginx.repo

# 在容器中运行命令
RUN yum install -y nginx

# 端口
EXPOSE 80

# 环境变量
ENV HOST=mynginx

# 启动
CMD ["nginx","-g","daemon off;"]
